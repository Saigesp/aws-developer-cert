# AWS CLI - Command Line Interface

[Install AWS CLI here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

## Configure

To configure users:
```
aws configure
```

To list configurations:
```sh
aws configure list
```

To see user configuration:
```sh
aws configure list --profile <username>
```

To set user as default in the current terminal:
```sh
export AWS_PROFILE=<username>
```

## S3

### List objects on bucket
```sh
aws s3 ls s3://<bucket-name>/<folder> --recursive --summarize --human-readable
```

## API Gateway

### Create a deployment
```sh
aws apigateway create-deployment
    --region <region> \
    --rest-api-id <id>
```
> The API is not callable until you associate this deployment with a stage.

Extra parameters:
- `--stage-name` create a stage.
- `--canary-settings` to configure Canary deployment.

### Associate deployment with a stage
```sh
aws apigateway update-stage
    --region <region> \
    --rest-api-id <id> \ 
    --stage-name <name> \ 
    --patch-operations op='replace',path='/deploymentId',value='<deployment-id>'
```

## DynamoDB

### Create a table
```sh
aws dynamodb create-table \
    --table-name Music \
    --attribute-definitions \
        AttributeName=Artist,AttributeType=S \
        AttributeName=SongTitle,AttributeType=S \
    --key-schema \
        AttributeName=Artist,KeyType=HASH \
        AttributeName=SongTitle,KeyType=RANGE \
    --provisioned-throughput \
        ReadCapacityUnits=5,WriteCapacityUnits=5 \
    --table-class STANDARD
```

### Read
```sh
aws dynamodb get-item \
    --table-name <name> \
    --key '{ ...filters}'
```

### Create
```sh
aws dynamodb put-item \
    --table-name <name>  \
    --item "{...data}"
```

### Update
```sh
aws dynamodb update-item \
    --table-name <name> \
    --key '{...filters}' \
    --update-expression "SET <field> = :newval" \
    --expression-attribute-values '{...data}' \
```
