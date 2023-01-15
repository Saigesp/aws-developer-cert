# AWS IAM - Identity and Access Management

Enables you to manage the access to AWS services and resources

**PD: This services usage is free**

## Introduction

- It works creating and configuring Resources, IAM Users, IAM Groups, IAM Roles, IAM Permissions and IAM Policies.
- IAM is a Global Service

## Best practices

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

## Login

https://custom_alias.signin.aws.amazon.com/console (alias created under IAM section)

## Users

Access type:
- Programatic access: Access ID and secret key
- Password access: User and password

## Policies

JSON stored control policies

#### Types:
- Identity Based Policies: Can be attached to a principal (user, group, role)
    - Managed Policies (AWS MAnaged, Customer Managed)
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

#### Notes

- You attach policies with required permissions to a role.
- You assume a role by calling the AWS Security Token Service (STS) AssumeRole API. These APIs return a set of temporally security credentials that application can use to sign requests to AWS.
- You can only associate one IAM Role with an EC2 instance at this time.
- You cannot associate an IAM Role to a IAM group.

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
