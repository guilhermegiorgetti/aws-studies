# AWS Services Overview 🧩

AWS provides a large collection of cloud services.

Each service is designed to solve a specific type of technology problem.

This section focuses on three foundational services introduced during my studies:

* Amazon SQS
* Amazon S3
* Amazon EC2

---

# Amazon SQS

**Amazon Simple Queue Service (SQS)** is a managed message queuing service.

It allows software components to communicate asynchronously.

---

## Why Use a Queue?

Imagine an application receiving many requests.

Instead of forcing one component to process everything immediately, messages can be placed into a queue.

```text
Producer
   │
   ▼
SQS Queue
   │
   ▼
Consumer
```

The producer sends a message.

SQS stores the message.

The consumer retrieves and processes the message.

---

## Example

An online store receives an order.

```text
Customer
   │
   ▼
Order System
   │
   ▼
SQS
   │
   ▼
Processing System
```

The queue helps separate the systems.

This can improve scalability and resilience.

---

# Amazon S3

**Amazon Simple Storage Service (S3)** is an object storage service.

S3 is commonly used for storing large amounts of data.

Examples:

* Images
* Videos
* Documents
* Backups
* Logs
* Application assets
* Data used for analytics

---

## S3 Mental Model

```text
Application
      │
      ▼
     S3
      │
 ┌────┼─────┐
 ▼    ▼     ▼
Image Files Backup
```

S3 stores data as **objects** inside **buckets**.

```text
Bucket
 │
 ├── image.jpg
 ├── document.pdf
 ├── backup.zip
 └── video.mp4
```

---

# Amazon EC2

**Amazon Elastic Compute Cloud (EC2)** provides virtual compute capacity.

An EC2 instance can be used as a virtual server.

You can choose different instance characteristics according to your workload.

Examples include:

* CPU
* Memory
* Storage
* Networking

---

## EC2 Mental Model

```text
AWS Infrastructure
        │
        ▼
   EC2 Instance
        │
 ┌──────┼──────┐
 ▼      ▼      ▼
 CPU   RAM   Storage
```

An EC2 instance can run an operating system and applications.

For example:

```text
EC2
 │
 ├── Linux
 │    └── Application
 │
 └── Networking
```

---

# SQS vs S3 vs EC2

| Service | Category                | Main Purpose             |
| ------- | ----------------------- | ------------------------ |
| **SQS** | Application Integration | Message queuing          |
| **S3**  | Storage                 | Object storage           |
| **EC2** | Compute                 | Virtual compute capacity |

---

# Easy Way to Remember

```text
SQS → Messages
S3  → Objects
EC2 → Compute
```

Or:

> **SQS moves messages.**

> **S3 stores objects.**

> **EC2 runs workloads.**

---

# Why These Services Matter

These three services represent different fundamental areas of cloud computing:

```text
Cloud
 │
 ├── Compute
 │     └── EC2
 │
 ├── Storage
 │     └── S3
 │
 └── Application Integration
       └── SQS
```

Understanding these categories makes it easier to learn other AWS services later.
