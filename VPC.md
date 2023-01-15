# AWS VPC - Virtual Private Cloud

Private network to create resources in it, logically separated from other VPCs.

### Main concepts

- **Region**:
    - A VPC belongs to a single region, but multiple VPC can exists on a single region.
    - A VPC can span multiple AZ.
- **CIDR (Classless Inter-Domain Routing)**:
    - A method for  allocating IP addresses and IP routing.
    - When creating a VPC, a range of IPv4 has to be specified in the form of a CIDR block (ex 10.0.0.0/16 -> primary CIDR block for the VPC).
    - CIDR notation is a compact representation of an IP address and its associated routing prefix (ex 10.0.0.0/24 represents the IP 10.0.0.0, its subnet mask 255.255.255.0 which has 24 leading 1-bits, and its routing prefix -> `2 ^ (32 - 24) = 256` addresses available).
- **Subnet**:
    - A part of the VPC that also has CIDR blocks
    - A subnet can only be inside a single AZ.
    - You need to create one or more subnets inside your VPC to be able to launch the resources.
    - Typically you will create a _private subnet_ (not exposed to internet or outside the VPC) and a _public subnet_ (exposed to internet).
    - The subnets inside a VPC cannot have overlapping IP address ranges
- **NAT Instance (Network address Translation)**:
    - Mechanism that contains a physical device acting as a mediador between the instances inside the network and the internet.
    - It is just an EC2 instance with NAT capability which stays in a public subnet.
    - When an instance inside the network request someting from internet, the NAT device keeps track of the private IP address of the instance, and makes the request with its own public IP, hidding the instance address. When the requested site replies, the NAT check what instance has done the request, and returns the information to it. This way the private IP address of the instance is never exposed to the internet.
- **NAT Gateway**:
    - AWS managed NAT device.
    - Highly available and scalable.
    - Works on the subnet level.
- **Internet gateway (IGW**:
    - A horizontally scaled, redundant and highly available VPC component that allows communication between instances in your VPC and the internet.
    - Purposes:
        - Provides a target in your VPC route tables for internet-routable traffic.
        - Performs network address translations (NAT) for instances that have been assigned public IPv4 addresses.
    - Works on the VPC level.
- **Route table**:
    - Contains a set of rules (routes) used to determine where the traffic is directed.
    - Each route in a table specifies a destination CIDR and a target.
    - Each subnet must be associated with a route table, which controls the routing for the subnet. If you don't explicity associate a subnet with a route talbe, the subnet is implicity associated with the main route table.
    - You cannot delete a main route table, but they can be replaced.
    - Every route table contains a local route for communication within the VPC over IPv4.
    - When you add an Internet Gateway an egress-only Internet Gateway, a Virtual Private Gateway, a NAT device, a peering Connection or a VPC endpoint in your VPC, you must update the route table for any subnet that uses these gateways or connections.
- **Security Groups**:
    - A set of "firewall" rules that control the traffic for your instance.
    - They define which port and protocols are allowed for incoming and outgoing traffic for the instance
    - Works at instance level, so they are the first layer of defence for an instance.
    - Stateful: if a port is open for incoming traffic, it is automatically open for outgoing traffic.
- **NEtwork Access Control List (NACL)**:
    - Consist of ordered rules, which contains:
        - Rule number (the lowest, the sooner that the rule is evaluated).
        - Protocol (80 for HTTP, 443 for HTTPS...).
        - Traffic source (CIDR range) and destination port or port range (listening) [for inbound rules].
        - Traffic destination (CIDR range) and destination port or port range [for outbound rules].
        - Action (ALLOW or DENY).
    - Works at the subnet level. -> Second layer of defence.
    - Stateless: if a port is open for incoming traffic, it is not automatically open for outgoing traffic.

### Typical VPC diagram

![img](img/aws-vpc-diagram.png)
