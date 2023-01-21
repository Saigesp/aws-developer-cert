# AWS CodeCommit

Version control service hosted by AWS that you can use to privately store and manage assets (such as documents, source code, and binary files) in the cloud.

#### Important Notes
- Date in transit and rest is encrypted by default.
- Access to repositories is made with:
    - Git credentials for AWS CodeCommit for [IAM user](IAM.md) (under IAM config).
    - SSH keys for AWS CodeCommit for IAM user.
    - [AWS CLI](CLI.md) Credential Helper:
        - Cryptographically signed version of IAM user credentials
        - EC2 instance Role access
- Can trigger [SNS](SNS.md) notifications or [Lambda](Lambda.md) functions on changes.
    - Notification events:
        - Pull request updates
        - Pull request comments
        - Commit comments
    - Trigger events:
        - Push to existing branch
        - Create branch (or tag)
        - Delete branch (or tag)
