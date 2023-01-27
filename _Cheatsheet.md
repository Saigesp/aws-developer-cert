# Cheatsheet

## ARN
```
arn:{partition}:{service}:{region}:{accountId}:{resource-type}:{resource-id}
```

## API Gateway
- Types:
    - REST
    - RESTful
    - Websocket
- Endpoints:
    - Edge-optimized
    - Regional
    - Private
- Integrations:
    - AWS
    - AWS_PROXY
    - HTTP
    - HTTP_PROXY
    - MOCK
- CORS:
    - "Access-Control-Allow-Headers"
    - "Access-Control-Allow-Origin"
- Authentication:
    - IAM
    - Lambda authorizers
    - Cognito authorizers
- Cache:
    - Default 300s TTL
    - Disabled with `Cache-Control: max-age=0` header
    - Forced with `InvalidateCache` or "Require authorization"

## Cloud9
- Environments:
    - EC2
    - SSH

## CloudFormation
- Template:
    - Resources (required)
    - Transform (required with SAM)
    - Format Version
    - Description
    - Metadata
    - Parameters
    - Rules
    - Mappings
    - Conditions
    - Outputs
- Update types
    - Direct update
    - Via Change Set
- Deletion policy
    - Retain
    - Delete
    - Snapshot

## CloudFront
- Cache:
    - Max object 30GB
    - Defaults 24h
    - Customized with `Cache-Control` or `Expires` header
    - Remove an item: `Invalidation API`
- Edge locations:
    - Regional Edge Cache (default)
    - Geo Restriction

## CloudTrail
- Monitoring
    - Periods of 15 minutes.
    - Retention 90 days.
- Events sources:
    - Users
    - Roles
    - Services
- Events targets:
    - S3
    - SNS
    - CloudWatch Logs and Events

## CloudWatch
- Monitoring:
    - Basic (default): 5 min with no charge
    - Details: 1 min
    - Log retention between 1 day - 10 years
- Metrics retention:
    - < 60 seconds -> 3 hours
    - 60 seconds -> 15 days
    - 300 seconds -> 63 days
    - 3600 seconds -> 455 days
- Alarms targets:
    - SNS
    - Auto Scaling Policy
- Event targets:
    - Lambda
    - Kinesis
    - EC2
    - ECS
    - EventBridge

## CodeCommit
- Encrypted by default (transit and rest)
- Access:
    - Git credentials for IAM user.
    - SSH keys.
- Notification events:
    - Pull request updates
    - Pull request comments
    - Commit comments
- Trigger events:
    - Push to existing branch
    - Create branch/tag
    - Delete branch/tag
- Trigger targets:
    - SNS
    - Lambda

## CodeBuild
- Specification file: buildspec.yml
- Phases
    - install
    - pre_build
    - build
    - post_build
- Event targets:
    - CloudWatch
- Supported OS:
    - Linux
    - Windows

## CodeDeploy
- Specification file: appspec.yml
- Artifact types:
    - ZIP
    - TAR
    - TAR.GZ
- Deploy targets:
    - EC2
    - Onprem
    - Lambda
    - ECS
- Deploy types:
    - In-place (only EC2 & Onprem)
        - AllAtOnce
        - HalfAtATime
        - OneAtATime
    - BLue/Green
- Agent
    - Required in EC2 & Onprem
    - Config params:
        - :log_aws_wire: If logs must be caputed.
        - :log_dir: Where to put the logs files.
        - :pid_dir:
        - :root_dir:
        - :proxy_uri: (Optional)
        - :max_revisions: (Optional)
        - :enable_auth_policy: (Optional, true with VPC)
- Hooks:
    - Common
        - BeforeAllowTraffic
        - AfterAllowTraffic
    - EC2/On-Prem
        - ApplicationStart
        - ApplicationStop
        - BeforeInstall
        - AfterInstall
        - BeforeBlockTraffic
        - AfterBlockTraffic
        - ValidateService
    - ECS
        - BeforeInstall
        - AfterInstall
        - AfterAllowTestTraffic
    - Lambda
        - only common hooks
- Rollback
    - Manual if its content overwrite

## CodePipeline
- Action types:
    - Source
    - Build
    - Test
    - Deploy
    - Approval
    - Invoke (Lambda)
    - Custom actions:
        - Only build, deploy, test and invoke.
        - Required job worker.
- Triggers:
    - CloudWatch events.
    - CloudWatch Scheduler.
    - Github WebHooks.
    - CodePipeline Polling.
- Security:
    - SSE-KMS
    - Git auth with AWS-managed OAuth or customer-managed.
- Monitoring
    - CloudTrail
    - CloudWatch events
- Jenkins
    - Install it on EC2

## Cognito
- User pools
    - Sign-up and sign-in (also social).
    - User directory and profiles.
    - Triggers Lambda
    - MFA
        - Only on poll creation
        - SMS and TOTP
- Identity pools
    - Temporary & limited credentials.
    - Federated Identities: Credentials to mobile & untrusted envs.
- Cognito Sync
    - Sync user data between applications

## DynamoDB
- Primary keys
    - Simple
    - Composite (with sort key)
- Secondary Indexes
    - Local
        - PK required
        - Only when creating.
        - Support strong consistency
    - Global
        - Pk not required
        - Anytime
        - No strong consistency
- Read consistency
    - Eventually (1 RCU/4KB)
    - Strong (1 RCU/8KB)
- Scans
    - Sequential
    - Parallel
- Streams
    - Item level
    - Time-ordered
- Accelerator (DAX)
    - Only works on eventually consistent.
    - Caches READs.
    - Caches WRITES after success update.
    - Eviction with TTL or when table is full.

## EC2
- Virtualization types:
    - Hardware-Assisted Virtual Machine
    - Paravirtualization
- Root Device Type
    - EBS
    - Instance Store
- By purpose:
    - General purpose
    - Compute optimized
    - GPU Graphics
    - GPU Compute
    - Memory optimized
    - Storage optimized
- Purchasing options:
    - On-demand
    - Reserved
    - Scheduled
    - Spot
    - Dedicated Host
    - Dedicated Instance
- Instance profiles
    - Only one role
    - All applications on the instance share the same role

## EBS
- Types:
    - General purpose SSD (GP2)
    - Provisioned IOPS SSD (IO1)
    - Magnetic HDD
    - Throughput optimized HDD
    - Cold HDD

## ECS
- Launch types:
    - Fargate (pay-as-you-go)
    - EC2
- Cluster states:
    - ACTIVE
    - PROVISIONING
    - DEPROVISIONING
    - FAILED
    - INACTIVE
- Task placement strategies
    - binpack
    - random
    - spread

## ElastiCache
- Memcached
    - No AZ
    - Multithreaded
    - Simple caching model
- Redis
    - AZ
    - Backups and restore
    - Pre-loaded data

## Elastic Beanstalk
- Config:
    Files in `.ebextensions/`
    JSON or YAML
- Deploy strategies
    - All-at-Once
    - Rolling
    - Rolling with additional batch
    - Immutable
    - Immutable with Traffic Splitting

## ELB
- Types:
    - Application Load Balancer
    - Network Load Balancer
    - Classic Load Balancer
- Cycles:
    - (Start) -> Pending -> In service
    - (Fails) -> Terminating -> Terminated
    - (Detach) -> Detaching -> Detached
    - (Attach) -> Pending -> In service
    - (Standby) -> EnteringStandby -> Standby
    - (Exit Standby) -> Pending -> In service
- Hooks:
    - Pending:Wait -> After enter Pending
    - Pending:Proceed -> Before enter In service
    - Terminating:Wait -> After enter Terminating
    - Terminating:Proceed -> Before enter Terminated
- Monitoring targets
    - S3
    - CloudWatch
    - CloudTrail

## Kinesis
- Shards
    - Read: 5/transaction/s; 2MB/s
    - Write: 1000 records/s; 1MB/s
- Data record components:
    - Partition key
    - Sequence number
    - Data blob
- Retention
    - Defaults 24h
    - 7 days
    - Long term up to 365 days
- Consumer types:
    - Shared fan-out
        - All share a shard’s 2 MB/s of read throughput and 5 transactions/s.
        - Requires GetRecords API.
    - Enhanced fan-out
        - Own 2 MB/s read throughput, with multiple consumers reading the same stream.
        - Requires SubscribeToShard API.

## KMS
- Key types
    - Customer managed
    - AWS managed
    - AWS owned
- Key limitations
    - Data up to 4KB
    - 100.000 keys per account per region
- Actions
    - CreateKey
    - GenerateDataKey
    - GenerateDataKeyWithoutPlaintext
    - Encrypt 
    - Decrypt
- Key deletion
    - Waiting time from 7 to 30 days

## Lambda
- Capacity and limits
    - 128MB memory, 3GB max.
    - 3s running per call, 900s max.
- Folders
    - `opt/` for layers.
    - `tmp/` for temporal use.
- Authorizer types:
    - Token-based
    - Request parameter (required in websocket)
- Best practices
    - Avoid recursive code
    - Separate handler from logic
    - Initialization outside of the handler
    - Minimize deployment package

## RDS
- Limits:
    - 40 total databases
    - 10 for licensed SQL Server
    - 10 for included-licensed Oracle
    - 40 for customer-licensed Oracle
    - 40 for MySQL, MariaDB, or PostgreSQL
- Storage types:
    - General Purpose SSD (aka gp2 or gp3).
    - Provisioned IOPS SSD (aka io1).
    - Magnetic (aka standard).
- Logs types:
    - Error log
    - General query log
    - Slow query log

## Route 53
- DNS Types:
    - Authoritative
    - Recursive/Resolver
- Routing Policies
    - Simple
    - Weighted
    - Latency
    - Geolocation
    - Failover

## S3
- Types:
    - Standard
    - Intelligent
    - Standard Infrecuent Access
    - One Zone Infrecuent Access
    - Glacier
    - Glacier Deep Archive
- Limits:
    - 5.500 READ per second per prefix.
    - 3.500 WRITE per second per prefix.
- Consistency types
    - Read after Write Consistent
    - Eventually Read after Write Consistent
    - Eventually Consistent
- Multipart Upload
    - Ideally >100MB
    - Required >5GB
    - Max >5TB
- Events targets
    - SNS
    - SQS
    - Lambda
- Caching
    - CloudFront
    - ElastiCache

## SQS
- Queue types
    - Standard
    - FIFO
- Polling types
    - Short
    - Long
- Visibility timeout
    - Default 30s
    - From 0s to 12h
