# Required
## Linux
We currently run our agents on Ubuntu Xenial (16.04.2) with kernel 4.9.34 with
patches for EFS. Until Titus open source supports EFS, it is likely that Xenial
and a 4.9 kernel is sufficient.

We run all other tiers of Titus on Trusty.

# Mesos and Zookeeper
- Zookeeper 3.4.8 (with Exhibitor 1.5.5)
- Mesos 1.0.1

We run a high availability configuration of Mesos and Zookeeper. You can run this
on a node using Docker in a non-production mode by using:

```
sudo docker run -d -p 2181:2181 --name zookeeper jplock/zookeeper:3.4.10
sudo docker run -d -p 5050:5050 --name mesomaster mesosphere/mesos-master:1.0.1-2.0.93.ubuntu1404 mesos-master --zk=zk://localhost:2181/titus/mainvpc/mesos --work_dir=/tmp/master --log_dir=/var/log/mesos --logging_level=INFO --quorum=1
```

You can see the key ports for these are 2181 (Zookeeper) and 5050 (Mesos).

# Spinnaker
We deploy Titus via Spinnaker to an EC2 cloud provider. Eventually we may
release these pipelines, but for now we suggest direct deployments using
the AWS EC2 console.

# Docker Registry
We operate a docker registry based on Docker Distribution. We have tested with
Dockerhub and suggest this or the EC2 Container Registry Service (ECR).

# AWS Configuration

- IAM Role and security group for Titus
  - TODO: specify what is required, for now tested with wide open security group
  and ``*`` IAM role. 
- IAM Role and security group for app container
  - TODO: Need to specify a test sec group and IAM role

# Optional
We use Cassandra as our persistence store for Titus master. For OSS, we
current suggest the in-memory store.

We have integration with Eureka to disable agents and monitor health. For now,
this is not part of the OSS implementation.

We write all task state updates to Elastic Search for operational insight. For
now, this is not part of the OSS implementation.

We use Edda to understand which ASG's are available in Titus. For now, this is
not part of the OSS implementation.