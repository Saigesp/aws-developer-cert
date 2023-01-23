# AWS CloudFormation

Service that helps you model and set up your AWS resources (Infraestructure as a Service).

You create a template that describes all the AWS resources that you want, and CloudFormation takes care of provisioning and configuring those resources for you.

## Template

JSON or YAML file that contains all the configuration details of the AWS resources to be provisioned (stack).

### Common sections
- Format version
- Description
- Metadata
- Resources
- Parameters
- Mappings
- Conditions
- Transforms
- Outputs

### Template vs SAM (Serverless Application Model)

SAM uses CloudFormation as the underlying deployment mechanism, so you can imagine SAM as an extended form of CloudFormation. SAM makes Serverless/Lambda deployments easier.

CloudFormation can deploy lambda scripts using inline scripts but it has a limitation of 4096 characters and you cannot pack custom dependencies, python libraries, etc, so to make Lambda/Serverless deployments easy SAM is used.

## Stack

Collection of AWS resources.

## Infraestructure updates

Anyone with stack update permissions can ull the resources in the stack.

Updates can be cancelled if the stack is still in UPDATE_IN_PROGRESS state.

Cancelling an update rolls back to the previous configuration.

### Update types
- **Direct update**
    - Changes deployed immediately.
    - Used for quick deployments.
- **Via Change Set**
    - Changes can be previewed before confirming them.
    - Used to ensure the intentional changes.

## Infraestructure deletion policy

Attribute in the template to specify if a resource should be preserved (Retain), deleted (Delete) or backed-up (Snapshot) when the stack is deleted.