# Agent

## Titus-vpc-driver
See the build instructions in the [repo](https://github.com/Netflix/titus-vpc-driver/blob/master/README.md#building).
## Titus-executor
See the build instructions in the [repo](https://github.com/Netflix/titus-executor/blob/master/README.md#building).



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
