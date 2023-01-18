# AWS CLI - Command Line Interface

[Install AWS CLI here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

## Configure

To configure users:
```
aws configure
```

To list configurations:
```
aws configure list
```

To see user configuration:
```
aws configure list --profile <username>
```

To set user as default in the current terminal:
```
export AWS_PROFILE=<username>
```

## S3

List objects on bucket
```
aws s3 ls s3://<bucket-name>/<folder> --recursive --summarize --human-readable
```
