# AWS CodeDeploy

Deployment service that automates application deployments to [EC2](EC2.md), on-premises instances, [Lambda](Lambda.m2) functions, or [ECS](ECS.md) services.

#### Notes:
- Artifacts must be compressed `.zip`, `.tar` or `.tar.gz` files.
- Instances are selected on CodeDeploy groups by its tags.

#### Deplyment types
- **In-place deployment**: The application on each instance is on the deployment group is stopped, the latest application revision is installed and the new version is started and validated. Supported only on EC2 and On-premises. Configs:
    - CodeDeployDefault.AllAtOnce
    - CodeDeployDefault.HalfAtATime
    - CodeDeployDefault.OneAtATime
- **Blue/Green deployment**: Provisions new compute platforms and when the deployment is successfully finished it reroutes the traffic from old environment (blue) to the new environment (green). Supported on all platforms.

## Deployment group

Deployment configuration, type, rollbacks, triggers and alarms.

## CodeDeply Agent

Agent that must be installed on EC2/On-prem instances for use of codeDeploy.

When the CodeDeploy agent is installed, a configuration file is placed on the instance. The configuration settings include:
- **:log_aws_wire:** : If logs must be caputed.
- **:log_dir:**: Where to put the logs files.
- **:pid_dir:**: Where to place the process ID.
- **:program_name:**: agent program name. Defaults to `codedeploy-agent`.
- **:root_dir:**: root dir.
- **:wait_between_runs:**: Time in seconds between polling for pending deployments. Defaults to 1.
- **:on_premises_config_file:**: For Onprem, the path to an alternate location for the configuration file.
- **:proxy_uri:**: (Optional). The proxy through which you want the agent to connect to AWS.
- **:max_revisions:**: (Optional). The number of revisions that you want to archive.
- **:enable_auth_policy:**: (Optional). Set to true if you want to use IAM authorization to configure access control. Must be true when using on a VPC.

## Rollback

When there is a deployment phase with a **content overwrite option as Overwrite**, it will delete all files from the previous deployment session. If this deployment fails, to restore to the previous working deployment, either Manual or Automatic rollback must be enabled. Also, deleted files as a part of the failed deployment phase **need to be added manually**.

## App spec (appspec.yml)

Collection of commands in YAML used to run a deploy.

Can be stored on the project or on S3.

#### Syntax

```yaml
version: 0.0
os: linux
files:
  - source: build
    destination: /var/www/html/

Resources:
  # EC2/On-Prem doesnt have resources
  
  # ECS
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "{arn-task-definition}"
        LoadBalancerInfo:
          ContainerName: "SampleApplicationName"
          ContainerPort: 80
  # Lambda
  - myLambdaFunction:
      Type: AWS::Lambda::Function
      Properties:
        Name: "myLambdaFunction"
        Alias: "myLambdaFunctionAlias"
        CurrentVersion: "1"
        TargetVersion: "2"

Hooks: # These are ONLY the available scripting hooks
  # EC2/On-Prem
  - ApplicationStart: "..."
  - ApplicationStop: "..."
  - BeforeInstall: "..."
  - AfterInstall: "..."
  - BeforeBlockTraffic: "..."
  - AfterBlockTraffic: "..."
  - BeforeAllowTraffic: "..."
  - AfterAllowTraffic: "..."
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