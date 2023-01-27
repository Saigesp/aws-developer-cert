# AWS CloudFormation

Service that helps you model and set up your AWS resources (Infraestructure as a Service).

You create a template that describes all the AWS resources that you want, and CloudFormation takes care of provisioning and configuring those resources for you.

## Template

JSON or YAML file that contains all the configuration details of the AWS resources to be provisioned (stack).

### CloudFormation vs SAM (Serverless Application Model)

[SAM](SAM.md) uses CloudFormation as the underlying deployment mechanism, so you can imagine [SAM](SAM.md) as an extended form of CloudFormation. SAM makes Serverless/Lambda deployments easier.

CloudFormation can deploy lambda scripts using inline scripts but it has a limitation of 4096 characters and you cannot pack custom dependencies, python libraries, etc, so to make Lambda/Serverless deployments easy [SAM](SAM.md) is used.

### Template sections

- **Resources** (required).
    - Stack resources and their properties.
- **Format Version** (optional).
    - The version that the template conforms to.
- **Description** (optional).
- **Metadata** (optional).
    - Objects that provide additional information about the template.
- **Parameters** (optional).
    - Values to pass to your template at runtime. You can refer them from Resources and Outputs.
- **Rules** (optional).
    - Validates a parameter or a combination to a template during stack creation or update.
- **Mappings** (optional).
    - A mapping of keys and associated values that you can use to specify conditional parameter values.
- **Conditions** (optional).
    - Conditions that control whether certain resources are created or whether certain resource properties are setted during stack creation or update.
- **Transform** (optional, required with SAM).
    - Specifies the version of [SAM](SAM.md) to declare resources in your template.
    - You can also use `AWS::Include` transforms to work with template snippets that are stored separately in [S3](S3.md).
- **Outputs** (optional).
    - Returned values when you view your stack's properties.

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

Attribute in the template to specify what happens when the stack is deleted:
- Delete: deleted
- Retain: should be preserved
- Snapshot: backed-up