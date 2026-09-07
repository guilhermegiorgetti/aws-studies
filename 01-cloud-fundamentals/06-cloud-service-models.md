# Cloud Service Models ☁️

Cloud computing can be divided into different service models based on how much of the technology stack is managed by the cloud provider versus the customer.

The main models are:

1. On-Premises
2. Infrastructure as a Service (IaaS)
3. Platform as a Service (PaaS)
4. Software as a Service (SaaS)

---

# The IT Stack

To understand the differences between the models, it helps to visualize the technology stack.

```text
Application
     ↓
Data
     ↓
Runtime
     ↓
Middleware
     ↓
Operating System
     ↓
Virtualization
     ↓
Servers
     ↓
Storage
     ↓
Networking
```

Different service models move responsibility for these layers between the customer and the provider.

---

# 1. On-Premises

With an on-premises environment, the organization manages essentially the entire technology stack.

```text
Customer manages everything

Application
Data
Runtime
Middleware
Operating System
Virtualization
Servers
Storage
Networking
```

### Examples

* Application: custom web application
* Data: company data
* Runtime: Python
* Middleware: application server/API layer
* Operating System: Linux
* Virtualization: VMware, Hyper-V
* Servers: physical servers
* Storage: SAN/NAS
* Networking: switches, routers, firewalls

---

# 2. Infrastructure as a Service — IaaS

**IaaS** provides the fundamental building blocks of cloud infrastructure.

The cloud provider manages the physical infrastructure.

The customer generally manages:

* Applications
* Data
* Runtime
* Middleware
* Operating System

Depending on the service, the exact division of responsibility can vary.

---

# IaaS Concept

```text
Customer
──────────────
Application
Data
Runtime
Middleware
Operating System
──────────────
Cloud Provider
Virtualization
Servers
Storage
Networking
Physical infrastructure
```

IaaS provides a high degree of control and flexibility.

### Examples

* Amazon EC2
* Virtual machines
* Virtual networks
* Cloud storage infrastructure

---

# 3. Platform as a Service — PaaS

**PaaS** provides a managed platform for developing and deploying applications.

The provider manages more of the underlying infrastructure and platform.

The customer can focus primarily on:

* Application
* Data

### Concept

```text
Customer
──────────────
Application
Data
──────────────
Cloud Provider
Runtime
Middleware
Operating System
Virtualization
Servers
Storage
Networking
```

The exact responsibility boundaries depend on the specific service.

---

# Example: Heroku

Heroku is an example of a platform-oriented service.

The developer can focus on deploying application code without managing the underlying servers, operating system, and much of the platform infrastructure.

---

# 4. Software as a Service — SaaS

**SaaS** provides a complete software application that customers can use.

The provider manages the application and the underlying infrastructure.

Examples include:

* Email applications
* Collaboration software
* Customer relationship management systems
* Online productivity applications

---

# SaaS Concept

```text
Cloud Provider
────────────────
Application
Data infrastructure
Runtime
Middleware
Operating System
Virtualization
Servers
Storage
Networking
────────────────

Customer
Uses and configures the application
```

The customer is not responsible for managing the underlying servers or operating system.

However, SaaS does **not** mean the customer has zero responsibilities.

Depending on the service, the customer may still manage:

* User accounts
* Access permissions
* Application configuration
* Data entered into the service
* Security settings

---

# Responsibility Comparison

A simplified model:

| Layer            | On-Premises | IaaS     | PaaS     | SaaS                  |
| ---------------- | ----------- | -------- | -------- | --------------------- |
| Application      | Customer    | Customer | Customer | Provider              |
| Data             | Customer    | Customer | Customer | Shared responsibility |
| Runtime          | Customer    | Customer | Provider | Provider              |
| Middleware       | Customer    | Customer | Provider | Provider              |
| Operating System | Customer    | Customer | Provider | Provider              |
| Virtualization   | Customer    | Provider | Provider | Provider              |
| Servers          | Customer    | Provider | Provider | Provider              |
| Storage          | Customer    | Provider | Provider | Provider              |
| Networking       | Customer    | Provider | Provider | Provider              |

> This is a simplified learning model. The exact responsibilities depend on the specific cloud service.

---

# IaaS vs PaaS vs SaaS

A useful way to remember the models:

```text
IaaS
↓
Rent infrastructure

PaaS
↓
Rent a managed platform

SaaS
↓
Use finished software
```

Another analogy:

### IaaS

> "Give me the infrastructure. I'll manage the operating system and application."

### PaaS

> "Give me an environment where I can deploy my application."

### SaaS

> "Give me the finished application. I just want to use it."

---

# Control vs. Convenience

Generally:

```text
More Control
     ▲
     │
   IaaS
     │
   PaaS
     │
   SaaS
     │
     ▼
More Managed
```

As you move from IaaS to PaaS to SaaS:

* The provider manages more.
* The customer manages less infrastructure.
* The customer generally has less control over the underlying stack.
* The customer can focus more on using or building the application.

---

# Key Takeaways

### On-Premises

> Customer manages essentially everything.

### IaaS

> Customer manages the operating system and above; provider manages the physical infrastructure and virtualization.

### PaaS

> Customer focuses mainly on the application and data; provider manages the underlying platform.

### SaaS

> Customer uses a complete application; provider manages the application and infrastructure.

---

# Memory Trick

```text
IaaS → Infrastructure
PaaS → Platform
SaaS → Software
```

Or:

```text
IaaS = Build it
PaaS = Deploy it
SaaS = Use it
```
