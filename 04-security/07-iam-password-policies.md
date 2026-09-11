# IAM Password Policy 🔐

An IAM Password Policy defines requirements for passwords used by IAM Users.

Password policies can help organizations establish stronger password requirements.

---

# Password Requirements

Depending on the configuration, a password policy can define requirements such as:

* Minimum password length
* Uppercase characters
* Lowercase characters
* Numbers
* Non-alphanumeric characters
* Password expiration
* Password reuse prevention
* Password reset requirements

Example:

```text
Password Policy
│
├── Minimum length
├── Uppercase
├── Lowercase
├── Numbers
├── Special characters
└── Expiration rules
```

---

# Strong Password

A stronger password policy can make passwords more resistant to guessing and brute-force attacks.

For example:

```text
Minimum Length
       +
Uppercase
       +
Lowercase
       +
Number
       +
Special Character
```

However, password complexity alone is not enough to provide strong account security.

---

# Password Expiration

An organization can configure password expiration requirements.

For example:

```text
Password created
       │
       ▼
Password remains valid
       │
       ▼
Expiration period reached
       │
       ▼
Password change required
```

Password expiration should be considered together with other authentication and security controls.

---

# Password Policy vs MFA

Password policies and MFA are different security controls.

## Password Policy

Protects the password itself.

```text
Password
   │
   └── Stronger requirements
```

## MFA

Adds another authentication factor.

```text
Password
   +
MFA
   │
   ▼
Authentication
```

Using both can provide stronger protection.

---

# Password Security

Good IAM security does not depend only on password complexity.

Important controls include:

* Strong passwords
* MFA
* Least privilege
* Secure credential management
* Regular access reviews
* Avoiding unnecessary privileged access

## Key Takeaway

> A strong password is important, but MFA adds another layer of protection because authentication no longer depends only on the password.
