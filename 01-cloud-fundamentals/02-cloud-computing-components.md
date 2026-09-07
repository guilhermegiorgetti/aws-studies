# `02-cloud-computing-components.md`

# Cloud Computing Components 🖥️

A server is a computer system that provides resources or services to other computers and applications.

A simplified model of a server can be represented as:

```text
Compute
   +
Memory
   +
Storage
   +
Database
   +
Network
   =
Server
```

---

# 1. Compute

**Compute** represents the processing capability of a computer.

The CPU executes instructions and performs calculations.

Examples:

* Calculations
* Application processing
* Running programs
* Processing requests

### Analogy

The CPU can be compared to the **brain's processing capability**.

---

# 2. Memory

**Memory**, commonly represented by RAM, provides temporary working space for data and programs that are actively being used.

RAM is:

* Fast
* Temporary
* Volatile

When a computer is powered off, the contents of RAM are normally lost.

### Analogy

RAM can be compared to a person's **working memory**.

---

# 3. Storage

**Storage** is used to persist data.

Examples:

* Files
* Documents
* Images
* Videos
* Application data

Unlike RAM, persistent storage is designed to retain data beyond a power cycle.

Examples of storage technologies include:

* SSD
* HDD
* Object storage
* Block storage
* File storage

---

# 4. Database

A database is a system designed to store and retrieve data in an organized way.

Databases can use different data models.

Examples include:

* Relational databases
* Document databases
* Key-value databases
* Graph databases

A relational database, for example, organizes data into tables.

---

# 5. Network

The network allows computers and services to communicate.

Important networking components include:

* Routers
* Switches
* DNS
* Network interfaces
* Cables
* Wireless connections

---

# Router

A **router** connects different networks and determines where network packets should be forwarded.

Simplified example:

```text
Network A
   │
   ▼
Router
   │
   ▼
Network B
```

---

# Switch

A **network switch** connects devices within a network, commonly within a local network.

Simplified example:

```text
        Switch
       /   |   \
      /    |    \
 Client Server Client
```

The switch forwards traffic to the appropriate destination on the local network.

---

# DNS

**DNS (Domain Name System)** translates domain names into IP addresses and can also perform other types of DNS resolution.

For example:

```text
example.com
     │
     ▼
DNS
     │
     ▼
IP Address
```

The important idea is:

> Humans prefer names. Networks communicate using IP addresses.

---

# Server Summary

A simplified server model is:

```text
┌──────────────────────────┐
│          SERVER          │
├──────────────────────────┤
│ Compute                  │
│ Memory                   │
│ Storage                  │
│ Database                 │
│ Network                  │
└──────────────────────────┘
```

These components work together to allow applications and services to operate.

---

# Key Takeaways

* **CPU → processing**
* **RAM → temporary working memory**
* **Storage → persistent data**
* **Database → organized data management**
* **Network → communication**
* **Router → connects networks**
* **Switch → connects devices within a network**
* **DNS → translates domain names into IP addresses**

---

