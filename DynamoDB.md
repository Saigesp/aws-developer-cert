# AWS DynamoDB

Fully managed NoSQL database service that provides fast and predictable performance with seamless scalability.

### Characteristics:
- **Fully managed** by AWS.
- **Multi-region** (opt).
- **Multi-master** (opt).
- **Performance at Scale**.
- **Distributed**.
- **In-memory cache**.
- **Encryption at rest**.
- **No downtime scalation**.
- **On-demand backup capability**.

## Primary keys (PK)

Uniquely identifies items in the table. Each partition key must be a scalar (string, number or binary).

#### Two types:

- **Simple**.
    - Items have only one **partition key**.
    - No two items can have the same partition key.
    - Internal hash function determines the partition.
- **Composite**.
    - Items have only one **partition key** and a **sort key**.
    - No two items can have the same partition key and sort key.
    - Internal hash function determines the partition.
    - All items with same **partition key** are stored together, sorted by **sort key**.

## Secondary Indexes

You can define up to 5 GSIs and 5 LSIs per table.

### Types:

- **Global Secondary Index (GSI)**.
    - An index with a partition key and sort key that can be different from those on the table.
    - Global: The queries on the index can span across all partitions.
- **Local Secondary Index (LSI)**.
    - An index that has the same partition key as the table, but a different sort key.
    - Local: every partition of a LSI is scoped to a base table partition that has the same partition key value.

### Differences between GSI and LSI

| Global Secondary Index (GSI)  | Local Secondary Index (LSI)              |
| ----------------------------- | ---------------------------------------- |
| PK simpe or composite         | PK composite                             |
| Index any table attr.         | Index has PK                             |
| Created at any time           | Must be created at table creation        |
| Can query entire table        | Only can query a single partition        |
| Supports eventual consistency | Supports eventual and strong consistency |

## Read consistency

Two types of read consistency:

- **Eventually Consistent Reads**:
    - When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation and might include some stale data. If you repeat your read request after a short time, the response should return the latest data.
- **Strongly Consistent Reads**:
    - When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful.
    - Some disadvantages:
        - Not supported on global secondary indexes (GSI).
        - Uses more throughput capacity (~x2). If there is a network delay or outage, it might not be available and DynamoDB may return a server error.
        - If read requests do not reach the leader node on the first attempt, it may experience a higher latency.

## Read Capacity Unit and Write Capacity Unit

1 RCU provides 4KB read per second for a strongly consistency model, 8KB for an eventually consistency model.

1 WCR provides 1KB write per second for both models.

[See documentation here](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html).

> An `ProvisionedThroughputExceededException` error will be thrown if the provisioned limit is exceeded. The usual technique for dealing with these errors is to implement retries in the client application. Each AWS SDK implements retry logic automatically (can be modified) and implements an exponential backoff algorithm ( progressively longer waits between retries).

### How to calculate

Ex: You want to read 420 items of 5KB each every minute using strong consistency read. How many RCUs will you need?

- Count items to read every second.
    - 420/60 = 7 items per second.
- Calcule how much RCU per item is needed.
    - 5KB/4KB = 1.25 and round up -> 2 RCUs to read one item.
- Multiply items per second and RCU needed per item.
    - 2*7 = 14 Total RCU required.

## Pricing

If you create a table and request 10 units of write capacity and 200 units of read capacity of provisioned throughput, you would be charged:

(10 x $0.01) + ((200 / 50) x $0.01) = $0.05 per hour

## DynamoDB Streams

Optional feature thath captures a time-ordered sequence of item-level modifications in any DynamoDB table and stores them encrypted in a log for up to 24 hours.

It only supports Lambda as a target for now.

## Read data

To read data from a table, you use operations such as:

- **GetItem** (1 item).
- **Query** (various items).
- **Scan** (all table, max 1 MB per page).

### ProjectionExpression

A projection expression is a string that identifies the attributes that you want, like projections in MongoDB. [See documentation here](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ProjectionExpressions.html).

To retrieve a single attribute, specify its name. For multiple attributes, the names must be comma-separated.

## DyanmoDB Acelerator (DAX)

DynamoDB-compatible caching service

## Expiring items

Allows you to define a per-item timestamp to determine when an item is no longer needed, deleting it automatically without consuming any write throughput.

> You can choose the attribute name (must be a timestamp)

## Global Tables

Provides a fully managed solution for deploying a multi-region, multi-master database without building and maintaining your own replication solution.

When you create a global table, you specify the AWS regions where you want the table to be available. DynamoDB performs all of the necessary tasks to create identical tables in these regions and propagate ongoing data changes to all of them.

## Parallel scans

By default, the Scan operation processes data **sequentially**.

A sequential Scan might not always be able to fully use the provisioned read throughput capacity: Even though DynamoDB distributes a large table's data across multiple physical partitions, a Scan operation can only read one partition at a time. For this reason, the throughput of a Scan is constrained by the maximum throughput of a single partition.

To perform a **parallel scan**, each worker issues its own Scan request with the following parameters:
- Segment: A segment to be scanned by a particular worker. Each worker should use a different value for Segment.
- TotalSegments: The total number of segments for the parallel scan. This value must be the same as the number of workers that your application will use.