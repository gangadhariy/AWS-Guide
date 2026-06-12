````md
# AWS EC2 Core Resources Interview Guide

---

# 1. EC2 Instance

## Definition

Amazon EC2 (Elastic Compute Cloud) is a virtual server provided by AWS that allows us to run applications in the cloud. It provides scalable compute capacity and allows users to launch Linux or Windows servers on demand.

## Components

- AMI
- Instance Type
- EBS Volume
- Security Group
- Key Pair
- IAM Role
- User Data

## Common Use Cases

- Application Hosting
- API Servers
- Kubernetes Worker Nodes
- Jenkins Servers
- Bastion Hosts

## Example

```text
Frontend Application
       ↓
EC2 Instance
       ↓
Backend Service
```

---

# 2. EC2 Instance Types

## Definition

Instance Types define the hardware configuration of an EC2 instance. They determine CPU, Memory, Storage and Networking capacity.

---

## General Purpose (M Series)

### Examples

- m5
- m6i
- m7i

### Provides

- Balanced CPU
- Balanced Memory
- Balanced Networking

### Use Cases

- APIs
- Web Applications
- Kubernetes Worker Nodes
- Microservices

### Interview Answer

M-series instances provide a balanced ratio of CPU, memory and networking resources, making them suitable for general application workloads.

---

## Compute Optimized (C Series)

### Examples

- c5
- c6i
- c7i

### Provides

- More CPU
- Less Memory

### Use Cases

- Jenkins Build Servers
- CI/CD Agents
- High Traffic APIs
- Data Processing

### Interview Answer

C-series instances are compute optimized and provide higher CPU performance. They are suitable for CPU-intensive workloads such as build servers and high-performance applications.

---

## Memory Optimized (R Series)

### Examples

- r5
- r6i
- r7i

### Provides

- Large Memory
- Moderate CPU

### Use Cases

- Redis
- Elasticsearch
- Caching Systems
- Large Java Applications

### Interview Answer

R-series instances are memory optimized and provide a higher memory-to-CPU ratio. They are used for memory-intensive workloads.

---

# 3. AMI (Amazon Machine Image)

## Definition

An AMI is a preconfigured machine image used to launch EC2 instances. It contains the operating system, installed software, configurations and storage mappings required to create identical servers.

## Contains

- Operating System
- Installed Packages
- Configurations
- Application Software
- EBS Snapshot References

## Why Use It?

Because it allows us to create multiple identical servers quickly and consistently.

## Example

```text
Ubuntu
Docker
Monitoring Agent
Security Tools
        ↓
Create AMI
        ↓
Launch Multiple EC2 Instances
```

---

# 4. Launch Template

## Definition

A Launch Template is a reusable EC2 launch configuration. It contains settings required to launch instances consistently.

## Contains

- AMI ID
- Instance Type
- Security Groups
- IAM Role
- EBS Configuration
- Key Pair
- User Data

## Why Use It?

Because we don't want to manually enter EC2 configuration every time.

## Common Use Cases

- Auto Scaling Groups
- EKS Node Groups
- Standardized Infrastructure

## Example

```text
Launch Template
      ↓
AMI
Instance Type
Security Group
IAM Role
User Data
      ↓
Launch EC2
```

---

# 5. Snapshot

## Definition

A Snapshot is a point-in-time backup of an EBS volume. AWS stores snapshots incrementally, meaning only changed blocks are stored after the first backup.

## Contains

- Database Files
- Logs
- Application Data
- User Files

## Why Use It?

- Backup
- Disaster Recovery
- Migration
- Volume Restoration

## Example

```text
EBS Volume
      ↓
Snapshot
      ↓
Create New Volume
      ↓
Attach to EC2
```

---

# 6. EBS (Elastic Block Store)

## Definition

Amazon EBS is persistent block storage used by EC2 instances. Data remains available even if the instance is stopped or restarted.

## Why Use It?

To store:

- Operating System
- Databases
- Application Data
- Logs

## EBS Types

### gp3

General Purpose SSD

#### Use Cases

- Web Applications
- APIs
- Most Production Workloads

### io2

Provisioned IOPS SSD

#### Use Cases

- High Performance Databases
- Critical Enterprise Applications

### st1

Throughput Optimized HDD

#### Use Cases

- Big Data
- Analytics
- Log Processing

### sc1

Cold HDD

#### Use Cases

- Archival Data
- Rarely Accessed Data

## Example

```text
EC2
 ↓
EBS Volume
 ↓
OS + Application + Logs
```

---

# 7. EFS (Elastic File System)

## Definition

Amazon EFS is a fully managed network file system that provides shared storage for multiple EC2 instances.

## Works With

- NFS Protocol

## Why Use It?

Because multiple EC2 instances can read and write the same files simultaneously.

## Common Use Cases

- Shared Application Storage
- Kubernetes Persistent Storage
- Shared Upload Directories

## Example

```text
EC2-1
   \
    \
     EFS
    /
   /
EC2-2
```

Both servers can access the same files.

---

# 8. FSx

## Definition

Amazon FSx is a fully managed high-performance file system service designed for specialized workloads.

## Types

### FSx for Windows File Server

Used for:

- Windows Applications
- Active Directory Integration

### FSx for Lustre

Used for:

- Machine Learning
- HPC Workloads
- Big Data Processing

### FSx for NetApp ONTAP

Used for:

- Enterprise Storage
- Hybrid Cloud

### FSx for OpenZFS

Used for:

- Linux Workloads
- High Performance Storage

## Interview Answer

FSx provides managed file systems optimized for specific workloads where EFS may not be sufficient.

---

# 9. Application Load Balancer (ALB)

## Definition

Application Load Balancer operates at Layer 7 and is used for HTTP/HTTPS traffic. It supports path-based and host-based routing making it ideal for microservices and web applications.

## Works At

Layer 7 (Application Layer)

## Understands

- HTTP
- HTTPS
- URLs
- Hostnames
- Headers

## Why Use It?

Because it can make routing decisions based on request content.

## Example

```text
myapp.com/api
      ↓
Backend Service

myapp.com/admin
      ↓
Admin Service
```

Host Based Routing:

```text
api.myapp.com
      ↓
API Servers

web.myapp.com
      ↓
Frontend Servers
```

---

# 10. Network Load Balancer (NLB)

## Definition

Network Load Balancer operates at Layer 4 and routes traffic based on TCP and UDP connections.

## Works At

Layer 4 (Transport Layer)

## Understands

- TCP
- UDP
- TLS

## Why Use It?

Because it provides extremely high performance and can handle millions of requests with very low latency.

## Common Use Cases

- Gaming Servers
- TCP Applications
- Database Proxies

---

# 11. Gateway Load Balancer (GWLB)

## Definition

Gateway Load Balancer is used to deploy, scale and manage third-party virtual appliances such as firewalls and intrusion detection systems.

## Why Use It?

To inspect traffic before it reaches applications.

## Common Use Cases

- Firewalls
- IDS
- IPS
- Security Appliances

## Example

```text
Internet
    ↓
GWLB
    ↓
Firewall Fleet
    ↓
Application
```

---

# 12. Target Group (TG)

## Definition

A Target Group is a collection of backend resources that receive traffic from a Load Balancer.

## Targets Can Be

- EC2 Instances
- IP Addresses
- Lambda Functions
- Containers

## Why Use It?

The Load Balancer forwards requests to healthy targets within a Target Group.

## Example

```text
ALB
 ↓
Target Group
 ↓
EC2-1
EC2-2
EC2-3
```

---

# 13. Auto Scaling Group (ASG)

## Definition

An Auto Scaling Group automatically adds or removes EC2 instances based on demand to maintain application availability.

## Why Use It?

- High Availability
- Fault Tolerance
- Cost Optimization

## Scaling Policies

### Target Tracking Policy

Example:

```text
Maintain CPU at 60%
```

AWS automatically scales based on target value.

---

### Step Scaling Policy

Example:

```text
CPU > 70%
Add 2 Instances

CPU > 90%
Add 5 Instances
```

---

### Simple Scaling Policy

Example:

```text
CPU > 80%
Add 1 Instance
Wait 5 Minutes
```

---

## Example

```text
Traffic Increases
       ↓
ASG Launches New EC2
       ↓
Traffic Decreases
       ↓
ASG Terminates EC2
```

---

# 14. Key Pair

## Definition

A Key Pair is a set of cryptographic keys used to securely connect to EC2 instances.

## Components

### Public Key

Stored on AWS EC2 instance.

### Private Key

Stored with the user.

## Why Use It?

For secure SSH authentication.

## Example

```text
Laptop
  ↓
Private Key
  ↓
SSH
  ↓
EC2 Instance
  ↓
Public Key Verification
```

## Interview Answer

A Key Pair consists of a public key and a private key. The public key is stored on the EC2 instance while the private key remains with the user. During SSH authentication AWS verifies that both keys match and allows secure access to the instance.
````
