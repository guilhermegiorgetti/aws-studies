# IAM Users and Groups 👥

IAM Users and Groups provide a way to organize identities and manage permissions within an AWS account.

---

# IAM Users

An IAM User represents an identity within an AWS account.

For example:

```text
AWS Account
│
├── Alice
├── Bob
├── Charles
├── David
└── Edward
```

Users can receive permissions through IAM Policies.

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

Instead of attaching the same policy individually to Alice, Bob, and Charles, the policy can be attached to the Development group.

```text
Development Group
        │
        ▼
   IAM Policy
        │
        ▼
    Permissions
```

---

# Groups Cannot Contain Groups

IAM Groups can contain users, but not other groups.

Valid:

```text
Development
│
├── Alice
├── Bob
└── Charles
```

Invalid:

```text
Development
│
└── Operations ❌
```

---

# Users Without Groups

An IAM User does not have to belong to a group.

Example:

```text
Development
├── Alice
└── Bob

Standalone User
└── Charles
```

Charles can still have permissions through policies attached directly to the user.

However, groups are generally useful when several users need the same permissions.

---

# Users in Multiple Groups

A user can belong to multiple groups.

Example:

```text
Development
│
├── Alice
├── Bob
└── Charles

Operations
│
├── David
└── Edward

Audit Team
├── Charles
└── David
```

In this example:

* Charles belongs to Development and Audit Team.
* David belongs to Operations and Audit Team.

Therefore, a user can receive permissions from multiple groups.

---

# Permissions

Permissions can come from different sources.

Example:

```text
Charles
│
├── Development
│      └── Policy A
│
└── Audit Team
       └── Policy B
```

Charles's effective access is determined by the applicable policies.

---

# Why Use Groups?

Groups simplify permission management.

Without groups:

```text
Alice → Policy A
Bob → Policy A
Charles → Policy A
David → Policy A
```

With a group:

```text
Development Group
│
├── Alice
├── Bob
└── Charles
        │
        ▼
     Policy A
```

This is easier to manage and maintain.

---

# Best Practice

When multiple users require the same permissions, consider using an IAM Group rather than repeatedly attaching identical policies directly to individual users.

This can make access management:

* Easier
* More consistent
* Easier to audit
* Easier to change

## Key Takeaways

* Users represent identities.
* Groups organize users.
* Groups cannot contain other groups.
* Users do not have to belong to a group.
* A user can belong to multiple groups.
* Groups can have policies attached to them.
* Users can also have policies attached directly.
