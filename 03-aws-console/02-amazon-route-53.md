# Amazon Route 53 🌐

Amazon Route 53 is AWS's highly available and scalable **Domain Name System (DNS)** service.

DNS translates human-readable domain names into information that computers use to locate services, commonly IP addresses.

## Example

When a user enters:

```text
www.example.com
```

DNS can resolve the domain to an IP address such as:

```text
203.0.113.10
```

The browser can then connect to the destination.

## Route 53 and AWS Regions

Route 53 is a **global AWS service**.

Unlike Regional services such as EC2, Route 53 does not require you to select a Region in the same way when managing the service.

## Common Route 53 Functions

Route 53 can be used for:

* Domain registration
* DNS management
* DNS routing
* Health checks
* Routing users to applications and resources

## Mental Model

```text
User
 │
 │ www.example.com
 ▼
Route 53
 │
 │ DNS resolution
 ▼
Destination
```

---

