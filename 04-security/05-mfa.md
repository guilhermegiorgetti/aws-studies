

# Multi-Factor Authentication (MFA) 🔐

MFA adds an additional authentication factor beyond a password.

Instead of relying only on:

```text
Something you know
        ↓
     Password
```

MFA can require:

```text
Something you know
        +
Something you have
```

For example:

```text
Password
   +
Authenticator code
```

## Why MFA Matters

If someone obtains your password, MFA can provide an additional layer of protection.

```text
Attacker obtains password
          │
          ▼
       MFA required
          │
          ▼
   Access can be blocked
```

## AWS MFA

MFA can be enabled for important AWS identities, especially the Root User.

Common MFA methods can include authenticator applications and supported hardware security devices.

## Security Best Practice

> Enable MFA on the AWS Root User and other important identities.

## Key Takeaways

* MFA adds an additional authentication factor.
* MFA helps protect against compromised passwords.
* Root User should have MFA enabled.
* IAM users and other supported identities can also use MFA.