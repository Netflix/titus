# Agent

## Titus-executor
See the build instructions in the [repo](https://github.com/Netflix/titus-executor/blob/master/README.md#building).



# Titus Api Definitions
```
git clone https://github.com/Netflix/titus-api-definitions.git
cd titus-api-definitions
./gradlew clean build
```

# Titus Control Plane
```
git clone https://github.com/Netflix/titus-control-plane.git
cd titus-control-plane
./gradlew clean buildDeb -PidlLocal
```

**Note:** the `titus-api-definitions` git repo must be in the same root folder as `titus-control-plane` git repo in order to build the master and gateway components with `-PidlLocal`.
Running all builds will produce 5 debs under the build/distributions folder in each component.

For example the gateway deb can be found as follows:
```
$ ls titus-server-gateway/build/distributions/*.deb
titus-server-gateway/build/distributions/titus-server-gateway_0.0.1-1_all.deb
```
