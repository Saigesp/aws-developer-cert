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

## Geolocation Routing Policy

Lets you choose the resources that serve your traffic based on the geographic location of your users. You can specify geographic locations by continent, by country, or by state in the United States.

If Route 53 cannot localize the origin traffic, it will route the traffic to the default record.

## Failover Routing Policy

Lets you route traffic to a resource when the resource is healthy or to a different resource when the first resource is unhealthy.

Health checks can monitor:
- The health of a specified resource.
- The status of other health checks.
- The status of an [CloudWatch](CloudWatch.md) alarm.

You can use failover routing policy for records in a private hosted zone.

## Multivalue Answer Routing Policy

Lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries, allowing to check the health of each resource.

To route traffic approximately randomly to multiple resources, you create one multivalue answer record for each resource and, optionally, associate a Route 53 health check with each record.

Notes:
- If you associate a health check with a multivalue answer record, Route 53 responds to DNS queries with the corresponding IP address only when the health check is healthy.
- If you don't associate a health check with a multivalue answer record, Route 53 always considers the record to be healthy.
- If you have eight or fewer healthy records, Route 53 responds to all DNS queries with all the healthy records.
- When all records are unhealthy, Route 53 responds to DNS queries with up to eight unhealthy records.