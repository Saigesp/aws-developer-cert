# AWS Cognito

User authentication and authorization service for web and mobile apps.

## User Pools

User directory in Cognito.

User pools provide:
- Sign-up and sign-in services.
- A built-in, customizable web UI to sign in users.
- Social sign-in with Facebook, Google, Amazon, Apple, etc.
- User directory management and user profiles.
- Security features such as MFA, checks for compromised credentials, account takeover protection, and phone and email verification.
- Customized workflows and user migration through AWS Lambda triggers.
- User pools help you track user device, location, and IP address, and adapt to sign-in requests of different risk levels.

### Adding MFA to a user pool

Multi-factor authentication (MFA) increases security for your app. It adds a something you have authentication factor to the something you know factor of user name and password.

You can only activate MFA when you initially create a user pool.

You can choose **SMS text messages** or **Time-based One-Time Passwords (TOTP)** as second factors to sign in your users.

#### SMS text message MFA

If you activate SMS as an MFA factor, you can require that users provide phone numbers and have your users verify them during sign-up.

Notes:
- SMS for MFA is charged separately (There is no charge for sending verification codes to email addresses).
- When a user successfully goes through the SMS text message MFA flow, their phone number is also marked as verified.

#### TOTP (Time-based one-time password) MFA

When you set up TOTP software token MFA in your user pool, your user signs in with a user name and password, then uses a TOTP to complete authentication.

Notes:
- Supports software token MFA through an authenticator app.
- Doesn't support hardware-based MFA.
- When a user has not configured it, he/she receives a one-time access token that your app can use to activate TOTP MFA for the user.
- If your users have set up TOTP, they can use it for MFA even if you deactivate TOTP later.

## Identity pools

Enables users to obtain temporary, limited-privilege AWS credentials to access other AWS services.

## Federated Identities

Web service that delivers scoped temporary credentials to mobile devices and other **untrusted** environments.

It uniquely identifies a device and supplies the user with a consistent identity over the lifetime of an application.

## Cognito Sync

Service and client library that makes it possible to sync application-related user data across devices.

Amazon Cognito Sync can synchronize user profile data across mobile devices and the web without using your own backend.

## Cognito authorizer

It's an alternative to using [IAM roles](IAM.md) or [Lambda](Lambda.md) authorizers.

You can use it to authenticate users in [API Gateway](APIGateway.md), and the authentication data can be stored on [DynamoDB](DynamoDB.md).

#### Cognito authorizer vs Lambda Authorizer:
- Use [Lambda](Lambda.md) authorizer if you need custom IAM roles or own logic.
- Use cognito authorizer if you need to authenticate and authorize using Oauth.
