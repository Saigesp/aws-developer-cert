# AWS API Gateway

AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale.

Creates an API endpoint {api-id}.execute-api.{region}.amazonaws.com

API Gateway creates RESTful APIs that:
- Are HTTP-based.
- Enable stateless client-server communication.
- Implement standard HTTP methods such as GET, POST, PUT, PATCH, and DELETE.

API Gateway creates WebSocket APIs that:
- Adhere to the WebSocket protocol, which enables stateful, full-duplex communication between client and server.
- Route incoming messages based on message content.

Amazon API Gateway offers features such as the following:
- Auth mechanisms, such as [IAM policies](IAM.md#policies), [Lambda](Lambda.md) authorizer functions, and [Amazon Cognito](Cognito.md) user pools.
- [CloudTrail](CloudTrail.md) logging and monitoring of API usage and API changes.
- [CloudWatch](CloudWatch.md) access logging and execution logging, including the ability to set alarms.
- Ability to use AWS [CloudFormation](CloudFormation.md) templates to enable API creation.
- Support for custom domain names.
- Integration with [AWS WAF](WAF.md) for protecting your APIs against common web exploits.
- Integration with [AWS X-Ray](XRay.md) for understanding and triaging performance latencies.

#### Types of endpoints
- Edge-optimized API endpoint.
- Private API endpoint.
- Regional API endpoint.

## Canary release

Canary release is a software development strategy in which a new version of an API is deployed for testing purposes, and the base version remains deployed as a production release for normal operations on the same stage.

In a canary release deployment, total API traffic is separated at random into a production release and a canary release with a pre-configured ratio.

## Linear release

With the Canary Deployment Preference type, Traffic is shifted in two intervals. With **Canary10Percent5Minutes**, 10 percent of traffic is shifted in the first interval while remaining all traffic is shifted after 5 minutes. **Linear10PercentEvery5Minute** will add 10 percent traffic linearly to the new version every 5 minutes. So, after 50 minutes all traffic will be shifted to the new version.

## CORS

When using a proxy integration with Lambda, it is necessary to add “Access-Control-Allow-Headers” and “Access-Control-Allow-Origin” headers the response in the Lambda function as a proxy integration will not return an integration response.

Steps:
- Enable CORS.
- Setup the OPTIONS method and set up the required OPTIONS response headers.
- Make changes to your backend to return “Access-Control-Allow-Headers” and “Access-Control-Allow-Origin” headers.

## Cache

A client of your API can invalidate an existing cache entry from the endpoint for individual requests. The client must send a request that contains the `Cache-Control: max-age=0` header. The client receives the response directly from the integration endpoint instead of the cache, provided that the client is authorized to do so. This replaces the existing cache entry with the new response, which is fetched from the integration endpoint.

To invalidate this, you have to impose **InvalidateCache** policy or you can check the **Require authorization** checkbox.