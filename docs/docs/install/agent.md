# Using Cloud-init
We recommend that you use the following cloud-init config with Ubuntu 16.04. Just make sure you've added an IAM role to your profile: [cloud-init](cloud-init.yml). There are a few fields we recommend filling in for yourself here. For example, you should fill in your SSH keys, and not ours. Also, you should configure the `/etc/mesos-attributes.sh` config file that's included in the cloud-init to be suited for your instance. The mesos-specific configuration that needs to be adjusted is the `stack`, `asg`, and `Res` attributes. You will also need to edit the run_cmd section, and change `zk://172.31.0.136:2181/titus/mainvpc/mesos`, based on your Zookeeper configuration. The specifics of the `Res` attribute, and how it refers to how your ENIs work is explained below.

In order to use cloud-init, you need to go ahead and paste the linked yaml file into your Ubuntu 16.04 VM's user data.

# Manual Install
This is for advanced users only. We do not recommend using these instructions unless you're running a customized system. We also recommend looking at the cloud-init as the reference for canonical configuration files.


## Linux
We recommend Ubuntu Linux, Xenial, with a 4.4 or newer version for running Titus. We also recommend this for developing Titus agent itself. We recommend using XFS or Btrfs for the storage of the Docker daemon itself, and separating out the storage of the agent "Control Plane" components versus the non-control plane components. 

## Install Packagecloud Repo
Run the following commands:

```
curl -s https://8095c452e9473a3fae3ea86a6f2572c2cde0d7b5ec63e84f:@packagecloud.io/install/repositories/netflix/titus/script.deb.sh | sudo bash
apt-get update
```


## Install Docker
Install docker as instructed [here](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository)

* Add the current user to docker group `sudo gpasswd -a $USER docker`, log out and then log back in.

You can add the following drop-in systemd unit to configure Docker for your needs:

```
# /etc/systemd/system/docker.service.d/10-docker-config.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --log-level=debug -H fd:// --init-path=/apps/titus-executor/bin/tini-static --iptables=false --storage-driver=overlay2
Restart=always
StartLimitInterval=0
RestartSec=5
```

You should also configure the Docker TCP Proxy, with the following systemd units:
### docker-tcp-proxy.service
```
# /lib/systemd/system/docker-tcp-proxy.service
[Unit]
Description=Docker Unix Socket TCP Proxy
Requires=docker.socket
Requires=docker-tcp-proxy.socket
After=docker.socket
After=docker-tcp-proxy.socket

[Service]
ExecStart=/lib/systemd/systemd-socket-proxyd /var/run/docker.sock

[Install]
WantedBy=multi-user.target
```
### docker-tcp-proxy.socket
```
# /lib/systemd/system/docker-tcp-proxy.socket
[Unit]
Requires=docker.socket
After=docker.socket

[Socket]
ListenStream=0.0.0.0:4243

[Install]
WantedBy=sockets.target
```
Once you've done this, we recommend restarting docker, enabling these systemd units to restart on every boot.

# Mesos-agent
You can download Mesos from Mesosphere's OSS site here: https://open.mesosphere.com/downloads/mesos/#apache-mesos-1.1.3, we recommend installing 1.1.3. You can download the deb directly [http://repos.mesosphere.com/ubuntu/pool/main/m/mesos/mesos_1.1.3-2.0.1.ubuntu1604_amd64.deb](here).

Unfortunately, Mesos does not come with a systemd unit file out of the box, so we have one that you can adapt to your own needs:
```
# /lib/systemd/system/mesos-agent.service
[Unit]
Description=Mesos
Wants=docker.service
After=docker.service
Conflicts=halt.target shutdown.target sigpwr.target

[Service]
ExecStartPre=/bin/mkdir -p /var/lib/mesos
EnvironmentFile=/etc/mesos-agent.config
ExecStart=/usr/sbin/mesos-agent
ExecStopPost=/bin/rm -rf /var/lib/mesos/meta/slaves/latest
Restart=always
StartLimitInterval=0
RestartSec=5
LimitNOFILE=65535
KillMode=mixed
KillSignal=SIGUSR1

[Install]
WantedBy=multi-user.target
```

Our `/etc/mesos-agent.config` includes the following:

```
MESOS_MASTER=zk://1.1.1.1:2181,2.2.2.2:2181/titus/devvpc7/mesos
MESOS_PORT=7101
MESOS_RECOVER=reconnect
MESOS_WORK_DIR=/var/lib/mesos
MESOS_STRICT=false
MESOS_RESOURCES=ports:[7150-7200];mem:230400;disk:523264;network:9000
MESOS_ATTRIBUTES=region:us-east-1;asg:titusagent-devvpc7-r4.8xlarge-v072;stack:devvpc7;zone:us-east-1c;itype:r4.8xlarge;cluster:titusagent-devvpc7-r4.8xlarge;id:i-0d736710f99b183f1;res:ResourceSet-ENIs-7-29;distcodename:xenial;distrelease:16.04
```
Of course, adapt the configuration file to your needs. The Mesos attributes are broken down as below:

* region: AWS region
* asg: AWS ASG name
* stack: Which (isolated) Titus environment
* zone: Which AWS availabilityzone
* itype: What AWS instance type
* cluster: This is an internal naming convention of titusagent-${stack}-${custom}
* id: EC2 instance identifier
* res: Special construction to signal to the Titus master how many ENIs this instance type can handle, for example "ResourceSet-ENIs-7-29", means that this machine can have 7 additional ENIs, and 29 IPs per ENI.


# VPC Driver
Install the VPC Driver from the repo. The name of the package is `titus-vpc-driver`.
* Add `net.ifnames=0` to GRUB_CMDLINE_LINUX= in `/etc/default/grub` and then run `sudo update-grub`.
* Remove the `/etc/udev/rules.d/70-persistent-net.rules` file and then run `sudo reboot`.
* After reboot, the default interface should be named `eth0` instead of the previous `ens3`.


# titus-executor
Install the VPC Driver from the repo. The name of the package is `titus-executor`.

You need to configure it with a configuration file at `/etc/titus-executor/config.json`. The configuration file looks like below:

```
{
  "stack": "mainvpc",
  "docker": {
    "host": "tcp://127.0.0.1:4243",
    "registry": "docker.io"
  },
  "uploaders": {
     },
  "env": {
    "copiedFromHost": [
      "NETFLIX_ENVIRONMENT",
      "NETFLIX_ACCOUNT",
      "NETFLIX_STACK",
      "EC2_INSTANCE_ID",
      "EC2_REGION",
      "EC2_AVAILABILITY_ZONE",
      "EC2_OWNER_ID",
      "EC2_VPC_ID",
      "EC2_RESERVATION_ID"
    ],
    "hardCoded": {
      "NETFLIX_APPUSER": "appuser",
      "EC2_DOMAIN": "amazonaws.com"
    }
  },
  "statusCheckFrequency": "10s",
  "useNewNetworkDriver": true,
  "usePrivilegedTasks": false,
  "useMetatron": true,
  "logsTmpDir": "/var/lib/titus-container-logs"
}
```