# AWS Elastic Beanstalk

Automation service for deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications.

## Advanced environment customization with configuration files (.ebextensions)

You can add AWS Elastic Beanstalk configuration files (`.ebextensions`) to your web application's source code to configure your environment and customize the AWS resources that it contains. Configuration files are YAML- or JSON-formatted documents with a `.config` file extension that you place in a folder named **.ebextensions** and deploy in your application source bundle.

Example:
```yaml
option_settings:
  aws:elasticbeanstalk:environment:
    LoadBalancerType: network
```

To define a [VPC](VPC.md):

``` yaml
namespace: aws:ec2:vpc
    option_name: VPCId
    value: vpc-170647c
```

## Deployment Strategies

- **All-at-Once**
    - Performs in place deployment on all instances.
- **Rolling**
    - Splits the instances into batches and deploys to one batch at a time.
- **Rolling with additional batch**
    - Splits the deployments into batches but for the first batch creates new EC2 instances instead of deploying on the existing EC2 instances.
- **Immutable**
    - If you need to deploy with a new instance instead of using an existing instance.
    - If the new instances don't pass health checks, Elastic Beanstalk terminates them, leaving the original instances untouched.
- **Traffic Splitting**
    - Performs immutable deployment and then forwards percentage of traffic to the new instances for a pre-determined duration of time. If the instances stay healthy, then forward all traffic to new instances and shut down old instances.

### Deployment Strategies Matrix

| Strategy    | ECS | Lambda | EC2/On-Premise |
| ----------- | --- | ------ | -------------- |
| All-at-Once | ✅  |   ✅   |      ❌        |
| In-Place    | ✅  |   ✅   |      ✅        |
| Blue/Green  | ✅  |   ✅   |   Only EC2     |
| Canary      | ✅  |   ✅   |      ❌        |
| Linear      | ✅  |   ✅   |      ❌        |

