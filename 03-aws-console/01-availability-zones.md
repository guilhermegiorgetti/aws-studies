# Availability Zones ⚡

An Availability Zone (AZ) is an isolated location within an AWS Region.

An Availability Zone consists of one or more discrete data centers with redundant power, networking, and connectivity.

## Example

São Paulo:

```text
Region: sa-east-1

├── sa-east-1a
├── sa-east-1b
└── sa-east-1c
```

The exact number and naming of Availability Zones varies by Region.

## Why Availability Zones Matter

Availability Zones are designed to be sufficiently isolated from each other so that failures in one location are less likely to affect another.

This allows applications to be designed for **high availability**.

For example:

```text
              Application
                   │
        ┌──────────┴──────────┐
        │                     │
       AZ-A                  AZ-B
        │                     │
     Server                Server
```

If one Availability Zone becomes unavailable, the application may continue operating from another AZ when it has been properly designed for redundancy.

## Mental Model

* **Region** → geographic area
* **Availability Zone** → isolated location inside a Region
* **Data Center** → physical facility inside an Availability Zone

---

# AWS Management Console 🖥️

The AWS Management Console is a web-based graphical interface used to interact with AWS services.

From the console, you can:

* Create resources
* Configure resources
* Monitor resources
* View billing information
* Manage security settings
* Search for AWS services
* Select AWS Regions

## Console Home

The AWS Console provides access to AWS services through:

* Service search
* **All Services**
* Recently visited services
* Region selector
* Account and security options

## Region Selector

Many AWS services are **Regional**.

Therefore, the selected Region matters when creating resources.

Example:

```text
AWS Console
     │
     └── Region: São Paulo
              │
              └── Create EC2 instance
```

The EC2 instance will be created in the selected Region.

## Important

Not every AWS service is Regional.

Some services are **Global**, meaning they are not tied to a single AWS Region in the same way.

Examples include:

* Amazon Route 53
* IAM

## Key Takeaway

> Always check the selected Region before creating a Regional AWS resource.
