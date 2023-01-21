# AWS CodePipeline

Continuous delivery service that enables you to model, visualize, and automate the steps required to release your software.

#### Notes:
- **Stages**.
    - Each step (build, deploy...) of the CI/CD pipeline.
    - Are executed sequentialy.
- **Actions**
    - Stages contains actions (tasks performed on the artifact):
    - Can be sequential or parallel.
    - Types are: Source, Build, Test, Deploy, Approval (require manual approval) or Invoke (Call a [Lambda](Lambda.md) function).
    - Depending on th etype, the action might have one or both of:
        - An input artifact.
        - An output artifact.
- **Artifacts**:
    - Stored in S3 bucket (named `codepipeline-{region}-{your-account-id}` by default).

## How it works (example)
1. Developer push code. Event is triggered by:
    - [CloudWatch](CloudWatch.md) Events.
    - CloudWatch Scheduler.
    - Github WebHooks.
    - CodePipeline Polling (periodict checks).
2. Source Stage actions are executed.
    - Ex: Compress source and upload it to [S3](S3.md).
3. Build Stage actions are executed.
4. Test Stage actions are executed.
5. Approval actions are executed.
    - Require manual approval from an user.
6. Deploy actions are executed.

## Custom actions

You can create custom actions for the following categories: build, deploy, test and invoke.

You must also create a **job worker** that will poll CodePipeline for job requests for this custom action, execute the job, and return the status result. This job worker can be located on any computer or resource as long as it has access to the public endpoint for CodePipeline.

## Best practices

See [all CodePipeline best practices here](https://docs.aws.amazon.com/codepipeline/latest/userguide/best-practices.html)

### Security

- Configure server-side encryption for artifacts stored in S3 by managing [KMS](KMS.md)-managed keys (SSE-KMS)
- Configure git authentication using AWS-managed OAuth token or customer-managed personal token

### Monitoring and logging

- Use [CloudTrail](CloudTrail.md) to log API calls and related events made by or on behalf of a AWS account.
- Use [CloudWatch](CloudWatch.md) Events to monitour your resources and the application.

### Jenkins plugins

- Install Jenkins on an [EC2](EC2.md) instance and configure a separate EC2 instance profile. Make sure the instance profile grants Jenkins only the permissions required for perform tasks on your project.