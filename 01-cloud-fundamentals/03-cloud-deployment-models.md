# Cloud Deployment Models ☁️

Cloud deployment models describe how cloud infrastructure is owned, operated, and made available.

The three fundamental models are:

1. Private Cloud
2. Public Cloud
3. Hybrid Cloud

---

# 1. Private Cloud

A **private cloud** is a cloud environment dedicated to a single organization.

The organization has greater control over the environment and how resources are managed.

It can be hosted:

* On the organization's own infrastructure
* By a third party
* In a dedicated environment

### Example

An organization with strict requirements may use a dedicated private cloud environment.

### Characteristics

* Dedicated environment
* Greater control
* Customization
* Potentially higher infrastructure and management costs

---

# 2. Public Cloud

In a **public cloud**, infrastructure and services are provided by a cloud provider to multiple customers.

Customers consume resources without owning the underlying physical infrastructure.

Examples:

* AWS
* Microsoft Azure
* Google Cloud

### Basic idea

```text
Cloud Provider
      │
 ┌────┼────┐
 │    │    │
 ▼    ▼    ▼
Customer Customer Customer
```

The underlying infrastructure is shared, while customer environments and resources are logically isolated.

---

# 3. Hybrid Cloud

A **hybrid cloud** combines private infrastructure with public cloud resources.

For example:

```text
Private Environment
        │
        │
        ▼
   Connection
        │
        ▼
Public Cloud
```

An organization might keep some workloads or data in its own environment while using the public cloud for additional capacity or specific services.

---

# Why Use Hybrid Cloud?

Hybrid cloud can provide:

* Flexibility
* Scalability
* Integration with existing infrastructure
* Additional capacity
* Support for specific compliance requirements

---

# Comparison

| Model         | Infrastructure                | Main Characteristic   |
| ------------- | ----------------------------- | --------------------- |
| Private Cloud | Dedicated to one organization | More control          |
| Public Cloud  | Provided by a cloud provider  | Shared infrastructure |
| Hybrid Cloud  | Combination                   | Flexibility           |

---

# Key Takeaway

> **Private = dedicated environment.**

> **Public = cloud provider infrastructure shared among customers.**

> **Hybrid = combination of private and public environments.**
