# AWS CloudTrail

Service that helps you enable operational and risk auditing, governance, and compliance of your AWS account.

Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail.

## CloudWatch vs CloudTrail

| CloudWatch                   | CloudTrail                             |
| ---------------------------- | -------------------------------------- |
| Resource centered            | User centered                          |
| Tracks resources performance | Tracks IAM users activity on API calls |
| 1-5 minutes period           | 15 minutes                             |

## Trails

A trail is a configuration that enables delivery of events to an [S3](S3.md) bucket, [CloudWatch](CloudWatch.md) Logs, and [CloudWatch](CloudWatch.md) Events.

You can use a trail to:
- Filter the events you want delivered.
- Encrypt log files with an [KMS](KMS.md) key.
- Set up [SNS](SNS.md) notifications for log file delivery.

## Events

An event in CloudTrail is the record of an activity in an AWS account. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail.

### Management Events

Management events provide information about management operations that are performed on resources in your AWS account.

### Data Events

Data events provide information about the resource operations performed on or in a resource.

The following data types are recorded:

- [S3](S3.md) object-level API activity (GetObject, DeleteObject...) on buckets and objects in buckets.
- [S3](S3.md) on Outposts object-level API activity.
- [S3](S3.md) Object Lambda access points API activity.
- [S3](S3.md) API activity on access points.
- [Lambda](Lambda.md) function execution activity (the Invoke API).
- [DynamoDB](DynamoDB.md) object-level API activity on tables (PutItem, DeleteItem...).
- [DynamoDB](DynamoDB.md) API activity on streams.
- [EBS](EBS.md) direct APIs (PutSnapshotBlock, GetSnapshotBlock...).
- and more...

## Insights Events

unusual API call rate or error rate activity in your AWS account.

## Event History

Viewable, searchable, and downloadable record of the past **90 days** of CloudTrail events.