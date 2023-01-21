# AWS Fargate

Service that you can use with [ECS](ECS.md) to run containers without having to manage servers or clusters of [EC2](EC2.md) instances.

offers platform versions for Amazon Linux 2 and Microsoft Windows 2019 Server Full and Core editions.

## Task definitions

Amazon ECS tasks on AWS Fargate do not support all of the task definition parameters that are available.

The following task definition parameters are NOT valid in Fargate tasks:
- disableNetworking
- dnsSearchDomains
- dnsServers
- dockerSecurityOptions
- extraHosts
- gpu
- ipcMode
- links
- pidMode
- placementConstraints
- privileged
- systemControls

The following task definition parameters have limitations:
- linuxParameters
- volumes
- cpu

## Network mode

Its required taht the network mode is set to `awsvpc`, which provides each task with its own elastic network interface.