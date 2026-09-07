# Amazon EC2 💻

Amazon EC2 (**Elastic Compute Cloud**) provides resizable compute capacity in the AWS Cloud.

An EC2 instance is a virtual server.

It can be used to run:

* Web applications
* APIs
* Databases
* Development environments
* Backend services
* Enterprise applications

## EC2 Is a Regional Service

EC2 resources are associated with an AWS Region.

For example:

```text
Region: São Paulo
        │
        ├── Availability Zone A
        │       └── EC2 instance
        │
        ├── Availability Zone B
        │       └── EC2 instance
        │
        └── Availability Zone C
```

## Creating an EC2 Instance

When launching an EC2 instance, you typically choose/configure:

1. **AMI** — operating system and initial software configuration.
2. **Instance type** — CPU, memory, networking, and other characteristics.
3. **Storage** — disks/volumes attached to the instance.
4. **Network configuration** — VPC, subnet, IP configuration, etc.
5. **Security group** — controls allowed network traffic.
6. **Key pair / access method** — depending on the operating system and access configuration.

## Instance Type

An instance type defines the compute characteristics of the virtual machine.

For example:

```text
Instance Type
     │
     ├── vCPU
     ├── Memory
     ├── Network performance
     └── Other capabilities
```

## EC2 Mental Model

Think of EC2 as:

> **A virtual server whose computing capacity you can provision and scale in AWS.**

It is similar in concept to renting a server, but EC2 offers many different instance types, networking options, storage choices, and AWS integrations.

## Key Takeaways

* **EC2 = compute**
* **EC2 instance = virtual server**
* EC2 is associated with an AWS Region.
* EC2 instances run inside a VPC and subnet.
* Instance type determines the compute characteristics.
* Security groups control network traffic to and from the instance.
* AMIs provide the starting image for an instance.
