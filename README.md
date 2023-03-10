# AWS Certifier Developer - Associate (DVA-C01)

AWS Certified Developer - Associate showcases knowledge and understanding of core AWS services, uses, and basic AWS architecture best practices, and proficiency in developing, deploying, and debugging cloud-based applications by using AWS. Preparing for and attaining this certification gives certified individuals more confidence and credibility. Organizations with AWS Certified developers have the assurance of having the right talent to give them a competitive advantage and ensure stakeholder and customer satisfaction.

[More info here](https://aws.amazon.com/certification/certified-developer-associate/?nc1=h_ls).

## What you'll learn

Preparing for and attaining this certification will showcase:

- Understanding of core AWS services, uses of the services, and basic AWS architecture best practices, including the AWS Shared Responsibility Model, application lifecycle management, and the use of containers in the development process
- Proficiency in developing, deploying, and debugging cloud-based applications using AWS and writing code for serverless applications
- Ability to identify key features of AWS services and use the AWS service APIs, AWS CLI, and SDKs to write applications
- Ability to apply a basic understanding of cloud-native applications to write code
- Ability to author, maintain, and debug code modules on AWS

## Main services and concepts

> Services with ⚡ are the most common on the exam

- Tools
    - [ARN](ARN.md)
    - [CLI](CLI.md)
    - [Cloud9](Cloud9.md)
- Compute
    - ⚡ [EC2](EC2.md)
    - ⚡ [AMI](EC2.md#ami)
    - ⚡ [Elastic Beanstalk](ElasticBeanstalk.md)
    - ⚡ [Lambda](Lambda.md)
    - [Athena](Athena.md)
- Containers
    - ⚡ [ECS](ECS.md)
    - ⚡ [ECR](ECR.md)
    - [Fargate](Fargate.md)
- Storage
    - ⚡ [S3](S3.md)
    - [Glacier](S3.md#storage-classes)
- Database
    - [Redshift](Redshift.md)
    - [RDS](RDS.md)
    - [Aurora](Aurora.md)
    - ⚡ [DynamoDB](DynamoDB.md)
    - ⚡ [ElastiCache](ElastiCache.md)
- CI/CD
    - [CodeCommit](CodeCommit.md)
    - ⚡ [CodeBuild](CodeBuild.md)
    - ⚡ [CodeDeploy](CodeDeploy.md)
    - ⚡ [CodePipeline](CodePipeline.md)
    - [CodeStar](CodeStar.md)
- Networking
    - ⚡ [API Gateway](APIGateway.md)
    - ⚡ [ELB](ELB.md)
    - ⚡ [VPC](VPC.md)
    - ⚡ [CloudFront](CloudFront.md)
    - ⚡ [Route 53](Route53.md)
    - ⚡ [Direct Connect](DirectConnect.md)
- Security, Identity & Compliance
    - ⚡ [IAM](IAM.md)
    - ⚡ [Cognito](Cognito.md)
    - ⚡ [KMS](KMS.md)
    - [CloudHSM](CloudHSM.md)
- Management & Governance
    - ⚡ [CloudWatch](CloudWatch.md)
    - ⚡ [CloudTrail](CloudTrail.md)
    - ⚡ [X-Ray](XRay.md)
    - ⚡ [CloudFormation](CloudFormation.md)
    - [SAM](SAM.md)
    - [Trusted Advisor](TrustedAdvisor.md)
- Analytics
    - ⚡ [Kinesis](Kinesis.md)
    - ES
- Integration
    - ⚡ [SNS](SNS.md)
    - ⚡ [SQS](SQS.md)
    - SES
    - [SWF](SWF.md)
    - ⚡ [Step Functions](StepFunctions.md)
    - EventBridge


## Secondary services

- **EKS**
    - Managed service to run Kubernetes.
- **CodeArtifact**
    - Managed artifact repository to store and share software packages.
- **CodeGuru**
    - Provides intelligent recommendations for Java applications.
- **Fault Injection Simulator**
    - Perform fault injection experiments on your AWS workloads (chaos engineering).
- **Secrets Manager**
    - Securely encrypt, store, and retrieve credentials for your databases and other services.
- **AppSync**
    - Managed GraphQL service.
- **Amplify**
    - Managed mobile backend & simple framework for iOS, Android, Web, and React Native.
- **Config**
    - Detailed view of the resources associated with your AWS account.
- **Elastic File System**
    - Managed file storage service (no need of provisioning or managing storage capacity).
- **Data Pipeline**
    - Automate the movement and transformation of data.
- **Inspector**
- **SSM Agent (Systems Manager Agent)**


## A repasar

- CodeDeploy rollback failures: https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-rollback-and-redeploy.html

- BeanStalk environment update: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-updating.html
- BeanStalk swap URL: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.CNAMESwap.html
- BeanStalk CLI: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html

- System Manager Parameter Store: https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html

- ECS Instances: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_instances.html

- Lambda lifecycle: https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html

- Step functions error handling: https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html

- Kinesis faqs: https://aws.amazon.com/kinesis/data-streams/faqs/
