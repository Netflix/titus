# Master

Run as ubuntu user

* update apt repos with `sudo apt-get update`
* install java8 with `sudo apt-get install openjdk-8-jdk`
* install mesos
```
sudo wget http://repos.mesosphere.com/ubuntu/pool/main/m/mesos/mesos_1.0.1-2.0.94.ubuntu1604_amd64.deb
sudo dpkg -i mesos_1.0.1-2.0.94.ubuntu1604_amd64.deb
sudo apt-get install -f
```
* Copy `titus-server-master/build/distributions/titus-server-master<version>.deb` to server
* Run `sudo dpkg -i titus-server-master<version>.deb` to install the debian

* Create `~/titus.properties` with the properties:
```
mantis.master.apiport=7001
mantis.master.apiProxyPort=7001
mantis.master.grpcServer.port=7104

mantis.zookeeper.connectString=<zookeeperIp>
mantis.zookeeper.root=/titus/main

mesos.master.location=<mesosMasterIp>:<mesosMasterPort>

titus.master.capacityManagement.instanceTypes.0.name=DEFAULT
titus.master.capacityManagement.instanceTypes.0.minSize=1

# Critical tier (0)
titus.master.capacityManagement.tiers.0.instanceTypes=m4.4xlarge,<INSTANCE_TYPE>
titus.master.capacityManagement.tiers.0.buffer=0.3

# Flex tier (1)
titus.master.capacityManagement.tiers.1.instanceTypes=m4.xlarge,<INSTANCE_TYPE>
titus.master.capacityManagement.tiers.1.buffer=0.3

titus.master.job.security.groups.default.list=<DEFAULT_SG>
mantis.master.framework.name=TitusFramework

```
* Start server with `sudo /opt/titus-server-master/bin/titus-server-master -p ~/titus.properties | tee ~/titus.log`

# Gateway

Run as ubuntu user

* update apt repos with `sudo apt-get update`
* install java8 with `sudo apt-get install openjdk-8-jdk`
* Copy `titus-server-gateway/build/distributions/titus-server-gateway_<version>.deb` to server
* Run `sudo dpkg -i titus-server-gateway_<version>.deb` to install the debian
* `export JAVA_OPTS=”-Dtitus.gateway.masterIp=<ip> -Dtitus.gateway.masterHttpPort=<port>”`
* Start server with `sudo /opt/titus-server-gateway/bin/titus-server-gateway | tee ~/titus.log`

