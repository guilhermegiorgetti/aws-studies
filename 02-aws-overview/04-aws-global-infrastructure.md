# AWS Global Infrastructure 🌎

AWS operates a global infrastructure designed to provide cloud services with scalability, availability, and geographic reach.

The main concepts to understand are:

* Regions
* Availability Zones
* Data Centers
* Edge Locations

---

# The Hierarchy

A simplified mental model is:

```text
AWS Global Infrastructure
        │
        ├── Regions
        │     │
        │     └── Availability Zones
        │             │
        │             └── Data Centers
        │
        └── Edge Locations
```

These concepts are related, but they are not the same thing.

---

# AWS Region

An **AWS Region** is a separate geographic area where AWS has infrastructure.

Examples include:

* São Paulo
* Northern Virginia
* Frankfurt
* Tokyo
* Sydney

Each Region has its own collection of Availability Zones.

---

# Availability Zone

An **Availability Zone (AZ)** is one or more discrete data centers with redundant power, networking, and connectivity within an AWS Region.

Availability Zones are designed to be sufficiently isolated from one another while remaining connected with low-latency networking.

```text
AWS Region
│
├── Availability Zone A
│     └── Data Center(s)
│
├── Availability Zone B
│     └── Data Center(s)
│
└── Availability Zone C
      └── Data Center(s)
```

AWS Regions have multiple Availability Zones.

> The exact number of Availability Zones varies by Region.

---

# Example: Sydney

The Sydney Region is:

```text
ap-southeast-2
```

It has Availability Zones with identifiers such as:

```text
ap-southeast-2a
ap-southeast-2b
ap-southeast-2c
```

The exact physical location of an Availability Zone is not directly revealed by its letter identifier.

---

# Why Multiple Availability Zones?

Multiple AZs allow applications to be designed for higher availability and resilience.

For example:

```text
             Application
              /       \
             /         \
            ▼           ▼
          AZ A         AZ B
            │           │
            └─────┬─────┘
                  │
               Database
```

If one Availability Zone experiences a problem, a properly designed application can continue operating using resources in another AZ.

> Multiple AZs provide the building blocks for high availability and fault-tolerant architectures, but the application must be designed to use them correctly.

---

# Data Centers

A data center is a physical facility containing computing infrastructure.

Data centers contain resources such as:

* Servers
* Storage systems
* Networking equipment
* Power systems
* Cooling systems
* Physical security infrastructure

An Availability Zone consists of one or more data centers.

---

# Edge Locations

**Edge Locations** are locations used by AWS services such as Amazon CloudFront to deliver content closer to end users.

The main goal is to reduce latency by serving content from locations geographically closer to users.

Simplified example:

```text
User
 │
 ▼
Nearby Edge Location
 │
 ▼
AWS Regional Infrastructure
 │
 ▼
Application / Origin
```

---

# How to Choose an AWS Region

Two important factors from the course are:

1. Compliance
2. Proximity

---

# 1. Compliance

Organizations may have legal, regulatory, or contractual requirements regarding where data can be stored or processed.

For example, a company may need to keep certain data within a specific geographic area.

Therefore, Region selection can be influenced by:

* Data residency requirements
* Regulations
* Industry requirements
* Organizational policies

---

# 2. Proximity

The geographic distance between users and infrastructure can affect network latency.

A common strategy is:

> **Choose a Region geographically close to your users when appropriate.**

For example:

```text
Users in Brazil
      │
      ▼
São Paulo Region
      │
      ▼
Lower network distance
```

This does not mean that proximity is the only factor.

Region selection should consider:

* Compliance
* Latency
* Service availability
* Pricing
* Architecture requirements
* Disaster recovery strategy

---

# São Paulo Region

For my AWS studies, I will use the São Paulo Region:

```text
sa-east-1
```

This is useful for practicing with AWS while keeping the learning environment geographically close to Brazil.

---

# Region vs Availability Zone vs Data Center

| Concept               | Meaning                                                                       |
| --------------------- | ----------------------------------------------------------------------------- |
| **Region**            | Geographic AWS infrastructure area                                            |
| **Availability Zone** | Isolated infrastructure location within a Region                              |
| **Data Center**       | Physical facility containing infrastructure                                   |
| **Edge Location**     | Location used by services such as CloudFront to serve content closer to users |

---

# Mental Model

```text
                  AWS
                   │
        ┌──────────┴──────────┐
        │                     │
     Region A              Region B
        │                     │
   ┌────┼────┐           ┌────┼────┐
   │    │    │           │    │    │
  AZ-A AZ-B AZ-C        AZ-A AZ-B AZ-C
   │    │    │
   ▼    ▼    ▼
 Data  Data  Data
Center Center Center
```

Edge Locations are part of AWS's broader global network and are not simply another layer underneath a Region.

---

# Key Takeaways

* AWS has a global infrastructure.
* A **Region** is a geographic area containing AWS infrastructure.
* A Region contains multiple **Availability Zones**.
* An Availability Zone consists of one or more data centers.
* Availability Zones are designed for isolation and resilience.
* **Edge Locations** help deliver content closer to users.
* Region selection should consider compliance, latency, service availability, cost, and architecture.
* São Paulo is identified as `sa-east-1`.
* Sydney is identified as `ap-southeast-2`.

---

## Personal Notes

My simplified mental model:

> **Region = geographic area**

> **Availability Zone = isolated location inside the Region**

> **Data Center = physical facility**

> **Edge Location = location closer to users for content delivery**
