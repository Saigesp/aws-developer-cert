# SAM - Serverless Application Model

Open-source framework for build serverless applications on 

It's an extension of [CloudFormation](CloudFormation.md).

It consists of:
- SAM template specification
- SAM command line interface (SAM CLI)

Benefits of using AWS SAM
- **Single-deployment configuration**.
    - It makes it easy to organize related components and resources, and operate on a single stack.
    - You can deploy all related resources together as a single, versioned entity.
- **Extension of [CloudFormation](CloudFormation.md)**.
    - You can define resources using CloudFormation in your SAM template.
    - You can use the full suite of resources, intrinsic functions, and other features of CloudFormation.
- **Built-in best practices**.
    - You can use AWS SAM to define and deploy your infrastructure as config (so use code reviews).
- **Local debugging and testing**.
    - The CLI provides a Lambda-like execution environment locally to catch issues upfront 
- **Deep integration with development tools**.
    - [CodeBuild](CodeBuild.md), [CodeDeploy](CodeDeploy.md), [CodePipeline](CodePipeline.md) and [CodeStar](CodeStar.md).
    - [X-Ray](XRay.md).
    - [Cloud9](Cloud9.md).
    - Etc

## SAM template

Closely follows the format of an AWS CloudFormation template file. The primary differences are:

- **Transform declaration**.
    - The declaration `Transform: AWS::Serverless-2016-10-31` is required for AWS SAM template files.
- **Globals section**.
    - The `Globals` section is unique to AWS SAM. It defines properties that are common to all your serverless functions and APIs.
- **Resources section**.
    - In SAM templates the `Resources` section can contain a combination of CloudFormation resources and SAM resources.
- **Parameters section**.
    - Objects that are declared in the Parameters section cause the `sam deploy --guided` command to present additional prompts.