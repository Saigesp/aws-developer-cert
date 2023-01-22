# AWS CloudWatch

Real time monitoring service that collects and tracks metrics, that can be measured for your resources and applications.

Automatically displays metrics about every AWS service you use, and you can additionally create custom dashboards to display metrics and display custom collections of them.

You can also create metrics alarms to send notifications or automatically make changes to the resources you are monitoring when a threshold is breached.

Is often used alongside with [CloudTrail](CloudTrail.md), which monitors the calls made to CloudWatch API for your account. When CloudTrail logging is turned on, CloudWatch writes log files to the [S3](S3.md) bucket that you specified.

## CloudWatch vs CloudTrail

| CloudWatch                   | CloudTrail                             |
| ---------------------------- | -------------------------------------- |
| Resource centered            | IAM Users centered                     |
| Tracks resources performance | Tracks IAM users activity on API calls |
| 1-5 minutes period           | 15 minutes                             |

## How it works

CloudWatch is basically a metrics repository. An AWS service (such as EC2) puts metrics into the repository, and you retrieve statistics based on those metrics.

## Monitoring

- **Basic monitoring**
    - Enabled by default.
    - Data available automatically in 5 minutes periods at no charge.
- **Detailes monitoring**
    - Must be specifically enabled at instance level.
    - Data available automatically in 1 minute periods for an additional cost.

## Namespaces

A namespace is a container for CloudWatch metrics. Metrics in different namespaces are isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics.

The AWS namespaces typically use the following convention: `AWS/{service}`. For example, `AWS/EC2`.

## Metrics

A metric represents a time-ordered set of data points that are published to CloudWatch, like a variable to monitor where the data points represent the values of that variable over time.

By default, many AWS services provide metrics at no charge for resources.

Metrics exist only in the Region in which they are created. Metrics cannot be deleted, but they automatically expire after 15 months if no new data is published to them.

Metrics are uniquely defined by a name, a namespace, and zero or more dimensions. Each data point has a time stamp, and optionally a unit of measure.

### Metrics retention

CloudWatch retains metric data as follows:

| Data point period | Available time |
| ----------------- | -------------- |
| < 60 seconds      | 3 hours        |
| 60 seconds        | 15 days        |
| 300 seconds       | 63 days        |
| 3600 seconds      | 455 days       |

Data points that are initially published with a shorter period are aggregated together for long-term storage.

## Dimensions

A dimension is a name/value pair that is part of the identity of a metric. You can assign up to 30 dimensions to a metric.

## Periods

A period is the length of time associated with a specific CloudWatch statistic. Each statistic represents an aggregation of the metrics data collected for a specified period of time.

## Alarms

An alarm watches a single metric over a specified time period, and performs one or more specified actions, based on the value of the metric relative to a threshold over time. 

The action is a notification sent to an [SNS](SNS) topic or an Auto Scaling policy.

Alarms invoke actions for sustained state changes only. CloudWatch alarms do not invoke actions simply because they are in a particular state. The state must have changed and been maintained for a specified number of periods.

### Alarm events

CloudWatch sends events to [EventBridge](EventBridge.md) whenever a CloudWatch alarm is created, updated, deleted, or changes alarm state. You can use these events to write rules that take actions, such as notifying you, when an alarm changes state.

## Events

Delivers a near real-time stream of system events that describe changes in AWS resources.

### Event rules

A rule matches incoming events and routes them to targets for processing.

### Event targets

A target processes events. Targets can include [EC2](EC2.m2) instances, [Lambda](Lambda.md) functions, [Kinesis](Kinesis.md) streams, [ECS](ECS.md) tasks...

> [EventBridge](EventBridge.md) is the preferred way to manage your events.