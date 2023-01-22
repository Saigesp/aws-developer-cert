# AWS CloudFront

A CDN service that speeds up distribution of your **static** and dynamic web content.

CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the request is routed to the edge location that provides the lowest latency.

#### How it works:
- If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately.
- If the content is not in that edge location, CloudFront retrieves it from the origin and stored it for future requests.