# Shared Responsibility Model 🤝

The AWS Shared Responsibility Model defines how security responsibilities are divided between AWS and the customer.

```text
             SECURITY
                │
       ┌────────┴────────┐
       │                 │
      AWS             Customer
       │                 │
 Security OF         Security IN
 the cloud            the cloud
```

## AWS Responsibility

AWS is responsible for protecting the infrastructure that runs AWS services.

This includes areas such as:

* Physical data centers
* Physical hardware
* Physical networking
* Physical facilities
* AWS global infrastructure
* Underlying infrastructure used to provide AWS services

AWS manages the security **of the cloud**.

## Customer Responsibility

The customer is responsible for security **in the cloud**.

Depending on the services being used, this can include:

* Data
* Identity and access management
* Permissions
* Operating systems
* Applications
* Network configuration
* Security groups
* Encryption configuration
* Resource configuration

The exact division of responsibility depends on the AWS service being used.

## Example: Amazon EC2

With EC2, AWS manages the underlying physical infrastructure.

The customer is responsible for things such as:

* The guest operating system
* Installed applications
* Data
* Access permissions
* Security group configuration
* Operating system updates and patches

```text
AWS
│
├── Physical infrastructure
├── Data centers
├── Hardware
└── Underlying networking

Customer
│
├── Operating system
├── Applications
├── Data
├── Permissions
└── Security configuration
```

## Managed Services

The customer's responsibilities can change depending on the service.

For example, with a more managed service, AWS may operate more of the underlying infrastructure and platform.

Therefore:

> The more managed the service, the more security responsibilities AWS may handle on the customer's behalf.

However, customers remain responsible for their data, identities, access, and configuration according to the service.

## Exam Tip 🎯

Remember:

> **AWS = security OF the cloud**
> **Customer = security IN the cloud**
