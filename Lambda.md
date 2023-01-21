# AWS Lambda

Compute service that lets you run code without provisioning or managing servers.

Lambda runs your code only when needed and scales automatically, from a few requests per day to thousands per second.

Lambda runs your code on a high-availability compute infrastructure and performs all of the administration of the compute resources (maintenance, capacity provisioning, scaling and logging).

#### Supported languages:
- Java
- C#
- Python
- Go
- Javascript (Node JS)
- Ruby ?

#### Default capacity
- 128MB memory.
- 3 seconds running per call.

#### Limitations (2018):
- 3GB maximum memory.
- 900 seconds running per call.

## Lambda function handler

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

## Lambda layer

A Lambda layer is a .zip file archive that can contain additional code or other content. A layer can contain libraries, a custom runtime, data, or configuration files.

It's a convenient way to package libraries and other dependencies that you can use with your Lambda functions. (it reduces the size of uploaded deployment archives and makes it faster to deploy your code).

You can include up to 5 layers per function.

## Step functions

Step Functions provide a reliable way to coordinate components and step through the functions of your application. 

AWS Step Functions is a web service that enables you to coordinate the components of distributed applications and microservices using visual workflows. You build applications from individual components that perform a discrete function, or task, allowing you to scale and change applications quickly.

## Lambda@Edge

Lambda@Edge is a compute service that lets you execute functions that customize the content that CloudFront delivers. You can author functions in one region and execute them in AWS locations globally closer to the viewer, without provisioning or managing servers.

Lambda@Edge scales automatically, from a few requests per day to thousands per second. Processing requests at AWS locations closer to the viewer than on origin servers significantly reduces latency and improves the user experience.