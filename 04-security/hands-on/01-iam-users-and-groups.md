# Hands-on: IAM Users and Groups 🛠️

This exercise demonstrates how to create IAM Users and Groups using the AWS Management Console.

---

# Important Security Note

The AWS Root User has unrestricted access to the AWS account.

Using the Root User for everyday activities is not considered a good security practice.

The goal is to create appropriate IAM identities for regular use.

---

# Step 1 — Open IAM

From the AWS Management Console:

```text
AWS Console
     │
     ▼
IAM
     │
     ▼
Users
```

IAM is a global service, so the Region selector does not determine where an IAM User is created.

---

# Step 2 — Create a User

Navigate to:

```text
IAM
 ↓
Users
 ↓
Create user
```

Enter the required information for the user.

The exact options presented by the AWS Console may change over time.

Depending on the authentication configuration, AWS can provide sign-in credentials for the user.

---

# Step 3 — Create a Group

Create a group according to the user's responsibilities.

Example:

```text
Admin
```

The group can then receive an IAM Policy.

Example:

```text
Admin Group
      │
      ▼
AdministratorAccess
```

---

# AdministratorAccess

`AdministratorAccess` is an AWS managed policy that provides broad administrative permissions.

It should only be assigned when that level of access is actually required.

For example:

```text
Developer
   │
   └── AdministratorAccess ❌
```

would generally be excessive if the developer only needs access to a specific service.

A better approach is to follow the Principle of Least Privilege.

---

# Step 4 — Add the User to the Group

After creating the group, add the appropriate user.

Example:

```text
Admin
│
└── User
```

The user can receive the permissions associated with the group's policies.

---

# Tags

Tags can be used to add metadata to supported AWS resources and identities.

Examples:

```text
Department = Development
Environment = Production
Project = AWS-Studies
Owner = Team-A
```

Tags can help with organization, management, automation, and identification.

---

# Account Alias

An AWS Account Alias is a human-friendly name associated with an AWS account sign-in URL.

It can make the sign-in URL easier to recognize than using only the AWS account ID.

Conceptually:

```text
Account ID
    ↓
Account Alias
    ↓
More recognizable sign-in URL
```

---

# Sign-in Information

The IAM user sign-in information can include information such as the AWS account sign-in URL.

This allows the IAM User to sign in separately from the Root User.

```text
Root User
     │
     └── AWS Account

IAM User
     │
     └── Same AWS Account
```

They are different identities accessing the same account.

---

# Final Structure

After completing the exercise:

```text
AWS Account
│
├── Root User
│
└── IAM
    │
    ├── Admin Group
    │      └── IAM User
    │
    └── Policies
```

## Key Takeaways

* Avoid using the Root User for everyday activities.
* IAM Users provide separate identities.
* Groups simplify permission management.
* Policies determine permissions.
* `AdministratorAccess` is highly privileged.
* Use least privilege whenever possible.
* Account aliases provide a friendlier account sign-in URL.
