# AWS SNS - Simple Notification Service

Managed service that provides message delivery from publishers to subscribers.

### Differences between SNS and [SQS](SQS.md)

| SNS                                          | SQS                             |
| -------------------------------------------- | ------------------------------- |
| Distributed publish-subscribe system         | Distributed queuing system      |
| Used to notify apps                          | Used to decouple/integrate apps |
| Messages are pushed to subscribers           | Receivers have to pull messages |
| Multiple subscribers                         | Single receiver                 |
| Messages immediately sent                    | Polling produces some latency   |
| Several endpoints (email, SMS, HTTP, SQS...) | Limited endpoints               |

### Features and capabilities:
- Application-to-application messaging.
- Application-to-person notifications.
- Standard and FIFO topics.
- Message durability.
    - Messages stored across multiple, geographically separated servers and data centers.
    - Delivery retry policy.
    - Dead-letter queue.
- Message archiving and analytics.
- Message attributes.
- Message filtering.
- Message security.
    - Server-side encryption using [KMS](KMS.md).
    - Available private connection between SNS and your [VPC](VPC.md).

## Fanaout scenario

When a message is sent to a topic and then replicated and pushed to SQS, HTTP endpoints, emails, etc.
