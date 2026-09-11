# IAM Policies 📜

IAM Policies are JSON documents that define permissions in AWS.

Policies determine which actions can be allowed or denied and which resources those permissions apply to.

---

# Why Do We Need Policies?

Users and groups need permissions to interact with AWS resources.

Policies provide those permissions.

```text
User
 │
 ▼
Group
 │
 ▼
Policy
 │
 ▼
Permissions
 │
 ▼
AWS Resource
```

For example:

```text
Development Group
       │
       ▼
S3 Read Policy
       │
       ▼
Read S3 Objects
```

---

# JSON Policies

IAM Policies use JSON.

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}
```

This policy allows the specified S3 `GetObject` action on objects in the specified bucket.

---

# Policy Structure

The main elements commonly encountered in an IAM Policy are:

```text
Policy
│
├── Version
├── Id
└── Statement
     │
     ├── Sid
     ├── Effect
     ├── Principal
     ├── Action
     ├── Resource
     └── Condition
```

Some elements are optional or only applicable to particular policy types.

---

# Version

The `Version` element specifies the version of the policy language.

A commonly used value is:

```json
"Version": "2012-10-17"
```

This is the policy language version, not the date the policy was created.

---

# Id

The `Id` element is optional.

It can be used as an identifier for the policy.

---

# Statement

The `Statement` element contains one or more permission statements.

Example:

```json
"Statement": [
  {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "*"
  }
]
```

A policy can contain multiple statements.

---

# Sid

`Sid` means Statement ID.

It is an optional identifier that can help identify a particular statement.

Example:

```json
{
  "Sid": "AllowReadAccess",
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "*"
}
```

---

# Effect

`Effect` defines whether the statement allows or denies access.

Possible values include:

```text
Allow
Deny
```

Example:

```json
"Effect": "Allow"
```

or:

```json
"Effect": "Deny"
```

---

# Action

`Action` specifies the AWS API actions affected by the policy.

Example:

```json
"Action": "s3:GetObject"
```

Another example:

```json
"Action": [
  "s3:GetObject",
  "s3:ListBucket"
]
```

---

# Resource

`Resource` identifies the AWS resource to which the permission applies.

Example:

```json
"Resource": "arn:aws:s3:::example-bucket/*"
```

---

# Principal

`Principal` identifies the entity that is allowed or denied access when the policy type supports this element.

It is particularly important in **resource-based policies**.

Examples of principals can include:

* AWS accounts
* IAM users
* IAM roles
* Federated identities
* AWS services

### Important

Do not think of `Principal` as something that normally appears in every IAM policy.

Standard identity-based policies attached to Users, Groups, or Roles generally do not specify a `Principal`.

---

# Condition

`Condition` is an optional element used to add additional restrictions.

Conditions can use information such as:

* Source IP
* Time
* Tags
* Region
* Request attributes

Example concept:

```text
Allow S3 access
       │
       └── Only from a specific IP range
```

---

# Multiple Policies

A user can receive permissions from multiple sources.

Example:

```text
                    User
                      │
          ┌───────────┼───────────┐
          │           │           │
       Direct      Group A     Group B
       Policy        │           │
          │        Policy A    Policy B
          │
       Policy C
```

AWS evaluates the applicable policies to determine whether an action is allowed.

---

# Explicit Deny

One of the most important IAM concepts is:

> An explicit Deny overrides an applicable Allow.

Example:

```text
Policy A
Allow S3 Access
      +
Policy B
Deny S3 Access
      │
      ▼
Access Denied
```

---

# Wildcards

The wildcard character `*` can represent multiple matching values.

Example:

```text
s3:Get*
```

This matches actions whose names begin with `Get`.

It does not mean "everything after Get" in a literal textual sense.

Another example:

```text
"Action": "s3:*"
```

This is much broader because it can match all S3 actions.

Broad wildcards should be used carefully.

---

# Policy Creation

AWS provides a Policy Editor that can be used to create policies.

Two common approaches are:

## Visual Editor

The Visual Editor provides a graphical interface.

A simplified flow is:

```text
Service
   ↓
Actions
   ↓
Resources
   ↓
Conditions
```

This can be easier for beginners.

## JSON Editor

The JSON editor allows the policy to be written directly as JSON.

This provides more control and is useful for advanced configurations.

---

# Principle of Least Privilege

Policies should grant only the permissions required.

Bad:

```json
"Action": "*"
```

Better:

```json
"Action": [
  "s3:GetObject"
]
```

The goal is to avoid unnecessarily broad access.

## Key Takeaways

* IAM Policies are JSON documents.
* Policies define permissions.
* `Effect` can be Allow or Deny.
* `Action` specifies operations.
* `Resource` identifies resources.
* `Condition` adds restrictions.
* `Principal` is used where applicable, especially in resource-based policies.
* Explicit Deny overrides Allow.
* Wildcards must be used carefully.
* Least privilege should guide policy design.
