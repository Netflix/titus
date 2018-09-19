# Overview

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** |  |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Cost to Implement** |  |  |  |  |  |  |
|  **Extensibility** |  |  |  |  |  |  |
|  **Integration Complexity** |  |  |  |  |  |  |
|  **Largest Known Cluster** |  |  |  |  |  |  |
|  **On-premise/multicloud** |  |  |  |  |  |  |
|  **Ongoing Operational Cost** |  |  |  |  |  |  |
|  **Open Source Community/Project Health** |  |  |  |  |  |  |
|  **Performance** |  |  |  |  |  |  |

#  Detailed Evaluation

## Open Source Community/Project Health

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** | **Comments** |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Open Source Community/Project Health** |  |  | Best in Show |  |  |  |
|  feature velocity |  |  |  |  |  | How quickly features are being added to the platform |
|  first derivative of feature velocity (acceleration) |  |  |  |  |  | Is the feature velocity increasing or decreasing? |
|  time to respond to bug reports |  |  |  |  | remains to be seen |  |
|  time to respond to PRs |  |  |  |  | remains to be seen |  |
|  accurate documentation |  |  |  |  | currently poor, expected to get better |  |
|  what are the community's core values |  |  |  |  | Developer Experience Reliability and Availability Cost savings | do they line up with our own? |
|  what language is it written in? | Go | Go | Go | C++ | Go, Java |  |
|  Open Source licensing |  |  |  |  |  |  |

## Extensibility

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** | **Comments** |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Extensibility** |  |  | Designed to be extensible. Lots of integration points. |  | current v2 is not so great. upcoming v3 api has a lot more pluggability points |  |
|  container runtime |  |  | Anything that complies with the open Container Runtime Interface (docker, rkt, et al) |  | 2 quarters ago Titus looked for a container runtime that was more reliable than Docker (lockups, orphans). TL;DR - Titus remains Docker only. The competition had problems.<br/>Rocket seems like the right technical direction, but the community is not yet strong enough.<br/>They continue looking at RedHat's OCID and kubelets, and decided to do 'the best that we can with Docker' because that is where the state of the art is. RedHat has 8 FTEs working on OCID.<br/>Waiting for that space to mature before switching out the container runtime. |  |
|  service discovery |  |  |  |  | Service Discovery agnostic, containers register themselves |  |
|  autoscaling |  |  |  |  |  |  |
|  overlay network |  |  | AWS VPC CNI or Lyft VPC CNI |  |  |  |
|  security |  |  |  |  |  |  |

## Cost to Implement

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** | **Comments** |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Cost to Implement** |  |  |  |  |  |  |
|  Cluster Setup |  |  | kops or EKS |  | Probably less than 1 sprint's worth of chef work? Current packages are built for ubuntu, which might be tricky. |  |
|  Control Plane HA |  | out of the box | EKS |  |  |  |
|  Make Platform Services available to all containers |  |  | DaemonSet |  |  |  |
|  Configuration Integration |  |  | use ConfigMap? |  |  |  |
|  Integrate with custom Service Discovery |  |  |  |  |  |  |
|  Security Group per Container |  | included out of the box, via API | See Pinterest notes |  | included out of the box, via API |  |
|  IAM Role per Container |  | included out of the box, via API | kube2iam, configures a temporary IAM Role per Pod |  |  |  |
|  Healthcheck Integration |  |  |  |  |  |  |
|  SPIFFE integration |  |  | SPIFFEE is part of the CNCF, integration likely through Envoy. |  |  |  |
|  Bin Packing |  |  | Optimization through custom Scheduler |  | lots of features for this kind of optimization planned in 2018 |  |
|  Task/Service Autoscaling |  |  |  |  |  |  |
|  Volume Management (stateful services) |  |  |  |  | No EBS. Only EFS and ephemeral storage. |  |
|  Cost attribution back to product teams |  |  |  |  | their cost attribution greps through elastic search logs. we could extend that functionality through this interface. |  |
|  Secrets |  |  |  |  | titus injects credentials into containers while they are launching. container then pulls those from a system similar to vault. credentials are not currently rotated. |  |
|  Logging Integration |  |  | Can use any docker compatible log driver. One option is a fluentd container in a DaemonSet. |  |  |  |
|  SSL Certificate Management |  |  |  |  |  |  |
|  Promising upcoming features? |  | Fargate | metaparticle |  |  |  |
|  Paid help available? |  |  |  |  |  |  |
|  Free help available? |  |  |  |  |  |  |

## Ongoing Operational Cost

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** | **Comments** |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Ongoing Operational Cost** |  |  |  |  | How do these costs scale to 2x, 5x, 10x, 100x cluster sizes?<br/>For each resource what is the scaling factor for roles, engineers, hosts, clusters? Do these scale linearly or is there hidden exponential growth in one of these factors? |  |
|  Manpower |  |  |  |  |  |  |
|  Cluster Autoscaling |  | AWS Auto Scaling Groups | AWS Auto Scaling Groups |  | AWS Auto Scaling Groups |  |
|  Computational Overhead |  |  |  |  |  |  |
|  Software Licensing |  |  |  |  |  |  |
|  Auditing/Reporting/Chargeback |  |  |  |  |  |  |
|  Security |  |  |  |  |  |  |
|  Volume Discounting |  |  |  |  |  |  |

## Integration Complexity

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** | **Comments** |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Integration Complexity** |  |  |  |  |  |  |
|  Cluster Setup |  |  | kops or Amazon EKS are the two contenders |  | see: https://netflix.github.io/titus/install |  |
|  Custom deployment logic |  |  |  |  | Would mostly use the Jobs API |  |
|  **Cluster Management** |  |  |  |  |  |  |
|  visibility container metrics host metrics system metrics |  |  |  |  | see healthchecks |  |
|  scaling |  |  |  |  |  |  |
|  load balancing |  |  |  |  | uses AWS Autoscaling Groups for Titus nodes. Netflix internally uses Spinnaker for this. |  |
|  scheduling strategies |  |  |  |  |  |  |
|  Architectural Complexity |  |  |  |  | How many administrative roles/nodes are required for HA? |  |
|  Upgrade cost |  |  |  |  |  |  |
|  Multi-account/multi-tenant |  |  |  |  |  |  |
|  How easy to migrate to a different implementation |  |  |  |  | externalized to Spinnaker at Netflix.<br/>See Titus Features> Task (Container) Migration<br/>Updating the underlying Mesos and ZooKeeper is tricky. |  |

## Performance

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** | **Comments** |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Performance** |  |  |  |  |  |  |
|  TCP |  |  |  |  |  |  |
|  UDP |  |  |  |  |  |  |
|  CPU |  |  |  |  |  |  |
|  thread scheduling |  |  |  |  |  |  |
|  resource isolation |  |  |  |  |  |  |
|  quota |  |  |  |  |  |  |

## Other

|   | **Swarm** | **ECS** | **k8s** | **Mesos** | **Titus** | **Comments** |
|  :------ | :------ | :------ | :------ | :------ | :------ | ------ |
|  **Largest known cluster** |  |  | 2,000 nodes. requires simplified scheduling that doesn't include lots of constraints. |  |  |  |
|  being used in production? |  |  |  |  | 7 clusters inside Netflix |  |
|  what logos? what workloads? |  |  | GitHub (api and web)<br/>Alibaba (transaction processing) |  |  |  |
|  **Support for other environments** |  |  |  |  | Netflix is "all in" with AWS, so it would be difficult for to extend the system in this way. |  |
|  On Premise |  |  |  |  |  |  |
|  Multicloud |  |  |  |  |  |  |
