# Mesos-slave

Ran mesos-slave with Docker image instead of installing natively.
- TODO - need to do this without Docker
- `docker run --rm --name mesosslave -v /apps/:/apps/ -v /etc/titus-executor/:/etc/titus-executor/ -v /var/run/docker.sock:/var/run/docker.sock --privileged --net=host -d mesosphere/mesos-slave:1.0.1-2.0.93.ubuntu1404 mesos-slave --master=zk://172.31.43.195:2181/titus/mainvpc/mesos --log_dir=/var/log/mesos/ --work_dir=/apps/mesos --logging_level=INFO --resources="network:1000" --attributes="region:hackday;asg:titusagent-m4.xlarge;stack:hackday;zone:hackdayd;itype:m4.xlarge;cluster:titusagent-hackday;id:l-deadbeef;res:ResourceSet-ENIs-7-29"`

# titus-executor
- Change registry URL in config.test.json to “registry.hub.docker.com/library”
- This allows simple Dockerhub images to be used for testing.

# titus-vpc-driver
- Created a hackday_environment.sh that populates env vars based on the stock values (e.g., NFLX_ACCOUNT)
- Hard coded values from the host being used (e.g., EC2_INSTANCE_ID or EC2_ENI_ID).
- Changed the “run” script to source this file instead of nflx_environment.sh.
- TODO: Convert from the stash snippet
- Ones that matter: export TITUS_REGISTRY="registry.hub.docker.com", 
- Set env var TITUS_REGISTRY to export TITUS_REGISTRY="registry.hub.docker.com"
- Changed the pause image to use to imageName = "kubernetes/pause:latest"
