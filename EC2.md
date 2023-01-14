# AWS EC2 - Elastic Compute Cloud

Virtual Machines hosted on AWS

## AMI

An AMI (Amazon Machine Image) is a required template (image) that contains the software configuration (OS, application server and applications). Also includes launch permissions and Volume information.

### Two main types of AMIs:
- **Backed by Amazon EBS**: the root device is an Amazon EBS volume.
- **Backed by instance store**: the root device is an instance store volume (created from a template stored in [Amazon S3](S3.md)).

### Categorization:

- Virtualization types:
    - **HVM (Hardware-Assisted Virtual Machine)**: Best performance.
    - **Paravirtualization**: 
- Root Device Type:
    - **EBS (Elastic Block Storage)**: 
    - **Instance Store**: 
- By purpose:
    - **General purpose**: Code (t-).
    - **Compute optimized**: Code (c-).
    - **GPU Graphics**: Code (p-).
    - **GPU Compute**: Code (p-).
    - **Memory optimized**: Code (x-).
    - **Storage optimized**: Code (h-).
- Purchasing options:
    - **On-demand**:
        - Suitable when you are not sure about the capacity needed (sudden spike of traffic, short projects, R&D, etc).
        - Pay-as-you-go: You pay instances when needed, with current listed price per usage.
        - Most expensive.
    - **Reserved**:
        - Suitable when you are aware about the capacity needed.
        - Proactive type: You pay the VMs beforehand.
        - Earlier and longer the period reserved, lower cost.
        - Instances can be reserved from 1 to 3 years.
    - **Scheduled**:
        - Purchase instances that are always available on the specified schedule for a one-year term.
    - **Spot**:
        - Give-me-if-available-for-this-price.
        - Saves costs significantly.
        - Little risky.
        - You bid for the unused EC2 instance capacity of AWS: If the actual price of that EC2 instance becomes equal or lower than the bid price, the instance gets assigned to you at **your bid price**, but if the actual price increases, the instance may get terminated with a half an hour's noticed (this half hour will not be charged).
        - Some concepts:
            - **Spot Instance Pool**: a set of unused instances with the same instance type.
            - **Spot Price**: Currently hourly price of the instance.
            - **Spot Instance Request**: Your bid or the price you are willing to pay for an instance.
            - **Spot Fleet**: You specify the criteria of the instance and then the Spot Fleet selects the Spot Instance Pool that meets your needs and launches Spot Instances.
            - **Spot Instance Interruption**: When Amazon terminates your instance.
    - **Dedicated Host**:
        - Instance it's not shaded with other customer's VMs, but may be in a different machine.
    - **Dedicated Instance**:
        - Instance it's not shader with other customer's VMs, and always will be running in the same machine.
        - Useful when using hardware licensing.

### Cross-regions AMI Copying

| Source to destination encryption | Supported |
| -------------------------------- | --------- |
| Unencrypted to unencrypted       | Yes       |
| Encrypted to encrypted           | Yes       |
| Unencrypted to encrypted         | Yes       |
| Encrypted to unencrypted         | No        |

#### Important notes:
- There aren't charges for copying an AMI, but standard storage and data transfer rates apply.
- AWS doesn't copy launch permissions, user-defined tags, or [Amazon S3](S3.md) bucket permissions.
- AMI sharing between different AWS accounts is allowed.
- You can deregister an AMI. This no affects currently running instances with this image but you will not be able to use it to launch new instances.

## EBS

A volume is a block-level storage device that you can attach to a single AMI instance (like a hard drive).

#### Notes:
- Mostly used as the primary storage for data that requires frequent updates.
- They are flexible: you can increase its size, modify IOPS (Input/output Operations Per Second) capacity, and change its type.
- The data stored on EBS volumes stays persistent even if the instance terminates.
- EC2 instance to volume is a 1:N relationship (many volumes can be attached to a single instance).

### Types of EBS volumes

- **General purpose SSD (GP2)**.
    - General purpose where it balances cost and performance.
    - Supports variety of workloads.
    - Mostly used for Low-latency interactive applications and dev/test environments.
    - It provides baseline performance for 3 IOPS per GiB.
    - Min-max IOPS is 100-10.000.
    - Can be used as the root volume of a EC2 instance.
- **Provisioned IOPS SSD (IO1)**.
    - For applications that required sustained IOPS performance, more that 10.000 IOPS or 160 MiB/s of throughput per volume.
    - For applications that demands extensively high-throughput and lowest latency.
    - Used when database workloads are very high.
    - It provides baseline performance for 50 IOPS per GiB.
    - Min-max IOPS is 100-32.000.
    - Can be used as the root volume of a EC2 instance.
- **Magnetic HDD**.
    - Suited for applications where low-cost storage for small volumes sizes is important.
    - Suited for workloads where data is accessed infrequantly.
    - Delivers aprox. 100 IOPS and max throughput is 90 MiB/s.
    - Can be used as the root volume of a EC2 instance.
- **Throughput optimized HDD**.
    - Suited for applications that have frequently access throughput-intensive workloads.
    - Low cost HDD option.
    - Max throughput per volume is 500 MiB/s.
    - Cannot be used as the root volume of a EC2 instance.
- **Cold HDD**.
    - Suited for applications that have less frequently access non-intensive workloads.
    - Lowest cost HDD option.
    - Max throughput per volume is 250 MiB/s.
    - Cannot be used as the root volume of a EC2 instance.

> Differences between SSD and HHD: **SSD** are optimized and more suited for read/write operations with small I/O size and generally they are more expensive; **HHD** are more useful when the throughput (MiB/s) is more critical than IOPS.

### EBS Advantages

- **Data availability**: AWS replicates the volume within the same AZ for high availability.
- **Data persistence**: The data persist indepently from the life of an instance (until the volume is deleted explicitly) and volumes can be reattached to new instances (enabling quick data recovering). Also, EBS-backed instances can be stopped and restarted without affecting the data stored.
- **Data encryption**: Encrypted EBS volumes can be created with the Amazon EBS encryption feature (using AES-256 and AWS KMS master keys).
- **Backup & Snapshots**: Data in volume can be backed via snapshots (stored in [S3](S3.md)), even in running volumes. Also volumes that are restored from a encrypted snapshot are automatically encrypted.
- **Flexibility**: Volume type, size and IOPS can be changed without service interruptions.

### Volume encryption

Only volumes that are not the root volume of a instance can be encrypted (requires KMS).

When you encrypt a volume the following types of data are encrypted:
- Data at rest inside the volume.
- All data moving between the volume and the instance.
- All snapshots created from the volume.
- All volumes created from these snapshots.

### Volume snapshots

A point-in-time backup of the data that is stored on a EBS volume. They are stored in [S3](S3.md) and contains all the information needed to restore your data to a new EBS volume.

#### Important points: 
- **Snapshots are incremental**: Only the blocks on the devide that have changed after your more recent snapshot are saved.
- **Loading snapshots is lazy-loading**: After a volume is created from a snapshot, not all the data is transfered from [S3](S3.md) instantly. If the instance tries to access non-loaded-yet data, the volume inmediatly downloads the requested data and then continues loading the rest of the data.
- **Region-aware**: Snapshots are constrained to the region where they are created, but can be cross-region copied.

## Instance storage

**Temporary** block-level storage located on disk that are physically attached to the host computer.

Ideal for temporary storage of information that changes frequently (buffer, caches, etc).

They are **physically attached** to the instance and connot be detached or attached to a different instance.

The data persist during the lifetime of the instance (even if the instace reboots), but can be lost if the underlying disk fails or the instance stops or terminates.

If you creates an AMI (image) from an instance, the data isn't preserved.

## Placement Group

Arrangements of instances on the underlying hardware/hypervisor. It determines how the instances are placed on the hypervisor upon launching.

There are no charges for creating a placement group.

#### Types:
- **Cluster**:
    - Instances on a single hypervisor in single AZ.
    - Instances are launched in a low-latency group.
    - You need to launch the instances that support _Enhanced Networking_.
    - It's recommended to launch all same-type instances.
    - If you try to add more instances to an existing placement group, or diferent types instances, you might get an insufficient capacity error.
    - If you stop stop an instance in a placement group and then start it again, it still runs in the placement group, but the start will fail if there isn't enought capacity for the instance.
    - If you receive a capacity error when launching an instance in a placement group that has already running instances, try to stop and start all of them, and try the launch again.
- **Spread**:
    - Each instance on a separate hypervisor in separate AZ.
    - Reduces the risk of impact of underlying hardware failure.
    - You can lauch different types of instances and at a different time.
    - Instances can span different AZ (max 7 running instances per AZ per group).
    - If you try to launch an instance and there is no unique hardware available, your request will fail (Try again after some time).
    - Not available for Dedicated Instances or Dedicated Hosts.

## Security Group

A set of "firewall" rules that control the traffic for your instance.

### Important notes

- Security Grouo protects the instance by applying a securitty wall of rules (like a firewall).
- Controls in-and-out traffic.
- Can be configured using:
    - Request type (TCP, UDP, SSH, HTTP...).
    - Protocol (TCP, UDP, ICMP...).
    - Port number or range.
    - Source of the traffic.
    - Description.
- By default all incoming traffic is denied. You can only define ALLOW rules.
- **If you allow traffic of a particular type from a source into your instance, the outgoing traffic is allowed automatically**. (Stateful definition).
- Security Groups works at instance level and multiple instances can be added under the same Security Group.
- The Security Group rules are applied instantaneously.

## Key pairs

To encrypt and decrypt the login information Amazon EC2 uses public-key cryptography: A public key that AWS stores and a private key that you stores.

## SSH Connection

HostName: ec2-user@<instance_dns_or_public_ip>

## Network interface

A component or a virtual network card that you can attach to an instance so it can be reached from another instance.

Every instance on a VPC has a default network interface, called the primary network interface (eth0). This primary interface cannot be detached, but you can attach additional network interfaces.

### Properties & attributes:

- A primary private IPv4 address.
- One or more secondary private IPv4 addresses.
- One Elastic IP address (IPv4).
- One public IPv4 address.
- One or more IPv6 addresses.
- One or more Security Groups.
- One MAC address.
- A source/destination check flag.
- A description.
