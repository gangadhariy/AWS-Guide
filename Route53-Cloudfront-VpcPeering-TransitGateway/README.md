# AWS Route53, CloudFront, VPC Peering & Transit Gateway Notes

# 1. What is Route53?

Amazon Route53 is a highly available and scalable Domain Name System (DNS) web service provided by AWS.

Its primary purpose is to route user traffic from human-readable domain names (example.com) to backend resources such as:

- EC2 instances
- Load Balancers
- CloudFront distributions
- S3 static websites
- API Gateway
- External servers

DNS translates:

example.com
↓
34.120.xx.xx (IP address)

without DNS users would need to remember IP addresses.

The name Route53 comes from:

53 = standard DNS port number
(DNS uses UDP/TCP port 53)

Main functions of Route53:

1. Domain registration
2. DNS routing
3. Health checks
4. Traffic routing policies
5. Hosted zone management

---------------------------------------------------------

# 2. What is DNS?

DNS (Domain Name System) is an internet directory that converts domain names into IP addresses.

Example:

User enters:

www.google.com

DNS returns:

142.250.xx.xx

Browser connects to IP.

Flow:

Browser
↓
DNS Query
↓
Route53
↓
IP Returned
↓
Server Response

---------------------------------------------------------

# 3. What is Domain Registration?

Domain registration means purchasing ownership rights for a domain name.

Example domains:

company.com
myapp.in
devopscloud.io

When buying a domain:

You become domain owner for a subscription period.

AWS Route53 can work as:

Registrar + DNS Provider

Process:

Buy domain
↓
Create hosted zone
↓
Add DNS records
↓
Users access website

---------------------------------------------------------

# 4. What is Hosted Zone?

Hosted zone = container storing DNS records for a domain.

Example:

Domain:

example.com

Hosted zone contains:

A Record
MX Record
TXT Record
CNAME Record

These tell DNS where traffic should go.

---------------------------------------------------------

# 5. Public Hosted Zone

Definition:

A public hosted zone stores DNS records accessible from the public internet.

Meaning:

Anyone worldwide can resolve records.

Example:

mycompany.com

DNS records:

www.mycompany.com → Load Balancer
api.mycompany.com → Kubernetes ingress

Flow:

Internet User
↓
Public Hosted Zone
↓
Public IP/LB
↓
Application

Use cases:

- Websites
- APIs
- Public services

---------------------------------------------------------

# 6. Private Hosted Zone

Definition:

Private hosted zones store DNS records only accessible inside associated VPCs.

These records are invisible outside AWS network.

Example:

db.internal
redis.internal

Only EC2s/VPC workloads can access.

Flow:

EC2
↓
Private Hosted Zone
↓
Private IP
↓
Database

Use cases:

Microservices
Internal APIs
Databases

---------------------------------------------------------

# 7. Why NS and SOA records are created automatically?

When hosted zone is created AWS automatically adds:

## NS Record (Name Server)

Definition:

NS records identify which DNS servers are authoritative for a domain.

Example:

example.com

NS:

ns-123.awsdns.com
ns-555.awsdns.net

Purpose:

When someone asks:

Who manages example.com DNS?

Internet checks NS record.

Without NS:

DNS resolution fails.

---------------------------------------------------------

## SOA Record (Start Of Authority)

Definition:

SOA record stores metadata about DNS zone.

Contains:

Primary DNS server
Admin info
Refresh interval
Retry interval
Serial number

Purpose:

Helps DNS servers synchronize changes.

Example:

If record updated:

SOA serial increases

Other DNS servers refresh.

---------------------------------------------------------

# 8. DNS Record Types

## A Record

Definition:

Maps domain → IPv4 address

Example:

app.com

↓

192.168.1.10

---------------------------------------------------------

## AAAA Record

Definition:

Maps domain → IPv6 address

---------------------------------------------------------

## CNAME Record

Canonical Name

Definition:

Maps one domain to another domain.

Example:

api.company.com

↓

loadbalancer.aws.com

Useful:

Subdomains
External services

Cannot be used at root domain:

company.com ❌

---------------------------------------------------------

## MX Record

Mail Exchange

Definition:

Specifies mail server responsible for receiving emails.

Example:

hello@company.com

↓

Google mail server

---------------------------------------------------------

## TXT Record

Definition:

Stores text information.

Used for:

Domain verification

SPF

DKIM

DMARC

Security validation

---------------------------------------------------------

# 9. What is Alias Record?

Alias is AWS-specific DNS record.

Definition:

Alias routes domain directly to AWS resources without extra DNS lookup.

Supports:

ALB
CloudFront
S3
API Gateway

Example:

company.com

↓

Application Load Balancer

Difference:

CNAME:

Domain → Domain → IP

Alias:

Domain → AWS Resource

Alias faster and supports root domains.

---------------------------------------------------------

# 10. Route53 Routing Policies

Routing policies determine HOW traffic is routed.

---------------------------------------------------------

## Simple Routing

Definition:

Routes traffic to single resource.

Example:

website.com

↓

One server

---------------------------------------------------------

## Weighted Routing

Definition:

Splits traffic using percentages.

Example:

80%

↓

Old version

20%

↓

New version

Used in:

Canary deployments

---------------------------------------------------------

## Latency Routing

Definition:

Routes user to region with lowest latency.

India User:

↓

Mumbai region

US User:

↓

Virginia

Improves performance.

---------------------------------------------------------

## Failover Routing

Definition:

Routes traffic to backup resource if primary fails.

Example:

Primary:

ALB1

Backup:

ALB2

If ALB1 unhealthy:

Traffic → ALB2

---------------------------------------------------------

## Geolocation Routing

Definition:

Routes traffic based on user country.

India:

↓

Indian website

US:

↓

US website

---------------------------------------------------------

## Geoproximity Routing

Definition:

Routes based on physical distance from resources.

---------------------------------------------------------

## Multi-value Routing

Definition:

Returns multiple healthy IP addresses.

Improves availability.

---------------------------------------------------------

# 11. What is CloudFront CDN?

CloudFront = AWS Content Delivery Network

Definition:

CloudFront caches content in edge locations worldwide and serves users from nearest location.

Without CDN:

India User
↓
US Server

High latency

With CDN:

India User
↓
India Edge Location

Fast response

Cached content:

Images
Videos
CSS
JS

---------------------------------------------------------

# 12. What is Geo Restriction in CloudFront?

Definition:

Restricts access to content based on country.

Example:

Allow:

India

Block:

USA

Use cases:

Licensing
Regional restrictions
Compliance

---------------------------------------------------------

# 13. What is VPC Peering?

Definition:

VPC peering creates direct private network connectivity between two VPCs.

Traffic remains within AWS backbone.

Example:

VPC-A

10.0.0.0/16

↔

VPC-B

10.1.0.0/16

Requirements:

Non-overlapping CIDRs

Route updates

Limitations:

No transitive routing

Meaning:

If:

A ↔ B
B ↔ C

A cannot reach C

---------------------------------------------------------

# 14. What is Transit Gateway?

Definition:

Transit Gateway acts like a central network router connecting multiple VPCs and on-premise networks.

Instead of many peering links:

All connect to:

Transit Gateway

Architecture:

VPC1
 \
VPC2 ---- Transit Gateway
 /
VPC3

Benefits:

Central routing

Scalable

Simpler management

Supports:

VPC
VPN
Direct Connect

---------------------------------------------------------

# 15. Can Transit Gateway connect cross-account?

YES

Supports:

Different AWS accounts

Example:

Prod account VPC
↓

Transit Gateway

↓

Shared Services account VPC

---------------------------------------------------------

# 16. Can Transit Gateway connect cross-region?

YES

Using:

Transit Gateway Peering

Example:

Mumbai TGW

↓

Virginia TGW

Allows multi-region communication.

---------------------------------------------------------

# 17. VPC Peering vs Transit Gateway

| Feature | VPC Peering | Transit Gateway |
|---------|-------------|-----------------|
| Connectivity | 1-to-1 | Hub-and-spoke |
| Scalability | Poor | High |
| Transitive Routing | No | Yes |
| Multiple VPCs | Difficult | Easy |
| Cross Account | Yes | Yes |
| Cross Region | Yes | Yes |
| Routing Complexity | High | Low |
| Cost | Lower | Higher |
| Central Management | No | Yes |
| Best For | Few VPCs | Large environments |

---------------------------------------------------------

# 18. When to use what?

Use VPC Peering:

- Small architecture
- Few VPCs
- Low cost

Use Transit Gateway:

- Enterprise networking
- Many accounts
- Many VPCs
- Multi-region environments

---------------------------------------------------------

# Production Example

Company:

20 AWS accounts
100 VPCs
VPN to on-premise

Using VPC peering:

Thousands of connections

Hard to manage

Using Transit Gateway:

All VPCs

↓

Transit Gateway

↓

Simple centralized routing

This is why enterprises prefer Transit Gateway.
