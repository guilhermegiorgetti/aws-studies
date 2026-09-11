# Hands-on: IAM Policies 🛠️

This exercise demonstrates how to inspect, attach, and create IAM Policies through the AWS Management Console.

---

# Step 1 — Open IAM

Navigate to:

```text
AWS Console
     │
     ▼
IAM
```

Then select:

```text
Users
```

---

# Step 2 — Select a User

Choose the IAM User whose permissions you want to manage.

Then select:

```text
Add permissions
```

---

# Step 3 — Attach Policies Directly

One option is:

```text
Attach policies directly
```

This attaches an identity-based policy directly to the selected user.

Example:

```text
IAM User
   │
   ▼
IAMReadOnlyAccess
```

---

# IAMReadOnlyAccess

`IAMReadOnlyAccess` is an AWS managed policy that provides read-only permissions for IAM resources covered by that policy.

It does not provide unrestricted administrative access.

The exact permissions can be inspected in the policy itself.

---

# Understanding "Attached Via"

When inspecting a user's permissions, the AWS Console can show where a policy is coming from.

Example:

| Policy   | Attached Via      |
| -------- | ----------------- |
| Policy A | Group: Admin      |
| Policy B | Group: Developers |
| Policy C | Directly          |

This is useful for understanding the source of a user's permissions.

---

# Example

Suppose:

```text
User: Alice
```

Alice belongs to:

```text
Admin
Developers
```

and also has one directly attached policy.

Her permissions may therefore come from:

```text
Alice
│
├── Direct Policy
│
├── Admin
│     └── Policy A
│
└── Developers
      └── Policy B
```

AWS evaluates the applicable policies when determining whether Alice can perform an action.

---

# Step 4 — Open Policies

From IAM, select:

```text
Policies
```

Here you can search for policies such as:

```text
AdministratorAccess
IAMReadOnlyAccess
```

---

# Policy Summary

A policy can be inspected through the console's summary and permission information.

The summary helps understand:

* Services
* Actions
* Resources
* Access level

---

# Policy JSON

Policies can also be viewed directly as JSON.

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

---

# Understanding `Get*`

A policy may contain an action such as:

```text
s3:Get*
```

The wildcard means that actions matching the `Get*` pattern can be included.

For example, depending on the service's available actions:

```text
s3:GetObject
s3:GetObjectAttributes
```

may match the pattern.

It does **not** mean "everything that comes after the word Get."

---

# Creating a Policy

AWS provides a Policy Editor.

There are two main ways to work with policies:

## Visual Editor

The Visual Editor provides a graphical interface.

The general process is:

```text
Service
   ↓
Actions
   ↓
Resources
   ↓
Conditions
```

Example:

```text
Service
   │
   └── S3

Actions
   │
   └── GetObject

Resources
   │
   └── Specific bucket
```

---

# JSON Editor

The JSON editor allows the policy to be written directly.

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3Read",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

---

# Visual Editor vs JSON

| Visual Editor                       | JSON                               |
| ----------------------------------- | ---------------------------------- |
| Graphical interface                 | Code-like configuration            |
| Easier for beginners                | More flexible                      |
| Guided configuration                | Direct control                     |
| Less error-prone for basic policies | Better for complex/custom policies |

Both represent the same underlying policy model.

---

# Least Privilege

When creating a policy, avoid granting more access than necessary.

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

Even better, when possible, restrict the resource too:

```json
"Resource": "arn:aws:s3:::example-bucket/*"
```

## Key Takeaways

* Policies define permissions.
* Policies can be attached directly to users.
* Policies can also be associated through groups.
* The AWS Console shows where permissions are attached.
* Policies can be inspected through summaries and JSON.
* The Visual Editor is graphical.
* The JSON Editor provides direct policy control.
* Always consider least privilege.
