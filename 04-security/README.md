# AWS Security 🔐

This folder contains notes about the fundamental security concepts used in Amazon Web Services (AWS).

The topics covered here include the AWS Shared Responsibility Model, Identity and Access Management (IAM), permissions, password policies, and Multi-Factor Authentication (MFA).

## Topics

* Shared Responsibility Model
* AWS Identity and Access Management (IAM)
* IAM Users
* IAM Groups
* IAM Policies
* IAM Roles
* Principle of Least Privilege
* IAM Password Policy
* Multi-Factor Authentication
* IAM hands-on exercises

## Folder Structure

```text
04-security/
│
├── README.md
├── 01-shared-responsibility-model.md
├── 02-iam.md
├── 03-users-and-groups.md
├── 04-iam-policies.md
├── 05-iam-password-policy.md
├── 06-multi-factor-authentication.md
│
└── hands-on/
    ├── 01-iam-users-and-groups.md
    ├── 02-iam-policies.md
    └── 03-iam-mfa.md
```

## Security Mental Model

AWS security can be understood through two main areas of responsibility:

```text
                    AWS SECURITY
                         │
             ┌───────────┴───────────┐
             │                       │
            AWS                   CUSTOMER
             │                       │
     Security OF the cloud    Security IN the cloud
```

### AWS

AWS is responsible for protecting the infrastructure that runs AWS services.

### Customer

The customer is responsible for securely configuring and using the AWS services and resources they operate.

## IAM Mental Model

IAM focuses on identity and access:

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

In simple terms:

> Who are you?

> What are you allowed to do?

> Which resources can you access?

## Security Principles

Some important principles covered in this section include:

### Least Privilege

Give identities only the permissions they actually need.

### Defense in Depth

Use multiple layers of security instead of relying on a single control.

### Strong Authentication

Use strong credentials and MFA to reduce the risk of unauthorized access.

### Secure Configuration

Security depends not only on AWS infrastructure, but also on how customers configure their own resources.

## Key Takeaway

> AWS provides a secure cloud infrastructure, but customers must securely configure and use the services they consume.
