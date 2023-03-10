# AWS Kinesis Data Streams

Service that collects and process large **streams** of data records in real time.

It works with **producers**, who put records into streams, and **consumers** (aka Kinesis data applications), who get records from the stream and process them.

## Data Stream

A data stream is a set of **shards**. Each shard has a sequence of **data records**. Each data record has a sequence number that is assigned by Kinesis Data Streams.

## Shard

A shard is a uniquely identified sequence of data records in a stream. A stream is composed of one or more shards, each of which provides a fixed unit of capacity.

Each shard can support:

| Read                  | Write                |
| --------------------- | -------------------- |
| 5 transactions/second | 1.000 records/second |
| 2 MB/s rate           | 1 MB/s rare          |

If your data rate increases, you can increase or decrease the number of shards allocated to your stream.

### Shard Iterator

A shard iterator specifies the shard position from which to start reading data records sequentially.

A new shard iterator is returned by every GetRecords request (as `NextShardIterator`), which you then use in the next GetRecords request (as `ShardIterator`).

If you are getting `ProvisionedThroughputExceededException` error messages, try to increase the number of shards in the data stream and specify a shard iterator.

## Data Record

A data record is the unit of data stored, composed of:

- Partition key.
    - Used to group data by shard. Kinesis segregates the records belonging to a stream into multiple shards, and uses the partition key to determine which shard the record belongs to.
- Sequence number.
    - Unique number per partition-key within its shard.
- Data blob
    - Immutable sequence of bytes.


## Capacity Mode

Determines how capacity is managed and how you are charged for the usage of your data stream:

- **on-demand**:
    - Kinesis automatically manages the shards in order to provide the necessary throughput.
    - You are charged only for the actual use.
- **provisioned**:
    - You must specify the number of shards for the data stream.
    - You are charged for the number of shards at an hourly rate

## Retention Period

The default retention period of 24 hours covers scenarios where intermittent lags in processing require catch-up with the real-time data. A seven-day retention lets you reprocess data for up to seven days to resolve potential downstream data losses. Long term data retention greater than seven days and up to 365 days lets you reprocess old data for use cases such as algorithm back testing, data store backfills, and auditing.

## Streams Application

An Kinesis Data Streams Application is a **consumer** of a stream that commonly runs on a fleet of EC2 instances.

There are two types of consumers that you can develop:
- **Shared fan-out**
    - All share a shard???s 2 MB/s of read throughput and 5 transactions/s.
    - Requires GetRecords API.
- **Enhanced fan-out**
    - Own 2 MB/s read throughput, with multiple consumers reading the same stream.
    - Requires SubscribeToShard API.

## Server-Side Encryption

Amazon Kinesis Data Streams can automatically encrypt sensitive data as a producer enters it into a stream with [KMS](KMS.md) master keys.

To read from or write to an encrypted stream, producer and consumer applications must have permission to access the master key.

## Kinesis Producer Library (KPL)

Interface that enables you to quickly achieve high producer throughput with minimal client resources.

## Common troubleshooting

- Some records are skipped when using the client library.
    - Problem: An unhandled exception thrown from `processRecords`.
    - Solution: Handle all exceptions within `processRecords` appropriately.
- Records belonging to the same shard are processed by different processors at the same time.
    - Problem: A worker instance that loses network connectivity.
    - Solution: When a `ShutdownException` raises, exit the current method cleanly.
- [More here](https://docs.aws.amazon.com/streams/latest/dev/troubleshooting-consumers.html)