# IAM Users, Groups, and Policies 👥

## IAM Users

An IAM user is an identity created within an AWS account.

A user may have permissions that allow them to interact with AWS services.

For example:

```text
Developer
   │
   └── IAM User
          │
          └── Permissions
```

## IAM Groups

An IAM group is a collection of IAM users.

Groups make it easier to assign common permissions to multiple users.

Example:

```text
Developers Group
│
├── Alice
├── Bob
└── Carol
```

A policy attached to the group can provide the appropriate permissions to its members.

## IAM Policies

IAM policies define what actions are allowed or denied.

A policy can contain concepts such as:

```text
Effect
Action
Resource
Condition
```

### Example

```text
Effect: Allow
Action: s3:GetObject
Resource: arn:aws:s3:::my-bucket/*
```

This can allow reading objects from a specific S3 bucket.

## Allow and Deny

IAM policies can explicitly allow or deny actions.

An explicit **Deny** takes precedence over an Allow.

```text
Allow + No Deny
      ↓
    Allowed

Allow + Explicit Deny
      ↓
    Denied
```

## Users vs Groups vs Policies

| Concept | Purpose                                  |
| ------- | ---------------------------------------- |
| User    | Represents an identity                   |
| Group   | Organizes users                          |
| Policy  | Defines permissions                      |
| Role    | Provides permissions that can be assumed |

---
