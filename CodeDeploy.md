# AWS CodeDeploy

Deployment service that automates application deployments to [EC2](EC2.md), on-premises instances, [Lambda](Lambda.m2) functions, or [ECS](ECS.md) services.

#### Notes:
- Artifacts must be compressed `.zip`, `.tar` or `.tar.gz` files.
- Instances are selected on CodeDeploy groups by its tags.

#### Deplyment types
- **In-place deployment**: The application on each instaance is on the deplyment group is stopped, the latest application revision is installed and the new version is started and validated. Supported only on EC2 and On-premises. Configs:
    - CodeDeployDefault.AllAtOnce
    - CodeDeployDefault.HalfAtATime
    - CodeDeployDefault.OneAtATime
- **Blue/Green deplyment**: Provisions new compute platforms and when the deployment is successfully finished it reroutes the traffic from old environment (blue) to the new environment (green). Supported on all platforms.

## Deployment group

Deployment configuration, type, rollbacks, triggers and alarms.

## CodeDeply Agent

Agent that must be installed on EC2/On-prem instances for use of codeDeploy.

On EC2 its done through EC2 Configuration / Advanced details / User data (placing code)

## App spec (appspec.yml)

Collection of commands in YAML used to run a deploy.

#### Syntax

```yaml
version: 0.0
os: linux
files:
  - source: build
    destination: /var/www/html/

Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:us-east-1:111222333444:task-definition/my-task-definition-family-name:1"
        LoadBalancerInfo:
          ContainerName: "SampleApplicationName"
          ContainerPort: 80
        # Optional properties
        PlatformVersion: "LATEST"
        NetworkConfiguration:
          AwsvpcConfiguration:
            Subnets: ["subnet-1234abcd","subnet-5678abcd"]
            SecurityGroups: ["sg-12345678"]
            AssignPublicIp: "ENABLED"
        CapacityProviderStrategy:
          - Base: 1
            CapacityProvider: "FARGATE_SPOT"
            Weight: 2
          - Base: 0
            CapacityProvider: "FARGATE"
            Weight: 1

Hooks:
  # EC2/On-Prem (there are more depending on deployment type)
  - BeforeInstall: "..."
  - AfterInstall: "..."
  - ApplicationStart: "..."
  - ValidateService: "..."
  # ECS
  - BeforeInstall: "..."
  - AfterInstall: "..."
  - AfterAllowTestTraffic: "..."
  - BeforeAllowTraffic: "..."
  - AfterAllowTraffic: "..."
  # Lambda
  - BeforeAllowTraffic: "..."
  - AfterAllowTraffic: "..."
```