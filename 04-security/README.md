# AWS Security 🔐

Security is a shared responsibility between AWS and the customer.

This folder covers the fundamental AWS security concepts required to understand how AWS protects its infrastructure and how customers are responsible for securing what they run in the cloud.

## Topics

1. Shared Responsibility Model
2. AWS Identity and Access Management (IAM)
3. Users, Groups, and Policies
4. Root User
5. Multi-Factor Authentication (MFA)

## Core Ideas

* AWS secures the underlying cloud infrastructure.
* Customers are responsible for security **in** the cloud.
* IAM controls access to AWS resources.
* IAM users represent identities that can authenticate to AWS.
* Groups can organize users and simplify permission management.
* Policies define permissions.
* The Root User has full access to the AWS account.
* MFA adds an additional authentication factor.

## Security Mental Model

```text
AWS Security
     │
     ├── AWS responsibility
     │      └── Security OF the cloud
     │
     └── Customer responsibility
            └── Security IN the cloud
```

## Key Takeaway

> AWS is responsible for security **of** the cloud.
> The customer is responsible for security **in** the cloud.

---
