# AWS SQS - Simple Queue Service

Hosted queue that lets you integrate and decouple distributed software systems and components.

### Queue types

| Standard               | FIFO                        |
| ---------------------- | --------------------------- |
| Throughput-centered    | Order-centered              |
| Unlimited throughput   | High throughput             |
| At-Least-Once Delivery | Exactly-Once Processing     |
| Best-Effort Ordering   | First-In-First-Out Delivery |

### Polling types

| Short polling               | Long polling             |
| --------------------------- | ------------------------ |
| Faster polling              | Slower polling           |
| Higher price                | Reduced price            |
| Queries only subsets        | Queries all SQS Servers  |
| Might false empty responses | No false empty responses |

## Visibility timeout

A period of time during which SQS prevents other consumers from receiving messages.
- Default: 30s
- Min: 0s
- Max: 12h

## Dead-letter queues

Queues that other queues can target for messaged that can't be processes successfully.

The dead-letter queues have to be of the same time as the associated queues (The dead-letter queue of a FIFO queue must also be a FIFO queue, and the dead-letter queue of a standard queue must also be a standard queue).

You must use the **same AWS account to create the dead-letter** queue and the other queues that send messages to it. Also, dead-letter queues **must reside in the same region** as the other queues that use the dead-letter queue.
