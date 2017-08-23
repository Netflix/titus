# Agent

```
mkdir -p $GOPATH/src/github.com/Netflix
cd $GOPATH/src/github.com/Netflix
git clone https://github.com/Netflix/titus-agent.git
cd titus-agent
make build
./gradlew clean buildDeb
```

# VPC Driver
```
mkdir -p $GOPATH/src/github.com/Netflix
cd $GOPATH/src/github.com/Netflix
git clone https://github.com/Netflix/titus-vpc-driver.git
cd titus-vpc-driver
go build
./gradlew clean buildDeb
```

# Metadata Service
```
mkdir -p $GOPATH/src/github.com/Netflix
cd $GOPATH/src/github.com/Netflix
git clone https://github.com/Netflix/titus-metadata-service.git
cd titus-metadata-service
make build
./gradlew clean buildDeb
```

This will produce product three deb files under build/distributions
