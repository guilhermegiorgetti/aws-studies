# AWS Identity and Access Management (IAM) 🔑

AWS Identity and Access Management (IAM) is a global AWS service that helps securely control access to AWS resources.

IAM allows organizations to determine:

* Who can access AWS
* What they can do
* Which resources they can access
* Under which conditions access is allowed

## IAM in One Sentence

> IAM controls **who can do what on which AWS resources**.

---

# Authentication vs Authorization

Two fundamental concepts in IAM are authentication and authorization.

## Authentication

Authentication answers:

> **Who are you?**

Examples:

* Username and password
* Access keys
* MFA

```text
User
 │
 ▼
Authentication
 │
 ▼
"Who are you?"
```

## Authorization

Authorization answers:

> **What are you allowed to do?**

```text
Authenticated User
        │
        ▼
Authorization
        │
        ▼
"What can you do?"
```

For example:

```text
Alice
 │
 └── Can read objects from S3
```

while another user might have:

```text
Bob
 │
 └── Can manage EC2 instances
```

---

# IAM Is a Global Service

IAM is a global AWS service.

It is not associated with a specific AWS Region in the same way as Regional services such as EC2.

Therefore:

```text
IAM
│
└── AWS Account
     │
     ├── Region A
     ├── Region B
     └── Region C
```

IAM identities and permissions are managed at the account level.

This is why the Region selector does not behave the same way when working with IAM as it does with Regional services.

---

# Main IAM Components

The main IAM concepts are:

```text
IAM
│
├── Users
├── Groups
├── Policies
└── Roles
```

Each has a different purpose.

---

# IAM Users

An IAM User represents an identity within an AWS account.

A user can represent a person who needs access to AWS.

Example:

```text
AWS Account
│
├── Alice
├── Bob
├── Charles
├── David
└── Edward
```

Users can have permissions assigned through policies.

---

# IAM Groups

An IAM Group is a collection of IAM Users.

Groups are useful when multiple users need similar permissions.

Example:

```text
Development
│
├── Alice
├── Bob
└── Charles
```

The group can have policies attached to it.

The users then receive the permissions associated with those policies through their group membership.

---

# Important Group Rules

### Groups contain users

An IAM Group can contain users.

### Groups cannot contain other groups

```text
Group A
   │
   └── Group B ❌
```

This is not supported.

### Users do not have to belong to a group

A user can exist without belonging to any group.

However, groups are useful for organizing and managing common permissions.

### Users can belong to multiple groups

Example:

```text
Development
│
└── Charles

Audit Team
│
└── Charles
```

Charles belongs to both groups.

The applicable permissions from those groups are considered when AWS evaluates his access.

---

# IAM Policies

IAM Policies are JSON documents that define permissions.

A policy can specify:

* Actions
* Resources
* Conditions
* Allow or Deny effects

Simplified model:

```text
User / Group / Role
        │
        ▼
      Policy
        │
        ▼
   Permissions
```

---

# Principle of Least Privilege

The **Principle of Least Privilege** means giving an identity only the permissions required to perform its tasks.

Example:

```text
Application requirement:

Read files from S3
```

Bad approach:

```text
AdministratorAccess ❌
```

Better approach:

```text
Only required S3 read permissions ✅
```

This reduces the potential impact of:

* Compromised credentials
* Accidental actions
* Unauthorized activity
* Excessive permissions

---

# IAM Roles

IAM Roles are identities that can be assumed by trusted entities.

Roles are commonly used by:

* AWS services
* Applications
* EC2 instances
* Users
* Federated identities

For example:

```text
EC2 Instance
     │
     ▼
IAM Role
     │
     ▼
S3 Permissions
```

This allows an EC2 instance to access S3 without embedding long-term access keys directly in the instance.

---

# Root User

When an AWS account is created, a Root User is created as part of the account.

The Root User has unrestricted access to the AWS account.

Because of this, the Root User should not be used for everyday administrative tasks.

```text
AWS Account
│
├── Root User
│
├── IAM Users
├── IAM Groups
└── IAM Roles
```

---

# IAM Security Best Practices

Important IAM practices include:

* Avoid using the Root User for everyday tasks.
* Enable MFA for privileged identities.
* Follow the Principle of Least Privilege.
* Use groups to organize common permissions.
* Use roles when appropriate.
* Avoid unnecessary long-term credentials.
* Regularly review permissions.

## Key Takeaways

* IAM is a global service.
* IAM manages identity and access.
* Authentication determines who you are.
* Authorization determines what you can do.
* Users represent identities.
* Groups organize users.
* Policies define permissions.
* Roles provide assumable permissions.
* Least privilege reduces unnecessary access.
