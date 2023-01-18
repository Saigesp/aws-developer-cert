# AWS Route 53

Highly available and scalable authoritative Domain Name System (DNS) web service.

You can use Route 53 to perform:
- Domain registration.
- DNS routing.
- Health checking.

## What is DNS?

DNS, or the Domain Name System, translates human readable domain names (for example, www.amazon.com) to machine readable IP addresses (for example, 192.0.2.44).

#### Types of DNS Service:
- **Authoritative DNS**: An authoritative DNS service provides an update mechanism that developers use to manage their public DNS names. Authoritative DNS has the final authority over a domain and is responsible for providing answers to recursive DNS servers with the IP address information.
- **Recursive/Resolver DNS**: Clients typically connect to recursive DNS, who can get the DNS information on your behalf. It returns the resolution name if it has it cached, or passes the query to one or more authoritative DNS servers to find the information.

#### Concepts:
- **Domain Name**: The address that users type in the browser (www.example.com).
- **Top Level Domain (TLD)**: The las part of the domain name (.com, .org, etc).
- **Subdomain**: Labels prepended to the domain name: (abc in abc.example.com).
- **Domain registrar**: Company acredited by ICANN to register domain names.
- **Domain registry**: Company that owns the right to sell specific top-level domains.
- **Name Servers**: Servers in the DNS that helps to translate domain names into IPs.
- **DNS Query**: Request for a resource associated with a domain name.
- **Time To Live (TTL)**: Time (in seconds) that the DNS server caches the values of a record.
- **DNS Record**: Maps domain or subdomains to IPs or other resources.
    - **A (Address)**: It indicates the IP address of a given domain.
    - **CNAME (Canonical Name)**: Maps domain or subdomains to another domain or subdomain.
    - **NS (Name Server)**: Specifies the servers that provides DNS services for that domain name.
    - **SOA (Start Of Authority)**: Includes administrative information about your zone.

#### Route 53-specific concepts:
- **Alias Record**: Route53 record to map domain or subdomains to AWS resources.
- **Zone Apex**: The root domain.
- **Route policy**: A settings for records to determine how Route 53 respond to DNS queries.
- **Hosted Zone**: A central container where you can create DNS records. It can be public or private (only reatched by VPC).

## Simple Routing Policy

The traffic is routed to a **single AWS resource**.

You **can not create multiple records** with the same name and type, but you can specify multiple values in the record. If you do so, Route 53 returns all values randomly to the recursive resolver.

Alias record:
- The endpoint will be an AWS resource.
- You can attach health checks to this record.

Basic/Non-Alias Record:
- You specify the IP address of individual web servers.
- You can not attach health checks.

## Weighted Routing Policy

The traffic is routed to **multiple AWS resources**.

You **can create multiple records** with the same name and type, and assign a relative weight to determine how much traffic you want to send to each resource (traffic routed = record weight / total weight).

## Latency Routing Policy

Helps serving the request from the AWS Region that provides the lowest latency.

Used for applications deployed in multiple AWS Regions.