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

## Inmutable deployments

Immutable deployments perform a immutable update to launch a full set of new instances running the new version of the application in a separate Auto Scaling group, alongside the instances running the old version. Immutable deployments can prevent issues caused by partially completed rolling deployments.

If the new instances don't pass health checks, Elastic Beanstalk terminates them, leaving the original instances untouched.