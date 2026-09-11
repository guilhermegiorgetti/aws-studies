# AWS Console Multi-session Support 🖥️

AWS Console Multi-session Support allows users to maintain multiple AWS Console sessions simultaneously.

This is useful when working with more than one AWS account.

---

# Why Use Multi-session Support?

Imagine that you work with two AWS accounts:

```text
Account A
    │
    └── Development

Account B
    │
    └── Production
```

Without multi-session support, switching between accounts can require repeatedly signing out and signing in.

With multiple sessions, different accounts can remain open in separate browser tabs.

Example:

```text
Browser
│
├── Tab 1
│   └── AWS Account A
│
├── Tab 2
│   └── AWS Account B
│
└── Tab 3
    └── Another AWS Session
```

---

# Enabling Multi-session Support

The AWS Console provides an option to enable this functionality:

```text
Turn on multi-session support
```

Once enabled, multiple AWS Console sessions can be maintained simultaneously.

---

# Multi-session Support and IAM

Multi-session support does not define permissions.

IAM determines:

```text
Who can access?
What can they do?
Which resources can they access?
```

Multi-session support is related to managing multiple AWS Console sessions.

Therefore:

```text
IAM
│
└── Identity and permissions

Multi-session Support
│
└── Multiple console sessions
```

---

# Practical Example

A cloud professional may need to compare resources in two AWS accounts.

```text
Tab 1
AWS Account: Development

Tab 2
AWS Account: Production
```

This makes it easier to work with both environments without repeatedly logging out.

---

# Important Security Consideration

Multi-session support improves convenience, but users must still be careful when working with multiple accounts.

Before performing a potentially destructive operation:

```text
Check Account
Check Region
Check Resource
Check Action
```

For example:

```text
Account: Production
Region: São Paulo
Resource: EC2 Instance
Action: Terminate
```

Always verify the environment before making important changes.

## Key Takeaways

* Multi-session support allows multiple AWS Console sessions.
* Different AWS accounts can be maintained in separate sessions.
* It is a Console feature, not an IAM permission mechanism.
* Always verify the active account and Region before making changes.
