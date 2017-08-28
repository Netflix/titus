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

**Note:** the `titus-api-definitions` git repo must be in the same root folder as `titus-control-plane` git repo in order to build the master and gateway components with `-PidlLocal`.
Running all builds will produce 5 debs under the build/distributions folder in each component.
