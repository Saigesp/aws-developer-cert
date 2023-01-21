# AWS Cloud9

A web browser IDE.

A computing resource (for example, an Amazon EC2 instance or your own server) connects to that environment.

### How it works
AWS Cloud9 IDE runs in a web browser on your local computer and interacts with your Cloud9 environment, storing your work in an [CodeCommit](CodeCommit.md) repository or other type of remote repository.

Behind the scenes, there are a couple of ways you can connect your environments to computing resources:
- **EC2 environment**: Cloud9 creates a EC2 instance and then connect the environment to that instance.
- **SSH environment**: Cloud9 connects an environment to an existing cloud compute instance or to your own server.

#### Notes:
- Multiple users can work on the same environment at same time (like google drive).
- It uses AWS Managed Temporary Credentials to access AWS resources.

### VPC Criteria
- The VPC can be in the same or different AWS account.
- The VPC has to be on the same Region as the Cloud9 environment.
- The VPC must have a public subnet.
- The subnet must have a route table with a minimun set of rules.
- The associated Security Groups for the VPC must allow a minimum set of inbound and outgoing traffic.
- If the VPC has a Network Access Control List (NACL), the NACL must allow a minimum set of inbound and outgoing traffic.