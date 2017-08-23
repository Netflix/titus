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
- dpkg -i titus-agent_0.0.1-1_all.deb

# titus-vpc-driver
- dpkg -i --force-overwrite titus-vpc-driver_0.0.1-1_all.deb
  - We use `--force-overwrite` flag to ensure all files are updated.
- Run `sudo /apps/titus-vpc-network-driver/bin/run` to start the driver. Since all of the other
components are in the host network, it should be fine to run it from the host.

# titus-metadata-service
- dpkg -i titus-metadata-service_0.0.1-1_all.deb
- Run `sudo /apps/titus-metadata-service/bin/run` to start the proxy
