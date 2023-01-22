# AWS X-Ray

Monitoring service that collects **requests** data that your application serves, and provides tools to view, filter, and gain insights.

For any traced request to your application, you can see detailed information about:
- The request & the response.
- App calls to downstream resources, microservices, databases, and web APIs.

X-Ray receives data from services as **segments** and groups them that have a common request into **traces**, to process them and generate a **service graph**.

Trace data sent to X-Ray is generally available to consume within **30 seconds**, and is stored for **30 days**.

## Segments

Data sent from resources about their work. Provides:
- The resource's name.
- Details about the request.
- Details about the work done.

### Subsegments

A segment can break down the data about the work done into subsegments, and can contain additional details.

## Traces

A trace collects all the segments generated by a single request.

A trace ID tracks the path of a request through your application.

## Daemons

Instead of sending trace data directly to X-Ray, each client SDK sends JSON segment documents to a daemon process listening for UDP traffic.

The X-Ray daemon buffers segments in a queue and uploads them to X-Ray in batches. The daemon is available for Linux, Windows, and macOS, and is included on [Elastic Beanstalk](ElasticBeanstalk.md) and [Lambda](Lambda.md).
