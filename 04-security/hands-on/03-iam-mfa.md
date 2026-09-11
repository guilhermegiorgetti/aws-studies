# Hands-on: IAM Password Policy and MFA 🛠️🔐

This exercise demonstrates how password policies and Multi-Factor Authentication can be configured in AWS IAM.

---

# Part 1 — IAM Password Policy

Navigate to:

```text
IAM
 ↓
Account settings
 ↓
Edit password policy
```

The available options can be used to configure password requirements for IAM Users.

---

# Password Security Options

Depending on the AWS Console configuration, options may include:

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
├── Minimum Length
├── Uppercase
├── Lowercase
├── Number
├── Special Character
└── Expiration
```

---

# Part 2 — MFA

MFA can be configured for an AWS identity through its security credentials.

A general flow is:

```text
IAM
 ↓
Select Identity
 ↓
Security Credentials
 ↓
Assign MFA Device
```

The exact location and interface may vary as AWS updates the console.

---

# Assigning a Virtual MFA Device

For a virtual MFA device, select the authenticator application option.

The setup generally involves:

```text
Device Name
       ↓
Authenticator App
       ↓
QR Code / Setup Information
       ↓
Authenticator Application
       ↓
Verification Codes
```

---

# Authenticator Application

A compatible authenticator application can be installed on a supported smartphone.

The course demonstrates:

```text
Twilio Authy
```

as an example.

The important concept is not the specific application, but that the application acts as a **virtual MFA device** capable of generating authentication codes.

---

# MFA Setup Flow

```text
AWS Console
     │
     ▼
Security Credentials
     │
     ▼
Assign MFA Device
     │
     ▼
Authenticator App
     │
     ▼
Scan QR Code
     │
     ▼
Enter Verification Codes
     │
     ▼
MFA Enabled
```

---

# Why MFA Is Important

Without MFA:

```text
Password
   │
   ▼
Authentication
```

With MFA:

```text
Password
   +
MFA Device
   │
   ▼
Authentication
```

If an attacker obtains only the password, the additional factor can prevent unauthorized access.

---

# Important Warning ⚠️

MFA is strongly recommended for important AWS identities, especially the Root User.

However, the MFA device must be managed carefully.

For example:

```text
Smartphone
     │
     └── MFA
```

If the smartphone is lost, damaged, replaced, or reset, access to the MFA method may be affected.

This does **not** mean that losing a phone automatically means losing the AWS account permanently.

However, recovery can become more complicated.

Before enabling MFA on a critical identity:

* Understand AWS's current MFA recovery process.
* Protect the device.
* Keep recovery information secure.
* Understand how device replacement works.
* Avoid relying on an MFA method you cannot recover.

---

# Root User MFA

The Root User has unrestricted access to the AWS account.

Therefore, protecting it with MFA is one of the most important AWS account security practices.

```text
Root User
    │
    ├── Password
    │
    └── MFA
          │
          ▼
      Stronger Protection
```

---

# Password Policy vs MFA

These are complementary controls.

```text
Password Policy
      │
      └── Protects password quality

MFA
      │
      └── Adds another authentication factor
```

Using both provides stronger protection than relying on password complexity alone.

## Key Takeaways

* Password policies define password requirements.
* MFA adds another authentication factor.
* MFA is especially important for privileged identities.
* Protect the MFA device and recovery information.
* Understand recovery before configuring MFA on critical accounts.
