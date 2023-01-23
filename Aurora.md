# AWS Aurora

Fully managed relational database engine that's compatible with MySQL and PostgreSQL

2 copies of data are kept in each AZ with a minimum of 3 AZ’s (6 copies).

Can handle the loss of up to 2 copies of data without affecting DB write availability and up to 3 copies without affecting read availability.

## Replication types

There are two types of replication:

| Feature                                          | Aurora Replica      | MySQL Replica
| ------------------------------------------------ | ------------------- | --------------
| Number of replicas                               | Up to 15            | Up to 5
| Replication type                                 | Asynchronous (ms)   | Asynchronous (seconds)
| Performance impact on primary                    | Low                 | High
| Replica location                                 | In-region           | Cross-region
| Act as failover target                           | Yes (no data loss)  | Yes (potentially minutes of data loss)
| Automated failover                               | Yes                 | No
| Support for user-defined replication delay       | No                  | Yes
| Support for different data or schema vs. primary | No                  | Yes
