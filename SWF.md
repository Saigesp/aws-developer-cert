# AWS SWF - Simple Workflow Service

Task distribution service.

## Tasks

Logical unit of work that is performed by a component of your application.

You can create tasks that are long-running, or that may fail, time out, or require restarts -or that may complete with varying throughput and latency.

Amazon SWF stores tasks and assigns them to workers when they are ready, tracks their progress, and maintains their state, including details on their completion.

## Workers

Workers perform tasks. Can run either on cloud infrastructure or on-prem.

## Actors

- Starter: any app that can initiate a workflow.
- Decider: an implementation of the workflow's coordination logic.
- Activity workers: a process or thread that performs the activity task.