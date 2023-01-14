# AWS ELB - Elastic Load Balancer

A load balancer accepts the traffic to your application and distributes it to the registered targets (EC2 instances, containers and IP addresses) such that none of them gets overloaded, reuccing the posibilities of a bottleneck. (_Elastic_ means that the load balancer automatically scales).

Handles the application layer traffic (HTTP) and network layer traffic (TCP).

AWS ELB monitors the health of its registered targets and ensures that it routes the traffic only to **healthy targets**.

With an ELB you can add of remove resources from your balancer without disrupting the service.

It can be managed from AWS Management Console, CLI, SDK or Query API.

Hybrid Load Balancing: it balances across AWS and on-premises resources.

#### Types:
- Application Load Balancer.
- Network Load Balancer.
- Classic Load Balancer.

### Important notes:
- When you enable an Availability Zone (AZ) for your load balancer, ELB creates a load balancer node in the AZ. If you register targets in your AZ but do not enable the AZ, these targets **do not receive traffic**.
- Cross-Zone Load Balancing: it routes the traffic to its registered targets in one or more AZ. It's enabled by default for Application Load Balancer and disabled by default for Network Load Balancer.
- The most effective way of using a load balancer is to ensure that it encompasses multiple AZ and each AZ has as least one registered target.
- If you disable a AZ, the targets in that AZ remains registered with the load balancer, but it will not route traffic to them.
- Listeners: You configure your load balancer to accept incoming traffic by specifying one or more listeners.

## ALB - Application Load Balancer

Accepts the HTTP/HTTPS traffic from clients.

#### Characteristics:
- High availability.
- Internet-facing or Internal.
- Health checks.
- Supports SSL.
- Cross-Zone Load Balancing is enabled.

#### Main components:
- Listeners:
    - Process that checks for connection requests, using the given protocol and port.
    - Each listener has a default rule, and more can be added. Default rule cannot have conditions and cannot be removed.
    - You need to configure SSL server certificate on ALB if the listener is configured for HTTPS.
    - Provide native support for WebSockets (HTTP and HTTPS) and HTTP/2 with HTTPS listeners.
- Rules:
    - Determines where the traffic will be forwarded.
    - Support HTTP and HTTPS and ports 1-65535.
    - They have priority (1, 2... the lower the higher), one or more actions, an optional host condition, and an optional path condition.
    - Conditions can be host conditions or path conditions
    - There is also a provision for a fixed-response action.
- Health Checks.
    - Monitors the health of the registered targets.
    - Health checks are performed on all targets registered in a target group that is specified in a listener rule.
- Targets and Target Groups.
    - Target groups are used to route requests to one or more registered targets.
    - You can configure different target groups for different types of requests.
    - Each Target Group has its own health checks.
    - Attaching a target group to an auto scaling group enables you to scale each service dynamically based on demand.
    - _Slow start mode_ gives targets time to warn up before the load balancer send them a full share of requests.

#### How it selects the target:
1. Evaluates the **listener** rules in **priority order** to determine which rule to apply.
2. Monitors the health of the registered targets.
3. Selects a healthy target from the target group.

#### Security policy notes:
- SSL Offload: You can deploy SSL certificate on the ALB itself.
- Security policy: Combination of protocols and ciphers to negotiate SSL connections between the client and the load balancer.