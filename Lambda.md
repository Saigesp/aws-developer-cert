# AWS Lambda

Compute service that lets you run code without provisioning or managing servers.

#### Supported languages:
- Java
- C#
- Python
- Go
- Javascript (Node JS)
- Ruby ?

#### Capacity and limits
- 128MB memory, 3GB max.
- 3 seconds running per call, 900 seconds max.

## Function

A function is a resource that you can invoke to run your code.

### Function handler

The Lambda function handler is the method in your function code that processes events. [See documentation here](https://docs.aws.amazon.com/lambda/latest/dg/python-handler.html)

```python
def handler_name(event, context): 
    """
    :param event: dict that the trigger sends (different formats)
    :param context: object with methods and properties for env. execution
    """
    # your code here

    return some_value # format also specific by trigger
```
> See [`context` documentation](https://docs.aws.amazon.com/lambda/latest/dg/python-context.html) for more info.

### Layer

A Lambda layer is a convenient way to package libraries and other dependencies that you can use with your Lambda functions. It's a ZIP file, so it reduces the size of uploaded deployment archives and makes it faster to deploy.

It's extracted into the `/opt` directory when setting up the execution environment.

You can include up to 5 layers per function.

### Environment variables

Pair of key-value string stored in a function's **version-specific** configuration.

To configure encryption for your environment variables use [KMS](KMS.md)

## Lambda authorizer

It's an alternative to using [IAM roles](IAM.md) or [Cognito](Cognito.md) authorizers.

You can use it to authenticate users in [API Gateway](APIGateway.md), and the authentication data can be stored on [DynamoDB](DynamoDB.md).

Two types:
- **Token-based**
    - Receives the caller's identity in a bearer token or an OAuth token.
- **Request parameter**
    - Receives the caller's identity in a combination of headers, query string parameters, stageVariables, and $context variables.

> For WebSocket APIs, only request parameter-based authorizers are supported.

#### Cognito authorizer vs Lambda Authorizer:
- Use [Lambda](Lambda.md) authorizer if you need custom IAM roles or own logic.
- Use cognito authorizer if you need to authenticate and authorize using Oauth.

## Lambda@Edge

Lambda@Edge is a compute service that lets you execute functions that customize the content that [CloudFront](CloudFront.md) delivers. You can author functions in one region and execute them in AWS locations globally closer to the viewer, without provisioning or managing servers.

## Best practices

- Function code
    - Code quality
        - Separate the Lambda handler from your core logic for unit-testable code.
        - Use environment variables to pass operational parameters.
        - When using SQS as event source, make sure the value of the function's expected invocation time does not exceed the Visibility Timeout value on the queue.
    - Performance
        - Always avoid recursive code (Important!).
        - Minimize your deployment package size and its dependencies.
        - Initialize SDK clients and database connections outside of the function handler and cache static assets locally in the `/tmp` directory.
        - Use a keep-alive directive to maintain persistent connections.
- Function configuration
    - Security
        - Use most-restrictive permissions when setting IAM policies
    - Quotas
        - Delete Lambda functions that you are no longer using (they count against your deployment package size limit).
- Metrics and alarms
    - Leverage your logging library and Lambda Metrics and Dimensions to catch app errors
    - Use AWS Cost Anomaly Detection to detect unusual activity.
- Working with streams
    - Test with different batch and record sizes so that the polling frequency of each event source is tuned to how quickly your function is able to complete its task.
    - Increase Kinesis stream processing throughput by adding shards (Lambda will poll each shard with at most one concurrent invocation)