# Master

# Gateway

Run as root

- install java8
  - `sudo apt-get update`
  - `sudo apt-get install openjdk-8-jdk`
  - Copy titus-server-gateway/build/distribute/titus-server-gateway_<version>.deb to server
  - Run `dpkg -i titus-server-gateway_<version>.deb`
  
Run as Ubuntu user

- `export JAVA_OPTS=”-Dtitus.gateway.masterIp=<ip> -Dtitus.gateway.masterHttpPort=<port>”`
- Start server with `./opt/titus-server-gateway/bin/titus-server-gateway | tee ~/titus.log`

