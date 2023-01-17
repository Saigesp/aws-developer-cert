# AWS Redshift

Redshift is a data warehouse service fast, fully managed and petabyte-scale. It's a collection of computing resources called **nodes**, which are organized into a group called a **cluster**. Each cluster runs an Amazon Redshift engine and contains one or more databases.

Is a relational database management system (RDBMS) and is compatible with other RDBMS applications. It provides the same functionality as a typical RDBMS, including Online Transaction Processing (OLTP) functions such as inserting and deleting data.

### Why to use?

- It specifically designed for Online Transaction Processing (OLTP) and Business Intelligence (BI) that require complex queries against large datasets.
- It's integrated with various ETL tools and BI reporting, data mining and analytics tools.
- It's based on PostgreSQL.

### Concepts:

- **Cluster**.
    - Group of one or more compute nodes.
- **Node**.
    - If a cluster has two or more nodes, an additional **leader node** coordinates them.
    - Leader node:
        - Handles external communication with applications.
        - Caches some queries results in memory.
    - Compute nodes:
        - Run the compiled code.
        - transparent to external applications.
        - Types:
            - Dense storage node.
            - Dense compute node.
- **Database**.
    - User data is stored in one or more databases on the compute nodes. Your SQL client communicates with the leader node, which in turn coordinates running queries with the compute nodes.
    - Within a database, user data is organized into one or more schemas.

### Consuming data

Data can also be consumed for analytical workloads as follows:

- Use **datashares** to share live data across Amazon Redshift clusters for read purposes with relative security and ease. You can share data at different levels, such as databases, schemas, tables, views (including regular, late-binding, and materialized views), and SQL user-defined functions (UDFs).
- Use **Amazon Redshift Spectrum** to query data in [Amazon S3](S3.md) files without having to load the data into Amazon Redshift tables. Amazon Redshift provides SQL capability designed for fast online analytical processing (OLAP) of very large datasets that are stored in both Amazon Redshift clusters and Amazon S3 data lakes.
- **Join data from relational databases**, such as Amazon Relational Database Service ([Amazon RDS](RDS.md)) and [Amazon Aurora](Aurora.md), or [Amazon S3](S3.md), with data in your Amazon Redshift database using a federated query. You can use Amazon Redshift to query operational data directly (without moving it), apply transformations, and insert data into your Amazon Redshift tables.
- **Amazon Redshift machine learning (ML)** creates models, using data you provided and metadata associated with data inputs. These models capture patterns in the input data. You can use these models to generate predictions for new input data.

## Workload Management (WLM)

Enables users to flexibly manage priorities within workloads so that short, fast-running queries won't get stuck in queues behind long-running queries.

WLM creates query queues at runtime according to _service classes_, which define the configuration parameters for various types of queues, including internal system queues and user-accessible queues.

When you run a query, WLM assigns the query to a queue according to the **user's user group** or by **matching a query group** that is listed in the queue configuration with a query group label that the user sets at runtime.

By default, Redshift configures one queue with a concurrency level of 5 (up to 5 queries simultaneously, plus 1 predefined Superuser queue, with concurrency 1).

You can configure up to 8 query queues.
