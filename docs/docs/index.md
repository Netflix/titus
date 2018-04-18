# Titus

Titus is a container management platform that provides scalable and reliable container execution and
cloud-native integration with Amazon AWS. Titus was built internally at Netflix and is used in production
to power Netflix streaming, recommendation, and content systems.

Titus provides the following:

- __A production ready container platform__. Titus is run in production at Netflix, managing thousands of AWS EC2
instances and launching hundreds of thousands of containers daily for both batch and service workloads.
- __Cloud-native integrations with AWS__. Titus integrates with AWS services, such as VPC networking, IAM and Security Group
concepts, Application Load Balancing, and EC2 capacity management. These integrations enable many cloud
services to work seamlessly with containers.
- __Netflix OSS integration__. Titus works natively with many existing Netflix OSS projects, including
[Spinnaker](https://www.spinnaker.io/), [Eureka](https://github.com/Netflix/eureka),
[Archaius](https://github.com/Netflix/archaius), and [Atlas](https://github.com/Netflix/atlas) among others.
- __Docker-native container execution__.  Titus can run images packaged as Docker containers while providing
additional security and reliability around container execution.

## Architectural Overview
An overview of how Titus works is provided [here](overview.md).

## Getting Started

You can find out about building Titus [here](build.md) and deploying [here](install/prereqs.md).

### Learn More About Titus

- [Titus: Introducing Containers to the Netflix Cloud](https://queue.acm.org/detail.cfm?id=3158370)
- [The Evolution of Container Usage at Netflix](https://medium.com/netflix-techblog/the-evolution-of-container-usage-at-netflix-3abfc096781b)
- [Running Container at Scale at Netflix](https://www.slideshare.net/aspyker/container-world-2018)
- [A Series of Unfortunate Container Events](https://www.infoq.com/news/2017/07/netflix-titus)

### Contributing

Refer to [contibuting.md](contributing.md)

Join us on [slack](https://titusoss.herokuapp.com/)

### License

Copyright 2018 Netflix, Inc.

Licensed under the Apache License, Version 2.0 (the “License”); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an “AS IS” BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

