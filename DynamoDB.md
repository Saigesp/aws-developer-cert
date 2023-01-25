# AWS DynamoDB

Fully managed NoSQL database service.

## Primary keys (PK)

Uniquely identifies items in the table. Each partition key must be a scalar (string, number or binary).

#### Types:
- **Simple**.
    - Items have only one **partition key**.
    - No two items can have the same partition key.
- **Composite**.
    - Items have one **partition key** and a **sort key**.
    - No two items can have the same partition key and sort key.
    - All items with same **partition key** are stored together, sorted by **sort key**.

## Secondary Indexes

You can define up to 5 Global and 5 Local secondary Indexes per table.

- **Local Secondary Index (LSI)**.
    - An index that has the same partition key as the table, but a different sort key.
    - Local: every partition is scoped to a base table partition that has the same partition key value.
- **Global Secondary Index (GSI)**.
    - An index with a partition key and sort key that can be different from those on the table.
    - Global: The queries on the index can span across all partitions.

### Differences between GSI and LSI

| Global Secondary Index (GSI)   | Local Secondary Index (LSI)              |
| ------------------------------ | ---------------------------------------- |
| PK simpe or composite          | PK composite                             |
| Index any table attr.          | Index has PK                             |
| Created at any time            | Must be created at table creation        |
| Can query entire table         | Only can query a single partition        |
| No supports strong consistency | Supports eventual and strong consistency |

## Read consistency

Two types of read consistency:

- **Eventually Consistent Reads**:
    - On a read, the response might not reflect the latests values upon some time.
- **Strongly Consistent Reads**:
    - On a read, the response reflects the latests values.
    - Not supported on global secondary indexes (GSI).
    - Uses more throughput capacity (~x2).
    - Possibly higher latency.

## Read Capacity Unit (RCU) and Write Capacity Unit (WCU)

Read cost 1 RCU for 8 KB in strongly consistency and 4 in eventually consistency.

Write cost 1 WCR for both models.

### How to calculate

Ex: You want to read 420 items of 5KB each every minute using strong consistency read. How many RCUs will you need?

- Count items to read every second.
    - 420/60 = 7 items per second.
- Calcule how much RCU per item is needed.
    - 5KB/4KB = 1.25 and round up -> 2 RCUs to read one item.
- Multiply items per second and RCU needed per item.
    - 2*7 = 14 Total RCU required.

### Pricing

(us-west) Each WCU cost $0.01, and 50 RCU costs $0.01 per hour.

Example: If you create a table and request 10 units of write capacity and 200 units of read capacity of provisioned throughput, you would be charged:

(10 x $0.01) + ((200 / 50) x $0.01) = $0.05 per hour

### Common errors

An `ProvisionedThroughputExceededException` error will be thrown if the provisioned limit is exceeded.

To solve it, use **retries** (SDK implements them automatically) or a **exponential backoff algorithm**.

## Queries

Find items based on primary key values.

You must provide:
- The name of the partition key attribute (CustomerID)
- A single value for that attribute (1321321).
- (Optional) A sort key attribute with a comparison operator (Age > 18).

```sh
aws dynamodb query \
    --table-name Customers \
    --key-condition-expression "ID = :id and LastSeen = :time" \
    --filter-expression "#v >= :num" \
    --expression-attribute-names '{"#v": "Views"}' \
    --expression-attribute-values '{
        ":id": {"N": "13213245" },
        ":time": {"S": "2023-01-21" },
        ":num": {"N": "3" }
    }'
```
- **KeyConditionExpression**: The condition that specifies the key values for items to be retrieved by the action. Must perform an equality test on a single partition key value.
- **FilterExpression**: A string that contains conditions to apply AFTER the query operation, but before the data is returned.
- **ProjectionExpression**: A string that identifies the attributes that you want (like projections in MongoDB).
- **ConsistentRead**: If set to true, then the operation uses strongly consistent reads.
- **Limit**: Limit the number of results.

## Scans

Can be sequential (default) or parallel.
- A **sequential scan** might not always be able to fully use the provisioned read capacity because is constrained by the maximum throughput of a single partition.
- A **parallel scan** works with workers who executes its own scan with the parameters:
    - Segment: A segment to be scanned by a particular worker (must be different).
    - TotalSegments: The total number of segments (same as number of workers).

## Updates

To manipulate data you use the `PutItem`, `UpdateItem`, and `DeleteItem`.

### Condition expressions

You can specify a condition expression to determine which items should be created, modified or deleted.

If the condition expression evaluates to true, the operation succeeds; otherwise, the operation fails.

```sh
aws dynamodb delete-item \
    --table-name ProductCatalog \
    --key '{"Id":{"N":"456"}}' \
    --condition-expression "(ProductCategory IN (:cat1, :cat2))" \
    --expression-attribute-values '{
        ":cat1": {"S": "Sporting"},
        ":cat2": {"S": "Gardening"}
    }'
```

## DynamoDB Streams

Optional feature that captures items modifications:
- **time-ordered**.
- **item-level** modifications.
- It only supports Lambda as a target (for now).

## DyanmoDB Accelerator (DAX)

DynamoDB-compatible caching service. Its a read-through & write-through cache (it sits "in front" of the database).

### Notes:
- Caches GetItem, BatchGetItem, Scan, and Query results.
- Caches PutItem, UpdateItem, DeleteItem, and BatchWriteItem ONLY if the table is successfully updated first.
- If the table is configured as strongly-consistent, DAX will be ignored (request will hit the table).

### Cache eviction
- With **TTL** value that specifies the time that an item is available.
- When the cache is full it evicts the **Least Recently Used** (LRU) item.

## Expiring items (TTL)

Allows you to define a per-item timestamp to determine when an item is no longer needed and deleting it automatically without consuming any write throughput.

You can choose the attribute name but must be a timestamp.

## Global Tables

Provides a fully managed solution for deploying a multi-region, multi-master database without building and maintaining your own replication solution.

When you create a global table, you specify the AWS regions where you want the table to be available. DynamoDB performs all of the necessary tasks to create identical tables in these regions and propagate ongoing data changes to all of them.

## Best practices

- Sort keys
    - Using sort keys for version control (ex: v0_ at the beginning of the sort key).
- Secondary indexes
    - Keep the number of indexes to a minimum.
    - Keep the size of the index as small as possible.
        - Project fewer attributes.
        - Avoid projecting attributes rarely used.
- Queries and scans
    - Use Query instead of Scan.
    - Reduce page size when scanning.
    - Use parallel scan with tables > 20 GB or when sequential scan is too slow.
- Large items
    - Compress large attribute values with algorithms such as GZIP or LZO
    - Storing large attribute values in Amazon S3 (and save the object identifier in the attribute)
- Time series data
    - Create one table per period.
    - Before the end of each period, prebuild the table for the next period.
    - Reduce provisioned read and/or write capacity to old tables.