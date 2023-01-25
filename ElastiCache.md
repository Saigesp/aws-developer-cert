# AWS ElastiCache

Service to manage and scale a distributed in-memory data store or cache environment in the cloud.

#### Supports:
- Memcached
    - Does not support AZ.
    - Supports multithreaded performance using multiple cores.
    - Supports simple caching model.
- Redis
    - Supports AZ.
    - Supports Backups and restore.
    - Allows startup with pre-loaded data.

## Clusters

A cluster is a collection of one or more cache nodes, all of which run an instance of the Redis cache engine software.