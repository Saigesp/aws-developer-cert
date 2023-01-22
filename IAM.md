# AWS IAM - Identity and Access Management

Enables you to manage the access to AWS services and resources

It works creating and configuring Resources, IAM Users, IAM Groups, IAM Roles, IAM Permissions and IAM Policies.

### General notes:

- IAM is a Global Service.
- This service usage is **free**.
- Principal: Users, Roles and AWS applications thath may use other services.
- Requests: Defined by actions, resources & principal who is doing the action.
- Policies: Based on identities or resources.
- Login point can be customized: https://custom_alias.signin.aws.amazon.com/console

## Users

[See documentation here](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html).

Access type:
- Programatic access: Access ID and secret key
- Password access: User and password

## Groups

It is preferable to create roles rather than groups.

## Policies

A policy is an object in AWS that, when associated with an identity or resource, defines their permissions. [See documentation here](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html).

### Policy types:

- Identity Based Policies: Can be attached to a principal (user, group, role)
    - Managed Policies (AWS Managed, Customer Managed)
    - Inline Policies
- Resource Based Policies: Can be attached to a resource (S3...). These are Inline Policies

#### Policy JSON format example:

```json
{
    "Version": "2020-10-23",
    "Statement": { // Can be array of objects -> OR
        "Sid": "LoremIpsum", // Statement identifier
        "Effect": "Allow",
        "Action": "dynamodb:*", // Can be array of strings
        "Resource": "arn:aws:dynamodb:us-east-2:235135135:table/Books", // Only Identity Based Policies
        "Principal": { // Only Resource Based Policies
            "AWS": "arn:aws:iam::754513654163:user/bob"
        },
        "Condition": "..."
    }
}
```

#### Policy Evaluation Logic:

1. Evaluate all policies
2. Is there a deny? -> Then deny
3. Is there an allow? -> Then allow
4. No deny nor allow? -> Then deny (Default deny)

## Roles

Used to delegate access with defined permissions to users, apps or services that don't normally have access to your AWS resources **without having to share long-term access keys**.

TO assume a role, an user must call [AssumeRole](#assumerole).

> **You can grant a role to an external user**. This is useful if you have many AWS accounts, because you can create users in only one AWS account and grant roles temporary in other accounts without register users again in these accounts.

#### Notes

- You attach policies with required permissions to a role.
- You assume a role by calling the AWS Security Token Service (STS) AssumeRole API. These APIs return a set of temporally security credentials that application can use to sign requests to AWS.
- You can only associate one IAM Role with an EC2 instance at this time.
- You cannot associate an IAM Role to a IAM group.
- The permissions of your IAM user and any roles that you assume are not cumulative. Only one set of permissions is active at a time. When you assume a role, you temporarily give up your previous user or role permissions and work with the permissions assigned to the role. When you exit the role, your user permissions are automatically restored.

### Definitions

- **Role**: a set of permissions that grant access to actions and resources in AWS. These permissions are attached to de role, not the user or group.
- **AWS service role**: A role that a service assumes to perform actions in your account in your behalf.
- **AWS service role for an EC2 instance**: A special type of service role that a service assumes to launch an AMazon EC2 instance that runs your application.
- **AWS service-linked role**: Predefined by a service and include all the permissions that the service requires to call other AWS services on your behalf.
- **Role chaining**: Role chaining occurs when you use a role to assume another role thought the AWS CLI or API.
- **Delegation**: The granting of permissions to someone.
- **Federation**: The creation of a trust relationship between an external identity provider and AWS.
- **Trust policy**: A JSON document in which you define who is allowed to assume the role
- **Permissions policy**: A JSON document in which you define what actions and resources the role can use.
- **Principal**: An entity that can perform actions (root user, IAM users, or a role).
- **Role for cross-account access**: Granting access to resources in one account to a trusted principal in a different account.

### When to use roles?

- Provide access for services offered by AWS to AWS resources.
- Provide access for an IAM user in one AWS account that you own to access resources in another account that you own.
- Provide access for externally authenticated users (identity federation).
- Provide access to IAM users in AWS accounts owned by third parties.

### Application access

To assume a role, an application calls the **AWS STS AssumeRole API** operation and passes the ARN of the role to use. When you call AssumeRole, you can optionally pass a JSON policy. This allows you to restrict permissions for that for the role's temporary credentials.

This is useful when you need to give the temporary credentials to someone else. They can use the role's temporary credentials in subsequent AWS API calls to access resources in the account that owns the role. 

You cannot use the passed policy to grant permissions that are in excess of those allowed by the permissions policy of the role that is being assumed.

## Access Analyzer

IAM Access Analyzer helps you identify the resources in your organization and accounts, such as Amazon S3 buckets or IAM roles, shared with an external entity.

#### Capabilities:

- Helps **identify resources** in your organization and accounts that are shared with an external entity.
- **Validates IAM policies** against policy grammar and best practices.
- **Generates IAM policies** based on access activity in your AWS CloudTrail logs.

#### It analyzes the following resource types:

- [S3](S3.md) buckets
- IAM roles
- [KMS](KMS.md) keys
- [Lambda](Lambda.md) functions and layers
- [SQS](SQS.md) queues
- [Secrets Manager](SecretsManager.md) secrets.
- [SNS](SNS.md) topics
- [EBS](EBS.md) volume snapshots
- [RDS](RDS.md) DB snapshots
- [RDS](RDS.md) DB cluster snapshots
- ECR repositories
- EFS file systems

## IAM database authentication

You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with:
- MariaDB
- MySQL
- PostgreSQL

With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token.

## AssumeRole

Returns a set of temporary security credentials that you can use to access AWS resources that you might not normally have access to. [See documentation here](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html).

Typically, you use AssumeRole within your account or for cross-account access.

These temporary credentials consist of:
- An access key ID
- A secret access key
- A security token

> The temporary security credentials created by AssumeRole can be used to make API calls to any AWS service **except AWS STS `GetFederationToken` and `GetSessionToken` API operations**.

### Using MFA with AssumeRole

You can include multi-factor authentication (MFA) information when you call AssumeRole. This is useful for cross-account scenarios to ensure that the user that assumes the role has been authenticated with an AWS MFA device.

In that scenario, the **trust policy of the role being assumed includes a MFA condition**. If the caller does not include valid MFA information, the request to assume the role is denied.

The condition in a trust policy that tests for MFA authentication might look like:

```json
"Condition": { "Bool": { "aws:MultiFactorAuthPresent": true } }
```

### Request Parameters

- **RoleArn** (Required): The Amazon Resource Name (ARN) of the role to assume.
- **RoleSessionName** (Required): An identifier for the assumed role session.
- **DurationSeconds** (Optional): The duration, in seconds, of the role session, from 900s (15 minutes) up to the maximum session duration set for the role. If you specify a value higher than this setting or the administrator setting (whichever is lower), the operation fails.
- **ExternalId** (Optional): A unique identifier that might be required when you assume a role in another account..
- **Policy** (Optional): An IAM policy in JSON format that you want to use as an inline session policy.
- **PolicyArns.member.N** (Optional): The Amazon Resource Names (ARNs) of the IAM managed policies that you want to use as managed session policies.
- **SerialNumber** (Optional): The identification number of the MFA device that is associated with the user who is making the AssumeRole call.
- **SourceIdentity** (Optional): The source identity specified by the principal that is calling the AssumeRole operation.
- **Tags.member.N** (Optional): A list of session tags that you want to pass.
- **TokenCode** (Optional): The value provided by the MFA device, if the trust policy of the role being assumed requires MFA.
- **TransitiveTagKeys.member.N** (Optional): A list of keys for session tags that you want to set as transitive.

## Best security practices

- Lock your AWS root user access keys
- Create individual IAM Users
- Configure a strong password policy (min-length, expiration)
- Rotate credentials regularly
- Remove unused credentials
- Enable MFA
- Assign permissions to groups instead of users
- Use AWS defined policies whenever possible
- Use policy conditions for extra security
- Grant least privileges
- Use roles to delegate permissions
- Monitor activities