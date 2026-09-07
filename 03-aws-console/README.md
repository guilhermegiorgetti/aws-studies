# AWS Console 🖥️

This folder contains notes about working with the AWS Management Console and understanding how AWS resources are organized by Region and Availability Zone.

## Topics

1. AWS Regions
2. Availability Zones
3. AWS Management Console
4. Amazon Route 53
5. Amazon EC2

## Core Ideas

* AWS resources are created in specific locations, depending on the service.
* Most AWS resources are associated with a Region.
* Availability Zones provide isolated infrastructure within a Region.
* The AWS Management Console provides a graphical interface for creating and managing AWS resources.
* Route 53 is a global DNS service.
* EC2 provides virtual compute capacity in AWS.

## Important Concepts

```text
AWS
 │
 ├── Regions
 │    │
 │    └── Availability Zones
 │
 ├── Global Services
 │
 └── Regional Services
```

## Key Takeaway

Understanding Regions, Availability Zones, and the AWS Management Console is essential before creating AWS resources.

---

# AWS Regions 🌎

An AWS Region is a separate geographic area where AWS operates infrastructure.

Examples:

| Region            | Region Code      |
| ----------------- | ---------------- |
| São Paulo         | `sa-east-1`      |
| Sydney            | `ap-southeast-2` |
| Northern Virginia | `us-east-1`      |

## Choosing a Region

When choosing an AWS Region, consider:

* **Latency** — proximity to users can reduce network latency.
* **Compliance** — some workloads have geographic or regulatory requirements.
* **Service availability** — not every AWS service or feature is available in every Region.
* **Pricing** — prices can vary between Regions.
* **Architecture** — the Region must support the architecture and services required by the workload.

## Example

If most users of an application are in Brazil, São Paulo (`sa-east-1`) may be a good choice because it is geographically closer to them.

## Mental Model

> **Region = geographic area containing AWS infrastructure.**

A Region contains multiple Availability Zones.

```text
Region
│
├── Availability Zone
├── Availability Zone
└── Availability Zone
```
