# Amazon Resource Name

Amazon Resource Names (ARNs) uniquely identify AWS resources. We require an ARN when you need to specify a resource unambiguously across all of AWS.

## ARN format

Basic format:
```
arn:{partition}:{service}:{region}:{accountId}:{resource-type}:{resource-id}
```

- partition
    - The partition in which the resource is located. A partition is a group of AWS Regions.
    - The following are the supported partitions:
        - aws - AWS Regions
        - aws-cn - China Regions
        - aws-us-gov - AWS GovCloud (US) Regions
- service
    - The service namespace that identifies the AWS product.
- region
    - The Region code.
- account-id
    - The ID of the AWS account that owns the resource, without the hyphens.
- resource-type
    - The resource type.
- resource-id
    - The resource identifier. This is the name of the resource, the ID of the resource, or a resource path.
    - Some resource identifiers include a parent resource (sub-resource-type/parent-resource/sub-resource) or a qualifier such as a version (resource-type:resource-name:qualifier).