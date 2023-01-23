# AWS Athena

An interactive query service to analyze data in [S3](S3.md) using standard SQL.

### Notes:
- Directly queries S3 (does not load the data into any compute).
- Only can query the last version of an file.
- If the data in the bucket is encrypted, it must be stored in the same region, and the pricipal who creates the Athena table must have the permissions to decrypt the bucket.
- Does not support:
    - Different storage classes within the bucket.
    - [Glacier](S3.md#storage-classes) storage class.
    - Requester Pays buckets.
- If you query files on a S3 bucket with large number of objects and the data is not partitioned, it may affect the Get request rate limits in [S3](S3.md).

