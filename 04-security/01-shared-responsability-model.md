# AWS Shared Responsibility Model 🤝

The AWS Shared Responsibility Model defines how security responsibilities are divided between AWS and the customer.

The fundamental idea is:

> AWS is responsible for **security OF the cloud**, while the customer is responsible for **security IN the cloud**.

## The Two Sides

```text
                    SECURITY
                       │
          ┌────────────┴────────────┐
          │                         │
         AWS                    CUSTOMER
          │                         │
 Security OF                  Security IN
 the cloud                     the cloud
```

---

# AWS Responsibility

AWS is responsible for the security **of the cloud**.

This means protecting the underlying infrastructure that provides AWS services.

Examples include:

* Physical data centers
* Physical facilities
* Physical hardware
* Physical networking
* Physical infrastructure
* Infrastructure virtualization
* Environmental controls
* Physical security

## Example

Imagine an AWS data center.

AWS is responsible for protecting:

```text
AWS Data Center
│
├── Physical building
├── Servers
├── Networking hardware
├── Power infrastructure
├── Cooling
└── Physical security
```

The customer does not have to physically protect the AWS data center.

---

# Customer Responsibility

The customer is responsible for security **in the cloud**.

Depending on the AWS service being used, this may include:

* Applications
* Customer data
* Sensitive data
* IAM configuration
* Permissions
* Operating systems
* Network configuration
* Security groups
* Encryption configuration
* Resource configuration
* Access management

The exact responsibilities depend on the AWS service.

---

# Example: Amazon EC2

Amazon EC2 provides virtual servers.

AWS manages the underlying infrastructure, while the customer has additional responsibilities for the virtual machine and workload.

```text
AWS
│
├── Physical data center
├── Physical hardware
├── Physical networking
└── Underlying infrastructure

CUSTOMER
│
├── Operating system
├── Applications
├── Data
├── IAM permissions
├── Security configuration
└── Network access configuration
```

For example, if the customer installs software on an EC2 instance, maintaining and securing that software is generally the customer's responsibility.

---

# Example: Firewall and Network Access

AWS provides network security capabilities, but the customer is responsible for configuring the appropriate controls for their workload.

For example, with EC2, customers configure security groups to control network traffic.

```text
Internet
   │
   ▼
Security Group
   │
   ├── HTTP → Allowed
   ├── HTTPS → Allowed
   └── Unnecessary Port → Blocked
```

The customer must decide which traffic should be allowed.

---

# Responsibilities Depend on the Service

The division of responsibilities is not identical for every AWS service.

For services where AWS manages more of the infrastructure and platform, AWS takes on more operational responsibility.

For services where the customer has more control, the customer has more security responsibilities.

```text
More customer control
        │
        ▼
More customer responsibility
```

```text
More AWS-managed infrastructure
        │
        ▼
More AWS responsibility
```

This is why understanding the specific AWS service being used is important.

---

# Exam Tip 🎯

For the AWS Cloud Practitioner exam, remember:

```text
AWS
│
└── Security OF the cloud

Customer
│
└── Security IN the cloud
```

## Key Takeaway

> AWS secures the infrastructure that runs the cloud, while customers are responsible for securely configuring and using the resources they operate in the cloud.
