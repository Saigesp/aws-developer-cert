# KMS - Key Management Service

Managed service that makes it easy for you to create and control the cryptographic keys that are used to protect your data.

KMS integrates with most other AWS services that encrypt your data. KMS also integrates with AWS CloudTrail to log use of your KMS keys for auditing, regulatory, and compliance needs.

Customer Master Keys are protected by FIPS 140-2 validated cryptographic modules.

## KMS keys

KMS keys are the primary resource in KMS. You can use a KMS key to encrypt, decrypt, and re-encrypt data. It can also generate data keys that you can use outside of KMS.

#### Notes: 
- Encrypts up tp 4KB of data
- Contains metadata information:
    - Key ID
    - Key spec
    - Key usage
    - Key description
    - Key state
    - Creation date
- They can be:
    - Symmetric
    - Asymmetric


### Key ARN

```
arn:{partition}:kms:{region}:{accountId}:key/{key-id}
```

### Multi-Region keys

KMS keys in different AWS Regions that can be used interchangeably. Each set of related multi-Region keys has the same key material and key ID, so you can encrypt data in one AWS Region and decrypt it in a different AWS Region.

You must manage each multi-Region key independently, including creating aliases and tags, setting their key policies and grants, and enabling and disabling them selectively.

### Customer keys and AWS keys

The KMS keys that you create are **customer managed keys**. KMS keys that AWS services create in your AWS account are **AWS managed keys**. KMS keys that AWS services create in a service account are **AWS owned keys**.

| Type | Can view | Can manage | Used only for my account | Automatic rotation | Pricing |
| --- | --- | --- | --- | --- | --- |
| Customer managed | Yes | Yes | Yes | Optional & Yearly | Per-use fee |
| AWS managed      | Yes | No  | Yes | Required & Yearly | Per-use fee |
| AWS owned        | No  | No  | No  | Varies            | No fees     |

## Key spec

Key spec is a property that represents the cryptographic configuration of a key.

- **AWS KMS keys**.
    - The spec determines whether the KMS key is symmetric or asymmetric.
    - It also determines the type of its key material, and the algorithms it supports.
    - You choose the key spec when you create the KMS key, and **you cannot change it**.
    - The default key spec, SYMMETRIC_DEFAULT, represents a 256-bit symmetric encryption key.
- Data keys
    - The spec determines the length of an AES data key.
- Data keys pairs
    - The spec determines the type of key material in the data key pair.

## Data keys

Data keys are symmetric keys you can use to encrypt data, including large amounts of data and other data encryption keys. Unlike symmetric KMS keys, data keys are returned to you for use outside of KMS.

When KMS generates data keys, it returns a plaintext data key for immediate use and an encrypted copy of the data key that you can safely store with the data. When you are ready to decrypt the data, you first ask KMS to decrypt the encrypted data key.

KMS does not store, manage, or track your data keys, or perform cryptographic operations with data keys.

### Key usage

Key usage is a property that determines the cryptographic operations the key supports:
- ENCRYPT_DECRYPT
- SIGN_VERIFY
- GENERATE_VERIFY_MAC

Each KMS key can have only one key usage.

### Envelope encryption

Envelope encryption is the practice of encrypting plaintext data with a data key, and then encrypting the data key under another key.

The top-level encryption key is known as the root key.

## Data key pairs

Data key pairs are asymmetric data keys consisting of a mathematically-related public key and private key. They are designed for use in client-side encryption and decryption or signing and verification outside of AWS KMS.

## Data key caching

Data key caching can improve performance, reduce cost, and help you stay within service limits as your application scales.

#### How it works
Data key caching stores data keys and related cryptographic material in a cache. When you encrypt or decrypt data, the AWS Encryption SDK looks for a matching data key in the cache. If it finds a match, it uses the cached data key rather than generating a new one.

## KMS Actions

Most common & asked actions are:

- **CreateKey**.
    - Creates a unique customer managed KMS key in your AWS account and Region.
- **GenerateDataKey**
    - Returns a plaintext data key and encrypted data key (the same data key).
    - You can use the plaintext key to encrypt your data and store the encrypted data key with the encrypted data.
- **GenerateDataKeyWithoutPlaintext**.
    - Returns a encrypted data key.
    - Useful to encrypt data not immediately (Then you decrypt the data key with Decrypt action)
- **Encrypt**.
    - Encrypts data of up to 4 KB.
    - Record the key and algorithms used!.
- **Decrypt**.
    - Decrypts ciphertext that was encrypted by a KMS.