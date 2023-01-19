# AWS Athena

An interactive query service that makes it easy to analyze data in [Amazon S3](S3.md) using standard SQL.

Athena does not load the data into any compute, it directly queries S3.

It can only query the last version of an S3 file.

If the data in the bucket is encrypted, it must be stored in the same region, and the pricipal who creates the Athena table must have the approviate permissions to decrypt it.

Athena does not support:
- Different storage classes within the bucket.
- [Glacier](S3.md#storage-classes) storage class.
- Requester Pays buckets.

If you query files on a S3 bucket with large number of objects and the data is not partitioned, it may affet the Get request rate limits in Amazon S3.
