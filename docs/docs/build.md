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

# Titus Api Definitions
```
git clone https://github.com/Netflix/titus-api-definitions.git
cd titus-api-definitions
./gradlew clean build
```

# Titus Master
```
git clone https://github.com/Netflix/titus-control-plane.git
cd titus-control-plane
./gradlew clean buildDeb -PidlLocal
```

# Titus Gateway
```
git clone https://github.com/Netflix/titus-control-plane.git
cd titus-control-plane
./gradlew clean buildDeb -PidlLocal
```

This will produce product three deb files under build/distributions
