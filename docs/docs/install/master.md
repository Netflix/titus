# Titus Control Plane

## Master

Run as ubuntu user

* add titus apt repo with `curl -s https://8095c452e9473a3fae3ea86a6f2572c2cde0d7b5ec63e84f:@packagecloud.io/install/repositories/netflix/titus/script.deb.sh | sudo bash`
* update apt repos with `sudo apt-get update`
* install java8 with `sudo apt-get install openjdk-8-jdk`
* install mesos with `sudo apt-get install mesos`
* Copy `titus-server-master/build/distributions/titus-server-master<version>.deb` to server
* Run `sudo dpkg -i titus-server-master<version>.deb` to install the debian

* Create `~/titusmaster.properties` with the properties:

```properties
titus.master.apiport=7001
titus.master.apiProxyPort=7001
titus.master.grpcServer.port=7104

titus.zookeeper.connectString=<ZK_IP>:<ZK_PORT>
titus.zookeeper.root=/titus/main

mesos.master.location=<MESOS_MASTER_IP>:<MESOS_MASTER_PORT>

titus.agent.fullCacheRefreshIntervalMs=10000
titus.agent.agentServerGroupPattern=.*

titusMaster.job.configuration.defaultIamRole=<DEFAULT_IAM_ROLE_ARN>
titusMaster.job.configuration.defaultSecurityGroups=<SECURITY_GROUP_ID>

mesos.titus.executor=/apps/titus-executor/bin/titus-executor
```

* Start server with `sudo /opt/titus-server-master/bin/titus-server-master -p ~/titusmaster.properties | tee ~/titusmaster.log`

## Gateway

Run as ubuntu user

* update apt repos with `sudo apt-get update`
* install java8 with `sudo apt-get install openjdk-8-jdk`

* Copy `titus-server-gateway/build/distributions/titus-server-gateway<version>.deb` to server
* Run `sudo dpkg -i titus-server-gateway<version>.deb` to install the debian

* Create `~/titusgateway.properties` with the properties:

```properties
titus.masterClient.masterIp=<MASTER_IP>
```

* Start server with `sudo /opt/titus-server-gateway/bin/titus-server-gateway -p ~/titusgateway.properties | tee ~/titusgateway.log`
