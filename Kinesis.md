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

The retention period is the length of time that data records are accessible after they are added to the stream

## Streams Application

An Kinesis Data Streams Application is a **consumer** of a stream that commonly runs on a fleet of EC2 instances.

There are two types of consumers that you can develop:
- **Shared fan-out**
- **Enhanced fan-out**

## Server-Side Encryption

Amazon Kinesis Data Streams can automatically encrypt sensitive data as a producer enters it into a stream with [KMS](KMS.md) master keys.

To read from or write to an encrypted stream, producer and consumer applications must have permission to access the master key.