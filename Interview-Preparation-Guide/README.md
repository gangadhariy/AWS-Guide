# AWS EC2 Resources – Interview Preparation Guide

This guide contains interview-friendly definitions of AWS EC2 resources with explanations and subpoints.

---

# 1. Amazon EC2 Instance

Amazon Elastic Compute Cloud (EC2) is a web service that provides secure and resizable virtual servers in the cloud. It allows users to launch computing resources on demand without maintaining physical hardware.

### Key Points
- Provides scalable compute capacity
- Supports Linux and Windows operating systems
- Pay only for the resources you use
- Can be launched in multiple instance types

### Use Cases
- Hosting applications
- Running web servers
- CI/CD pipelines
- Testing environments

---

# 2. Amazon Machine Image (AMI)

An Amazon Machine Image (AMI) is a pre-configured template used to launch EC2 instances. It contains the operating system, software packages, application server, and configurations required to create an instance.

### Key Points
- Speeds up instance deployment
- Can be customized
- Can be shared across AWS accounts

### Types
- AWS Managed AMI
- Custom AMI
- Marketplace AMI

---

# 3. Launch Template (LT)

A Launch Template is a configuration template that stores EC2 launch parameters such as AMI ID, instance type, security groups, storage, and user data. It simplifies launching identical EC2 instances.

### Key Points
- Reusable launch configuration
- Used by Auto Scaling Groups
- Supports versioning

### Contains
- AMI
- Instance type
- Security group
- Key pair
- IAM role
- User data script

---

# 4. Security Group

A Security Group is a virtual firewall that controls inbound and outbound traffic for EC2 instances. It helps secure resources by allowing only authorized traffic.

### Key Points
- Stateful firewall
- Supports allow rules only
- Attached at instance level

### Example
Allow port 22 for SSH access.

---

# 5. Network ACL (NACL)

A Network Access Control List is an optional layer of security that controls traffic entering and leaving subnets.

### Key Points
- Stateless firewall
- Supports allow and deny rules
- Works at subnet level

### Use Case
Subnet-wide security filtering.

---

# 6. Elastic IP (EIP)

An Elastic IP is a static public IPv4 address designed for dynamic cloud computing. It allows instances to retain the same public IP even after restart or reassociation.

### Key Points
- Static public address
- Reassignable
- Supports failover

### Use Cases
- Fixed DNS mapping
- Disaster recovery

---

# 7. Auto Scaling Group (ASG)

An Auto Scaling Group automatically launches or terminates EC2 instances based on workload demand to maintain application availability.

### Key Points
- Automatic scaling
- Health checks
- High availability

### Scaling Types
- Dynamic scaling
- Scheduled scaling
- Predictive scaling

---

# 8. Target Group

A Target Group is used by load balancers to route requests to registered backend resources like EC2 instances.

### Key Points
- Performs health checks
- Routes traffic to healthy targets
- Works with ALB and NLB

### Supported Targets
- EC2 instances
- IP addresses
- Lambda functions

---

# 9. Application Load Balancer (ALB)

An Application Load Balancer distributes HTTP and HTTPS traffic across multiple targets. It operates at Layer 7 of the OSI model.

### Key Points
- Path-based routing
- Host-based routing
- SSL termination

### Use Cases
- Web applications
- Microservices routing

---

# 10. Network Load Balancer (NLB)

A Network Load Balancer distributes TCP, UDP, and TLS traffic at Layer 4 with ultra-low latency.

### Key Points
- High performance
- Static IP support
- Millions of requests per second

### Use Cases
- Gaming applications
- Real-time systems

---

# 11. Gateway Load Balancer (GLB)

A Gateway Load Balancer distributes traffic to third-party virtual appliances such as firewalls and intrusion detection systems.

### Key Points
- Layer 3 load balancing
- Transparent traffic routing

### Use Cases
- Security inspection
- Packet filtering

---

# 12. Amazon EBS

Amazon Elastic Block Store provides block-level persistent storage volumes for EC2 instances.

### Key Points
- Persistent storage
- High performance
- Detachable volumes

### Use Cases
- Databases
- Operating system disks

---

# 13. EBS Snapshot

An EBS Snapshot is a point-in-time backup of an EBS volume stored in Amazon S3.

### Key Points
- Incremental backup
- Durable storage
- Volume restoration

### Use Cases
- Disaster recovery
- Backup automation

---

# 14. Amazon EFS

Amazon Elastic File System is a scalable file storage service that can be shared across multiple EC2 instances.

### Key Points
- Shared file access
- Auto scaling
- NFS protocol

### Use Cases
- Shared applications
- CMS platforms

---

# 15. Amazon FSx

Amazon FSx is a fully managed file storage service optimized for high-performance workloads.

### Types
- FSx for Windows
- FSx for Lustre
- FSx for NetApp ONTAP
- FSx for OpenZFS

### Use Cases
- Enterprise storage
- HPC workloads

---

# 16. Amazon S3

Amazon Simple Storage Service is an object storage service designed for durability and scalability.

### Key Points
- Unlimited storage
- Highly durable
- Versioning support

### Use Cases
- Static website hosting
- Backup storage

---

# 17. CloudWatch

Amazon CloudWatch is a monitoring service used to collect logs, metrics, and events from AWS resources.

### Key Points
- Real-time monitoring
- Dashboards
- Alarm creation

### Monitors
- CPU
- Memory
- Disk
- Network

---

# 18. CloudWatch Alarm

A CloudWatch Alarm monitors metrics and triggers actions when thresholds are met.

### Actions
- SNS notification
- Auto scaling trigger
- Lambda invocation

### Example
CPU utilization exceeds 80%.

---

# 19. IAM Role for EC2

An IAM Role allows EC2 instances to securely access AWS services without storing credentials.

### Key Points
- Temporary credentials
- Secure service access

### Example
EC2 accessing S3 bucket.

---

# 20. User Data

User Data is a startup script that runs automatically when an EC2 instance launches.

### Key Points
- Automates configuration
- Installs software
- Executes initialization tasks

### Example
Install Nginx during launch.

---

# 21. Spot Instance

Spot Instances use unused AWS capacity at reduced cost.

### Key Points
- Up to 90% cheaper
- Interruptible

### Use Cases
- Batch processing
- Testing workloads

---

# 22. Reserved Instance

Reserved Instances provide discounted pricing for long-term workloads.

### Key Points
- Lower cost
- 1-year or 3-year commitment

### Use Cases
- Predictable production workloads

---

# 23. Dedicated Host

A Dedicated Host is a physical AWS server dedicated to a single customer.

### Key Points
- Hardware isolation
- Compliance support

### Use Cases
- Licensing requirements

---

# 24. Systems Manager (SSM)

AWS Systems Manager allows secure management of EC2 instances without SSH access.

### Features
- Session Manager
- Patch Manager
- Automation

### Benefits
- Improved security
- Centralized management

---

# Interview Answer Format

For every AWS EC2 resource answer like this:

**What is it?**  
Define the resource clearly.

**Why is it used?**  
Explain purpose.

**Key features?**  
Mention technical capabilities.

**Real-world use case?**  
Provide practical example.

This structure makes your interview answers strong and professional.