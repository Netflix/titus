# Upgrade Ubuntu
```
sudo su
apt-get update
apt-get upgrade
apt-get dist-upgrade
```

# Install Docker
Install docker as instructed [here](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository)

# Mesos-slave
Ran mesos-slave with Docker image instead of installing natively.
- TODO - need to do this without Docker
- `docker run --rm --name mesosslave -v /apps/:/apps/ -v /etc/titus-executor/:/etc/titus-executor/ -v /var/run/docker.sock:/var/run/docker.sock --privileged --net=host -d mesosphere/mesos-slave:1.0.1-2.0.93.ubuntu1404 mesos-slave --master=zk://172.31.43.195:2181/titus/mainvpc/mesos --log_dir=/var/log/mesos/ --work_dir=/apps/mesos --logging_level=INFO --resources="network:1000" --attributes="region:hackday;asg:titusagent-m4.xlarge;stack:hackday;zone:hackdayd;itype:m4.xlarge;cluster:titusagent-hackday;id:l-deadbeef;res:ResourceSet-ENIs-7-29"`

# titus-executor
- Take the built titus-executor_linux_amd64 binary and put it on the host at /apps/titus-executor/bin/titus-executor
- Take the testrun script from /test and put it on the host at /apps/titus-executor/bin/run
  - You will need to update the line `export ZKHOSTS=127.0.0.1:2181` to point to your ZK server
  - Ensure run is executable
- Take the config file from /test/config/config.test.json and put it on the host at /etc/titus-executor/config.json
  - Ensure the registry is updated or use the dockerhub link of “registry.hub.docker.com/library”
- TODO: use gradle to create deb files

# titus-vpc-driver
- Created a hackday_environment.sh that populates env vars based on the stock values (e.g., NFLX_ACCOUNT)
- Hard coded values from the host being used (e.g., EC2_INSTANCE_ID or EC2_ENI_ID).
- Changed the “run” script to source this file instead of nflx_environment.sh.
- TODO: Convert from the stash snippet
- Ones that matter: export TITUS_REGISTRY="registry.hub.docker.com", 
- Set env var TITUS_REGISTRY to export TITUS_REGISTRY="registry.hub.docker.com"
- Changed the pause image to use to imageName = "kubernetes/pause:latest"
