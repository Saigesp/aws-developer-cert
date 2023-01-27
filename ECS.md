# AWS ECS - Elastic Container Service

Managed container management service to run, stop, and manage containers on a cluster.

#### Features:
- A serverless option with AWS [Fargate](Fargate.md).
- Integration with [IAM](IAM.md).
- Managed container orchestration
- Continuous integration and continuous deployment (CI/CD).
- Support for service discovery.
- Support for sending your container instance log information to [CloudWatch](CloudWatch.md).

## Launch types

There are two models that you can use to run your containers:
- [Fargate](Fargate.md): Serverless pay-as-you-go option. You can run containers without needing to manage your infrastructure.
    - For large workloads that need to be optimized for low overhead.
    - For small workloads that have occasional burst.
    - For batch workloads.
- [EC2](EC2.md): Run containers in your EC2 infraestructure.
    - For workloads that require consistently high CPU core and memory usage.
    - For large workloads that need to be optimized for price.
    - Your applications need to access persistent storage
    - You must directly manage your infrastructure

## Clusters

An Amazon ECS cluster is a logical grouping of tasks or services. 

When creating a ECS cluster, AWS runs an [EC2 AMI](EC2.md#ami) designed for ECS, and ECS agents within that EC2 instance register it on the cluster.

### Cluster notes:
- Clusters are AWS Region specific.
- Cluster have states:
    - ACTIVE: The cluster is ready to accept tasks and you can register container instances.
    - PROVISIONING: The resources needed are being created.
    - DEPROVISIONING: The resources are being deleted.
    - FAILED: The resources needed for the capacity provider have failed to create.
    - INACTIVE: The cluster has been deleted.
- A cluster can contain a mix of tasks that are hosted on [Fargate](Fargate.md), [EC2](EC2.md) instances, or external ones.
- A cluster can contain a mix of Auto Scaling group capacity providers and Fargate capacity providers.
- Custom [IAM policies](IAM.md#policies) may be created to allow or restrict user access to specific clusters.

## Containers and images

A **container** is a standardized unit of software development that holds everything that your software application requires to run (relevant code, runtime, system tools, libraries...).

Images are read-only template, typically built from a Dockerfile, from which containers are created.

## ECS service

Runs and maintains your desired number of tasks simultaneously in an Amazon ECS cluster.

If any of your tasks fail or stop for any reason, the Amazon ECS service scheduler launches another instance based on your task definition to maintain your desired number of tasks in the service.

## ECS instances

#### Notes:
- If you stop (not terminate) an Amazon ECS container instance, the status remains ACTIVE, but the agent connection status transitions to FALSE within a few minutes.

## Tasks

A task is the instantiation of a task definition within a cluster.

Its a JSON file that describes one or more containers (max 10) that form your application.

### Task definitions

It specifies the various parameters for your application. For example, you can use it to specify the operating system parameters, which containers to use, which ports to open, what data volumes to use, etc.

After you create a task definition, you can run the task definition as a task or a service.

### Task placement constraints

A task placement constraint is a rule that's considered during task placement.

Can be specified when either running a task or creating a new service, and can be updated for existing services as well.

#### Constraints:
- **distinctInstance**
    - Place each task on a different container instance
- **memberOf**
    - Place tasks on container instances that satisfy an expression.
- **ecs.os-family**
    - LINUX or WINDOWS_SERVER_{OS-release}_{FULL/CORE}.

### Task placement strategies

A task placement strategy is an algorithm for selecting instances for task placement or tasks for termination.

Can be specified when either running a task or creating a new service, and can be updated for existing services as well.

For tasks running as part of ECS service, the default task placement strategy is `spread` using the `attribute:ecs.availability-zone`.

#### Strategies:
- **binpack**
    - Tasks are placed on container instances so as to leave the least amount of unused CPU or memory.
    - This strategy minimizes the number of container instances in use.
- **random**
    - Tasks are placed randomly.
- **spread**
    - Tasks are placed evenly based on the specified value, like:
        - instanceId or host (same effect)
        - Any platform or custom attribute that's applied to a container instance, like `attribute:ecs.availability-zone`.
        - Service tasks are spread based on the tasks from that service. Standalone tasks are spread based on the tasks from the same task group.
