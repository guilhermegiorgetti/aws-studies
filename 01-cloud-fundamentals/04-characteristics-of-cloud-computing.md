# Characteristics of Cloud Computing ☁️

Cloud computing has several characteristics that distinguish it from traditional IT infrastructure.

The main characteristics studied in this module are:

1. On-Demand Self-Service
2. Broad Network Access
3. Resource Pooling
4. Rapid Elasticity
5. Measured Service

---

# 1. On-Demand Self-Service

Users can provision computing resources when they need them without requiring direct human interaction with the cloud provider.

For example, an AWS customer can create an EC2 instance through the AWS Management Console or API.

```text
User
  │
  ▼
AWS Console / API
  │
  ▼
Cloud Resource
```

The user does not need to call an AWS employee every time a resource needs to be created.

---

# 2. Broad Network Access

Cloud resources are accessible through networks using standard mechanisms.

This allows users and applications to access cloud services from different types of devices and locations.

Examples:

* Computers
* Servers
* Smartphones
* Tablets
* Applications

---

# 3. Resource Pooling

Cloud providers pool computing resources to serve multiple customers.

The physical infrastructure is shared, while customer resources are logically isolated.

```text
            Cloud Provider
                  │
          Resource Pool
        ┌─────────┼─────────┐
        │         │         │
        ▼         ▼         ▼
   Customer A Customer B Customer C
```

### Analogy

Imagine an apartment building.

The building's infrastructure is shared:

* Electricity infrastructure
* Plumbing
* Elevators
* Physical structure

But each tenant has a separate apartment.

The same general idea helps explain resource pooling and isolation in cloud environments.

---

# 4. Rapid Elasticity

Cloud resources can be provisioned and released rapidly according to demand.

For example:

```text
Low Demand
   │
   ▼
2 Instances
```

When demand increases:

```text
High Demand
   │
   ▼
10 Instances
```

When demand decreases:

```text
Low Demand
   │
   ▼
2 Instances
```

This ability to adapt resources to demand is one of the major advantages of cloud computing.

---

# Scalability vs. Elasticity

These concepts are related but not identical.

### Scalability

Scalability is the ability of a system to handle increased workload by adding resources.

Examples:

* More CPU
* More RAM
* More servers

### Elasticity

Elasticity emphasizes automatically or rapidly increasing and decreasing resources according to demand.

```text
Demand
  ▲
  │        /\ 
  │       /  \
  │  /\  /    \__
  │ /  \/
  └─────────────────► Time

Resources can adapt to the demand.
```

---

# 5. Measured Service

Cloud providers measure resource usage.

Customers can then be charged according to the resources they consume, depending on the service and pricing model.

Examples of measured resources can include:

* Compute time
* Storage consumed
* Data transfer
* Number of requests

This supports the **pay-as-you-go** model.

---

# Summary

| Characteristic         | Meaning                                       |
| ---------------------- | --------------------------------------------- |
| On-Demand Self-Service | Provision resources when needed               |
| Broad Network Access   | Access services through networks              |
| Resource Pooling       | Shared provider infrastructure with isolation |
| Rapid Elasticity       | Quickly scale resources up or down            |
| Measured Service       | Usage is measured for monitoring/billing      |

---

# Key Takeaways

Cloud computing is designed to provide:

* On-demand resources
* Network-based access
* Shared infrastructure
* Rapid scalability and elasticity
* Usage measurement

These characteristics help explain why cloud computing can be more flexible than traditional infrastructure.
