# AWS API Gateway

REST, HTTP, and WebSocket APIs service.

#### Main features:
- Support for custom domain names.
- Auth mechanisms:
    - [IAM policies](IAM.md#policies).
    - [Lambda](Lambda.md) authorizer functions.
    - [Amazon Cognito](Cognito.md) user pools.
- [CloudWatch](CloudWatch.md), [CloudTrail](CloudTrail.md) and [X-Ray](XRay.md) integration.
- Can be used on [CloudFormation](CloudFormation.md).
- [WAF](WAF.md) integration.

#### RESTful APIs:
- HTTP-based.
- Enable stateless client-server communication.
- Standard HTTP methods (GET, POST, PUT, PATCH, and DELETE).

#### WebSocket APIs:
- Adhere to the WebSocket protocol.
- Stateful, full-duplex communication.
- Route incoming messages based on message content.

#### Types of endpoints
- **Edge-optimized**.
    - Default. Best for geographically distributed clients.
- **Regional**.
    - Intended for clients in the same region
- **Private**.
    - Can only be accessed from [VPC](VPC.md)

#### Types of integrations
- **AWS**
    - Expose AWS service actions.
    - Request and response has to be configured.
- **AWS_PROXY**
    - Integrated with a [Lambda](Lambda.md) function.
    - No request and response configuration.
- **HTTP**
    - Expose HTTP endpoints in the backend.
    - Request and response has to be configured.
- **HTTP_PROXY**
    - Allows a client to access the backend HTTP endpoints.
    - No request and response configuration.
- **MOCK**
    - Return a response without sending the request to the backend.

## Endpoint

With default domain name: `http://{api-id}.execute-api.{region}.amazonaws.com/`

With custom name: `http://{example.com}/`

With stage variable: `http://example.com/${stageVariables.v2}/bar`

## Stages

A stage is a reference to a state of your API (dev, v2, etc).

### Stage variables

Stage variables are key-value pairs that act like environment variables and can be used in your setup.

You can use a stage variable as part of an HTTP integration URI:

- A full URI without protocol
    - Variable value: `example.com/lorem`
    - Example: `${stageVariables.<name>}`
- A full domain
    - Variable value: `example.com`
    - Example: `${stageVariables.<name>}/resource/operation`
- A subdomain
    - Variable value: `mysubdomain`
    - Example: `${stageVariables.<name>}.example.com/resource/operation`
- A path
    - Variable value: `api/v1`
    - Example: `example.com/${stageVariables.<name>}/bar`
- A query string
    - Variable value: `lorem`
    - Example: `example.com/foo?q=${stageVariables.<name>}`

## API Deployments & releases

A deployment is a snapshot of your API configuration. After you deploy an API to a stage, it’s available for clients to invoke.

You **must redeploy an API** for changes to take effect.

### Canary release

A new version of an API is deployed (base version remains) and traffic is splitted at random into both releases with a pre-configured ratio.

### Linear release

A new version of an API is deployed (base version remains) and traffic is splitted linearly. Example: **Linear10PercentEvery5Minute** will add 10 percent traffic linearly to the new version every 5 minutes (after 50 minutes all traffic will be shifted to the new version).

## Authentication

Users can authenticate with [Lambda](Lambda.md) authorizers, or [Cognito](Cognito.md) authorizers.

#### Cognito authorizer vs Lambda Authorizer:
- Use [Lambda](Lambda.md) authorizer if you need custom IAM roles or own logic.
- Use cognito authorizer if you need to authenticate and authorize using Oauth.

## CORS

Disabled CORS is one of the common mistakes in configuration. CORS always use the following headers:
- "Access-Control-Allow-Headers"
- "Access-Control-Allow-Origin"

When using Javascript to call API Gateway, ensure that CORS is enabled and both headers are being passed.

When using a proxy integration with Lambda, ensure that both headers are present in the Lambda function response.

## Cache

Caching can be enabled for API Gateway. It's done at **stage level** and only GET methods have caching enabled by default.

The default TTL is 300 seconds, and the maximun is 3600 seconds.

Caching is charged by the hour based on the selected cache size.

### Disable-cache at client-side

The client must use the header `Cache-Control: max-age=0` (if policy allows it).

### Invalidate disable-cache at client-side

- Impose **InvalidateCache** policy.
- Check the **Require authorization** checkbox in the console.
