# AWS History 🚀

Amazon Web Services (AWS) grew from Amazon's own experience building and operating large-scale technology infrastructure.

The development of AWS was strongly connected to Amazon's need to build scalable, reusable infrastructure.

---

# 2002 — AWS Beginnings

AWS began as an internal effort at Amazon.

Amazon had developed significant technology infrastructure and recognized that some of these capabilities could potentially be offered as services to external customers.

---

# 2003 — Infrastructure as a Service Idea

Amazon identified an opportunity to provide technology infrastructure and services to other organizations.

The broader idea was:

> **Use the infrastructure Amazon had built and make it available to other developers and businesses.**

This became an important foundation for the development of AWS.

---

# 2004 — Amazon SQS

In 2004, Amazon launched **Amazon Simple Queue Service (SQS)**.

SQS is a managed message queuing service.

It allows applications to send, store, and receive messages between software components without requiring them to communicate directly at the same time.

### Simplified example

```text
Application A
     │
     ▼
   SQS Queue
     │
     ▼
Application B
```

This creates a buffer between application components.

---

# 2006 — S3 and EC2

In 2006, AWS publicly launched important foundational services including:

* **Amazon S3**
* **Amazon EC2**

---

## Amazon S3

**Amazon Simple Storage Service (S3)** is an object storage service.

It is commonly used to store:

* Files
* Images
* Videos
* Backups
* Logs
* Data

A simplified representation:

```text
Application
     │
     ▼
    S3
     │
 ┌───┼────┐
 ▼   ▼    ▼
Files Data Backups
```

S3 is not simply a traditional hard drive. It is an **object storage service**.

---

## Amazon EC2

**Amazon Elastic Compute Cloud (EC2)** provides resizable compute capacity in the AWS Cloud.

An EC2 instance can be thought of as a virtual server.

You can select characteristics such as:

* CPU
* Memory
* Storage
* Networking capabilities

```text
AWS
 │
 ▼
EC2 Instance
 │
 ├── CPU
 ├── Memory
 ├── Storage
 └── Network
```

EC2 gives customers control over many aspects of the virtual computing environment.

---

# 2007 — AWS in Europe

AWS expanded internationally, including launching infrastructure in Europe.

This expansion was important because customers could deploy workloads closer to users and meet regional requirements.

---

# AWS Growth

AWS has grown from a relatively small set of infrastructure services into a large cloud platform with services across many technology categories.

Today, AWS provides services for:

* Compute
* Storage
* Databases
* Networking
* Security
* Analytics
* AI and Machine Learning
* Developer tools
* Application integration
* Monitoring

The exact number of AWS services changes over time as AWS introduces new services and capabilities.

> Avoid treating statements such as "AWS has exactly 200 services" as a permanent fact. The AWS service catalog changes continuously.

---

# Important Early AWS Services

| Year | Service / Event                                   |
| ---- | ------------------------------------------------- |
| 2002 | Early internal AWS initiative                     |
| 2003 | Broader idea of offering infrastructure to others |
| 2004 | Amazon SQS                                        |
| 2006 | Amazon S3 and Amazon EC2                          |
| 2007 | Expansion into Europe                             |

---

# Why AWS History Matters

Understanding the history helps explain why AWS became a major cloud platform.

Amazon had to solve problems involving:

* Large-scale infrastructure
* Storage
* Computing
* Distributed systems
* Scalability
* Reliability

Many of these capabilities eventually became cloud services available to other organizations.

---

# Key Takeaways

* AWS originated from Amazon's internal infrastructure experience.
* Amazon SQS was an early AWS service.
* S3 introduced scalable object storage.
* EC2 provided cloud-based compute capacity.
* AWS expanded geographically over time.
* AWS now offers services across many technology categories.

---

## Personal Notes

My main mental model:

```text
Amazon's infrastructure experience
              ↓
Reusable technology
              ↓
Cloud services
              ↓
AWS
```
