# AWS DynamoDB

Fully managed NoSQL database service that provides fast and predictable performance with seamless scalability.

### Characteristics:
- **Fully managed** by AWS.
- **Multi-region**.
- **Multi-master**.
- **Performance at Scale**.
- **Distributed**.
- **In-memory cache**.
- **Encryption at rest**.
- **No downtime scalation**.
- **on-demand backup capability**.

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

## DynamoDB Streams

Optional feature thath captures a time-ordered sequence of item-level modifications in any DynamoDB table and stores them encrypted in a log for up to 24 hours.

Can be used for example with AWS Lambda to create a trigger