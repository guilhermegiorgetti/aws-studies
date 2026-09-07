# AWS Root User 👑

The Root User is the identity created when an AWS account is initially created.

It has complete access to all AWS resources and services in the account.

## Root User Credentials

The Root User is associated with the email address and password used to create the AWS account.

The Root User is different from an IAM user.

```text
AWS Account
│
├── Root User
│
└── IAM Users
```

## Best Practice

The Root User should **not** be used for everyday AWS administration.

Instead:

* Use IAM identities for normal work.
* Protect the Root User with MFA.
* Avoid creating unnecessary Root User access credentials.
* Use the Root User only when an action specifically requires it.

## Important Exam Concept

> Root User has unrestricted access to the AWS account.

Therefore, protecting the Root User is extremely important.

---