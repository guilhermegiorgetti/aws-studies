# AWS IAM 🔑

AWS Identity and Access Management (IAM) is a service used to securely control access to AWS resources.

IAM helps determine:

* **Who** can access AWS
* **What** they can do
* **Which resources** they can access

## IAM Mental Model

```text
Identity
   │
   ▼
Authentication
   │
   ▼
Authorization
   │
   ▼
AWS Resource
```

### Authentication

Authentication answers:

> **Who are you?**

For example:

* Username and password
* Access keys
* MFA

### Authorization

Authorization answers:

> **What are you allowed to do?**

Permissions determine which actions an identity can perform.

## IAM Is a Global Service

IAM is not tied to a specific AWS Region in the same way as Regional services such as EC2.

IAM is used across the AWS account to manage identities and permissions.

## Principle of Least Privilege

One of the most important security principles is **least privilege**.

It means:

> Give a user, application, or service only the permissions required to perform its task.

### Bad Example

A developer only needs to read objects from an S3 bucket but receives full administrator permissions.

```text
Required:
S3 Read

Granted:
Administrator Access ❌
```

### Better Example

```text
Required:
S3 Read

Granted:
Only the required S3 read permissions ✅
```

Least privilege reduces the potential impact of compromised credentials or accidental actions.

## IAM Components

The main IAM concepts are:

```text
IAM
│
├── Users
│
├── Groups
│
├── Policies
│
└── Roles
```

## IAM Users

An IAM user represents an identity within an AWS account.

A user can have permissions assigned through policies, directly or through groups.

## IAM Groups

Groups allow multiple IAM users to be managed together.

Instead of assigning the same permissions individually:

```text
User A ──┐
User B ──┼── Developers Group ── Policy
User C ──┘
```

This simplifies permission management.

## IAM Policies

Policies are documents that define permissions.

A policy can specify:

* Effect
* Action
* Resource
* Conditions

Simplified example:

```text
Effect: Allow
Action: s3:GetObject
Resource: specific S3 objects
```

## IAM Roles

An IAM role is an identity with permissions that can be assumed by trusted entities.

Roles are commonly used by:

* AWS services
* Applications
* EC2 instances
* Users or identities that need temporary access

For example, an EC2 instance can assume a role that allows it to access an S3 bucket without storing long-term AWS access keys on the server.

## Key Takeaways

* **IAM = identity and access management**
* Authentication = who you are
* Authorization = what you can do
* Policies define permissions
* Groups organize users
* Roles provide permissions to trusted entities
* Use least privilege
