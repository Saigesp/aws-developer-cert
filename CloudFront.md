# AWS CloudFront

A CDN service that speeds up distribution of your **static** and dynamic web content.

CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the request is routed to the edge location that provides the lowest latency.

#### How it works:
- If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately.
- If the content is not in that edge location, CloudFront retrieves it from the origin and stored it for future requests (for 24H defaultly).

### Caching duration and minimum TTL

- Use the default value of 24 hours.
- Configure your origin to add a `Cache-Control` or `Expires` header field to each object.
- Specify a value for Minimum TTL.

### Maximum cacheable file size

CloudFront saves in its cache is 30 GB.

## Edge locations

### Regional Edge Cache

The regional edge caches are located between your origin web server and the global edge locations that serve content directly to your viewers.

It's enabled by default.

### Geo Restriction

You can specify a list of allowed countries from which your users can access your content.

It has a 99.8% accuracy.

### Remove an item from edge location

Use the Invalidation API or wait the item to expire.
