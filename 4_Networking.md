# Module 3: Networking Part 1

## Module Overview

This module identifies the elements that build an elastic, secure virtual network that includes private, public, and protected subnets. The content includes strategies for a layered security approach to virtual private cloud (VPC) subnets.

## Learning Objectives

In this module, you learn how to:
- Assign IP addresses to various network components
- Define and configure virtual private cloud (VPC)
- Define and implement VPC traffic security
- Implement common networking use cases in the VPC

## Module Structure

### Why Two Networking Modules?

We are dividing networking into two separate modules because:
- **Module 3 (Part 1)**: Provides necessary networking knowledge for compute, storage, and databases
- **Module 10 (Part 2)**: Covers advanced networking after monitoring, automation, and containers

### Module 3 Topics

1. **IP Addressing**
   - Network component IP assignment
   - Addressing strategies

2. **VPC Fundamentals**
   - Virtual Private Cloud concepts
   - VPC configuration and setup

3. **VPC Traffic Security**
   - Network Access Control Lists (NACLs)
   - Security Groups
   - Layered security approach

4. **Knowledge Check**
   - Assessment of networking concepts

## Context for Future Modules

The networking knowledge from this module is essential for:
- **Module 4**: Compute services
- **Module 5**: Storage solutions
- **Module 6**: Database services

These resources need to sit within a network infrastructure, making VPC fundamentals crucial for understanding subsequent modules.

# IP Addressing

## Overview

This lesson explores the fundamental concepts of IP addressing. You will learn about IPv4 and IPv6 addressing formats.

---

## CIDR Notation Fundamentals

AWS uses the CIDR notation or Classless Inter-Domain Routing. Whenever you see an IP address followed by a forward slash and a number, it means that we are representing a range of IP addresses.

### Key Rule
**The bigger the number, the smaller the network, and the smaller the number, the bigger the network.**

### Example: /16 Network
- **Network**: `172.31.0.0/16`
- **Network portion**: `172.31` (fixed)
- **Host portion**: `2.15` (variable)
- **Subnet mask**: `255.255.0.0`

### Binary Representation
Using the `ipcalc` utility, a /16 network shows:
```
11111111.11111111.00000000.00000000
```
- **16 ones** = /16 (network bits)
- **16 zeros** = host bits

### CIDR Examples
- **0.0.0.0/0**: Entire IPv4 address space
- **/32**: Single IP address (most specific)
- **/16**: 65,536 addresses
- **/28**: 16 addresses (smallest AWS allows)

---

## IP Address Types

### IPv4 Addresses
**Developed**: Early 1980s  
**Format**: 32-bit addresses grouped into four 8-bit octets  
**Notation**: Numeric dot-decimal (e.g., 192.168.1.1)  
**Capacity**: 4.3 billion addresses  
**Limitation**: Addresses must be reused and masked

**Components**:
- **Network portion**: Identifies the network
- **Host portion**: Identifies resources within the network

**Example**: `10.22.0.0/16`
- Network: `10.22` (first two octets)
- Hosts: `0.0` (last two octets)
- Capacity: 255 × 255 = 65,536 resources

### IPv6 Addresses
**Developed**: 1998 (to replace IPv4)  
**Format**: 128-bit addresses  
**Notation**: Eight groups of four hexadecimal digits separated by colons  
**Capacity**: 340 trillion trillion trillion addresses  
**Benefit**: Unique addresses for all devices, automatic configuration

**Example**:
- Full: `50b2:6400:0000:0000:0000:6c3a:b17d:0:10a9`
- Shortened: `50b2:6400::6c3a:b17d:0:10a9`

---

## CIDR in AWS VPC

### CIDR Block Requirements
- **VPC CIDR blocks**: Up to 5 per VPC
- **Address ranges**: Cannot overlap
- **Block sizes**: Between /28 (16 IPs) and /16 (65,536 IPs)
- **IPv4**: Required for all VPCs and subnets
- **IPv6**: Optional association

### Subnet Mask
A 32-bit number that determines network vs. host portions:
- **Network bits**: Set to all ones (1)
- **Host bits**: Set to all zeros (0)
- **Purpose**: Defines which bits identify network vs. hosts

### AWS Supported CIDR Ranges

| Network Bits | Host Bits | CIDR | Max Resources | Use Case |
|--------------|-----------|------|---------------|----------|
| 28 bits | 4 bits | /28 | 16 | Small subnets |
| 24 bits | 8 bits | /24 | 256 | Standard subnets |
| 16 bits | 16 bits | /16 | 65,536 | Large networks |

### CIDR Notation Components
- **Dot notation**: Identifies the network (e.g., 10.0.0.0)
- **Slash notation**: Specifies subnet mask (e.g., /16)
- **Network identification**: First portion of IP address
- **Host identification**: Remaining portion for resources

---

## Key Concepts Summary

1. **CIDR blocks** represent ranges of IP addresses
2. **Larger numbers** after the slash = smaller networks
3. **Subnet masks** define network vs. host portions
4. **AWS VPCs** support both IPv4 and IPv6
5. **Planning** IP allocation is crucial for network design

---

## Next Steps

In the next lesson, you will learn the fundamentals of virtual private cloud (VPC) and subnets.

# Amazon Virtual Private Cloud (VPC)

## Overview

Amazon VPC is your network environment in the cloud. With Amazon VPC, you can launch AWS resources into a virtual network that you have defined. VPCs deploy into one of the AWS Regions and can host resources from any Availability Zone within its Region. You can use both IPv4 and IPv6 in your VPC for secure and easy access to resources and applications. A VPC is a virtual network dedicated to your AWS account.

---

## Subnets

### Definition
A subnet is a range of IP addresses in your VPC. You can launch AWS resources into a specified subnet. A subnet resides within one Availability Zone.

### Subnet Types
- **Public subnet**: For resources that must be connected to the internet
- **Private subnet**: For resources that won't be connected to the internet

### AWS IP Address Reservations
AWS reserves the first four IP addresses and the last IP address in each subnet CIDR block.

### Best Practice
Consider larger subnets over smaller ones (/24 and larger). You are less likely to waste or run out of IPs if you distribute your workload into larger subnets.

### Example: VPC CIDR Block Distribution
**VPC CIDR**: `172.31.0.0/16` (65,536 total IP addresses)

| Subnet Type | CIDR Block | IP Range | Available IPs |
|-------------|------------|----------|---------------|
| Public subnet 1 | 172.31.0.0/20 | 172.31.0.1 - 172.31.15.254 | 4,096 |
| Public subnet 2 | 172.31.16.0/20 | 172.31.16.1 - 172.31.31.254 | 4,096 |
| Private subnet 1 | 172.31.32.0/20 | 172.31.32.1 - 172.31.47.254 | 4,096 |
| Private subnet 2 | 172.31.48.0/20 | 172.31.48.1 - 172.31.63.254 | 4,096 |

---

## VPC Components

### Public Subnets

**Definition**: A public subnet is associated with a route table that has a route to an internet gateway.

**Characteristics**:
- Allows resources to be reached from the public internet
- Assigns public IP addresses automatically
- Acts as a two-way door for internet traffic
- Has routes to an internet gateway

**Components**:
- **Internet gateways**: Allow communication between VPC resources and the internet
- **Route tables**: Include routes to the internet gateway
- **Public IP addresses**: Can be reached from the internet
- **Private IP addresses**: Only reachable within the network

### Private Subnets

**Definition**: Private subnets allow indirect access to the internet while keeping traffic within your private network.

**Characteristics**:
- No direct internet connectivity
- Private IP addresses never change (unless manually modified)
- Traffic stays within private network
- Recommended for web-tier instances behind load balancers

**Best Practice**: Put web-tier instances in private subnets behind a load balancer placed in a public subnet.

### Internet Gateways

**Definition**: A horizontally scaled, redundant, and highly available VPC component that permits communication between instances in your VPC and the internet.

**Key Features**:
- No availability risks or bandwidth constraints
- Supports IPv4 and IPv6 traffic
- Performs Network Address Translation (NAT)

**Two Main Purposes**:
1. **Routing target**: Provides a target in route tables for internet-routable traffic
2. **IP protection**: Performs NAT by mapping public and private IP addresses

**NAT Process Example**:
- **Outbound**: Private IP (172.31.2.15) → Public IP (54.56.9.10)
- **Inbound**: Public IP (54.56.9.10) → Private IP (172.31.2.15)

### Route Tables

**Definition**: Contains a set of rules (routes) that determine where network traffic is directed.

**Key Concepts**:
- Every VPC automatically has a main route table
- Contains a local route for VPC communication (cannot be modified)
- Local route covers all instances within the VPC
- Custom route tables can be created

**Route Table Types**:
- **Public route table**: Has route to internet gateway (0.0.0.0/0 → IGW)
- **Private route table**: No internet gateway route

---

## Network Routing vs. Network Access

### Important Distinction
**Network routing** ≠ **Network access**

For successful communication between hosts:
1. **Network routing** must exist (route tables)
2. **Firewall rules** must allow traffic (security groups/NACLs)

### Local Route
Every route table includes a local route that:
- Enables routing between subnets within the VPC
- Uses "local" as the target
- Does NOT grant access (firewall rules still apply)

---

## Default VPC

### Overview
Each AWS account comes with a default VPC that is preconfigured for immediate use.

### Default VPC Characteristics
- **CIDR block**: Always /16 subnet mask (e.g., 172.31.0.0/16)
- **Capacity**: Up to 65,536 IP addresses
- **Subnets**: One public subnet per Availability Zone (/20 subnet mask = 4,096 addresses each)
- **Components**: Includes internet gateway and main route table
- **Purpose**: Allows immediate EC2 instance deployment without networking knowledge

### Why Default VPCs Exist
AWS provides default VPCs so users don't need networking knowledge before operating EC2 instances. They're provisioned at account creation and preconfigured for immediate usage.

---

## Key Takeaways

1. **Public subnets** have routes to internet gateways and assign public IPs
2. **Private subnets** provide indirect internet access and enhanced security
3. **Route tables** determine traffic flow, not access permissions
4. **Internet gateways** provide internet connectivity and perform NAT
5. **Default VPCs** enable immediate resource deployment
6. **Network routing** and **network access** are separate concepts

---

## Next Steps

Now that you have learned about the fundamentals of VPC, we will dive deeper into VPC concepts and learn about Elastic IP addressing.

# VPC Advanced Concepts

## Elastic Network Interfaces (ENI)

### Definition
An elastic network interface is a logical networking component in a VPC that represents a virtual network card.

### Key Features
- **Mobility**: Can be moved across EC2 instances in the same Availability Zone
- **Persistence**: Maintains private IP address, Elastic IP address, and MAC address
- **Flexibility**: Can be created, attached, detached, and reattached to instances

### Use Cases
- **Failover scenarios**: Move network interface to backup instance
- **Operating system limitations**: Use secondary ENI for additional connections
- **Network segmentation**: Separate management and application traffic

### Attributes That Follow the ENI
- Private IP address
- Elastic IP address (if assigned)
- MAC address
- Security group associations

---

## NAT Gateways

### Purpose
Network Address Translation (NAT) enables private subnet instances to access the internet while preventing inbound connections from the internet.

### Key Concepts
- **One-way connectivity**: Outbound only from private subnets
- **IP conservation**: Uses single public IP for multiple private instances
- **Security**: Prevents external traffic from reaching private instances

### How Private Subnets Access Internet

**Problem**: Private subnets don't have internet gateway routes, but instances need internet access for:
- Package updates (yum, apt, Windows Update)
- Database backups
- API calls to external services

**Solution**: NAT Gateway in public subnet

### NAT Gateway Architecture

```
Private Subnet → NAT Gateway → Internet Gateway → Internet
```

**Traffic Flow**:
1. **Outbound**: Instance → NAT Gateway → Internet Gateway → Internet
2. **Return Traffic**: Internet → Internet Gateway → NAT Gateway → Instance
3. **No Inbound**: Internet cannot initiate connections to private instances

### Route Table Configuration

**Private Subnet Route Table**:
- `10.0.0.0/16` → Local (VPC communication)
- `0.0.0.0/0` → NAT Gateway (internet access)

**Public Subnet Route Table**:
- `10.0.0.0/16` → Local (VPC communication)  
- `0.0.0.0/0` → Internet Gateway (internet access)

### NAT Gateway vs NAT Instance

| Feature | NAT Gateway | NAT Instance |
|---------|-------------|--------------|
| **Management** | AWS managed | Self-managed |
| **Availability** | Highly available within AZ | Single point of failure |
| **Bandwidth** | Up to 45 Gbps | Depends on instance type |
| **Configuration** | Automatic routing setup | Manual packet forwarding setup |
| **Cost** | Per hour + data processing | Instance cost + data transfer |

---

## Elastic IP Addresses

### Definition
A static, public IPv4 address designed for dynamic cloud computing that can be associated with any instance or network interface in your VPC.

### Key Features
- **Static**: IP address doesn't change
- **Portable**: Can be reassigned between instances
- **Immediate**: Traffic redirection happens instantly
- **Regional**: Up to 5 Elastic IPs per region by default

### Use Cases
- **Failover**: Mask instance failure by remapping to backup instance
- **DNS stability**: Maintain consistent IP for DNS records
- **Whitelisting**: Provide static IP for external service access

### BYOIP (Bring Your Own IP)
- Import your own IP address ranges to AWS
- Maintain existing IP reputation and configurations
- Requires ROA (Route Origin Authorization) setup

---

## Multi-AZ VPC Deployment

### Architecture Overview
Deploy VPC across multiple Availability Zones for high availability and fault tolerance.

### Components

#### Public Subnets (per AZ)
- **NAT Gateways**: One per AZ for redundancy
- **Load Balancer nodes**: Distributed across AZs
- **Internet Gateway**: Single gateway serves entire VPC

#### Private Subnets (per AZ)
- **Application servers**: Backend instances
- **Database instances**: Data tier resources
- **Internal services**: Non-internet facing resources

### Traffic Patterns

#### Inbound Traffic Flow
```
Internet → Internet Gateway → Load Balancer → Application Servers (Private Subnets)
```

#### Outbound Traffic Flow
```
Application Servers → NAT Gateway (same AZ) → Internet Gateway → Internet
```

### High Availability Benefits
- **Fault tolerance**: Outage in one AZ doesn't affect others
- **Load distribution**: Traffic spread across multiple AZs
- **Redundancy**: Multiple NAT gateways prevent single points of failure
- **Scalability**: Can add more AZs as needed

### Best Practices
1. **Deploy NAT gateways in each AZ** for redundancy
2. **Use Elastic Load Balancing** for traffic distribution
3. **Place application servers in private subnets** for security
4. **Configure route tables per AZ** for optimal routing

---

## Elastic Load Balancing Integration

### Regional Service
- **Scope**: Operates at Region level, not tied to specific AZ
- **Distribution**: Places nodes in specified public subnets
- **Availability**: Automatically handles AZ failures

### Configuration
- **Target subnets**: Specify public subnets for load balancer nodes
- **Backend targets**: Route traffic to private subnet instances
- **Health checks**: Monitor instance health across AZs

### Traffic Direction
- **Inbound**: Internet → Load Balancer → Private Instances
- **Outbound**: Private Instances → NAT Gateway → Internet

---

## Key Architectural Patterns

### 1. Three-Tier Architecture
- **Web Tier**: Load balancer in public subnets
- **Application Tier**: App servers in private subnets
- **Data Tier**: Databases in private subnets

### 2. Security Layers
- **Network isolation**: Public vs private subnets
- **Access control**: Security groups and NACLs
- **Traffic filtering**: Load balancer and NAT gateway controls

### 3. Availability Design
- **Multi-AZ deployment**: Resources across multiple AZs
- **Redundant components**: Multiple NAT gateways and subnets
- **Failover capability**: Elastic IPs for quick recovery

---

## Summary

- **ENIs** provide flexible network interface management
- **NAT Gateways** enable secure outbound internet access for private subnets
- **Elastic IPs** offer static public addressing for dynamic environments
- **Multi-AZ deployment** ensures high availability and fault tolerance
- **Load balancers** distribute inbound traffic while maintaining security

---

## Next Steps

In the next lesson, you will learn about securing your VPC with security groups and network ACLs.

# VPC Traffic Security

## Overview

VPC traffic security is implemented through two main firewall layers:
- **Security Groups**: Instance-level firewalls (stateful)
- **Network ACLs**: Subnet-level firewalls (stateless)

---

## Security Groups

### Definition
A security group acts as a virtual firewall for your instance to control inbound and outbound traffic at the network interface level.

### Key Characteristics
- **Level**: Network interface (ENI) level
- **State**: Stateful firewall
- **Rules**: Allow rules only (deny by default)
- **Assignment**: Must be manually assigned to instances
- **Reusability**: Can be reused across multiple instances

### Stateful Behavior
**Analogy**: Like a security guard at a party who remembers your face
- If inbound traffic is allowed, return traffic is automatically allowed
- No need for explicit outbound rules for response traffic
- Tracks connection state and allows related traffic

### Default Behavior
- **Inbound**: All traffic denied by default
- **Outbound**: All traffic allowed by default (can be modified)
- **New security groups**: No inbound rules, allow all outbound

### Rule Components
- **IP Protocol**: TCP, UDP, ICMP, etc.
- **Port Range**: Specific ports or port ranges
- **Source/Destination**: IP addresses, CIDR blocks, or other security groups
- **Action**: Allow only (implicit deny for unmatched traffic)

### Security Group Chaining
Create layered defense by referencing security groups within rules:

```
Web Tier SG → App Tier SG → Database SG
```

**Example Chain**:
- **Web Security Group**: Allows inbound HTTP/HTTPS from internet
- **App Security Group**: Allows inbound traffic only from Web Security Group
- **Database Security Group**: Allows inbound traffic only from App Security Group

### Custom Security Group Rules Example

**Web Server Security Group**:

| Direction | Protocol | Port | Source/Destination | Purpose |
|-----------|----------|------|-------------------|---------|
| Inbound | TCP | 80 | 0.0.0.0/0 | HTTP traffic |
| Inbound | TCP | 443 | 0.0.0.0/0 | HTTPS traffic |
| Outbound | TCP | 1433 | Database SG | SQL Server |
| Outbound | TCP | 3306 | Database SG | MySQL |

---

## Network Access Control Lists (NACLs)

### Definition
A network ACL is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.

### Key Characteristics
- **Level**: Subnet level
- **State**: Stateless firewall
- **Rules**: Both Allow and Deny rules supported
- **Assignment**: Automatically applied to all instances in subnet
- **Evaluation**: Rules processed in numerical order

### Stateless Behavior
**Analogy**: Like a checkpoint that doesn't remember previous interactions
- Inbound and outbound traffic evaluated independently
- Explicit rules required for both directions
- No connection tracking or state memory

### Default NACL
- **Behavior**: Allows all inbound and outbound IPv4 traffic
- **Modifiable**: Can be customized as needed
- **Automatic**: Every VPC comes with a default NACL

### Custom NACL
- **Default behavior**: Denies all traffic until rules are added
- **Association**: Must be explicitly associated with subnets
- **Flexibility**: Can be configured to allow or deny specific traffic

### NACL Rule Components
- **Rule Number**: Determines evaluation order (lowest first)
- **Type**: Traffic type (SSH, HTTP, custom, etc.)
- **Protocol**: Standard protocol number
- **Port Range**: Listening ports for traffic
- **Source**: For inbound rules (CIDR range)
- **Destination**: For outbound rules (CIDR range)
- **Action**: Allow or Deny

### Rule Evaluation
1. **Order**: Rules evaluated starting with lowest number
2. **First Match**: First matching rule is applied
3. **Asterisk Rule**: Default deny rule (cannot be modified)
4. **Processing**: Stops at first match, ignoring higher-numbered rules

---

## Security Groups vs Network ACLs Comparison

| Feature | Security Groups | Network ACLs |
|---------|----------------|--------------|
| **Level** | Instance (ENI) level | Subnet level |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Default** | Deny inbound, allow outbound | Allow all (default NACL) |
| **Assignment** | Manual to instances | Automatic for subnet instances |
| **Rule Evaluation** | All rules evaluated | First match wins |
| **Return Traffic** | Automatically allowed | Requires explicit rule |
| **Use Case** | Primary instance protection | Subnet-level backup defense |

---

## Layered Defense Architecture

### Defense in Depth Strategy
Implement multiple security layers for comprehensive protection:

1. **Network ACL**: First line of defense at subnet boundary
2. **Security Group**: Second line of defense at instance level
3. **Instance Firewall**: Optional third layer on the OS
4. **Application Security**: Fourth layer within applications

### Typical Multi-Tier Architecture

```
Internet → Internet Gateway → Public Subnet (Web Tier)
                                    ↓
                              Private Subnet (App Tier)
                                    ↓
                              Private Subnet (Database Tier)
```

**Security Implementation**:
- **Public Subnet NACL**: Allow HTTP/HTTPS, deny other protocols
- **Web Security Group**: Allow 80/443 from internet, database ports to app tier
- **App Security Group**: Allow traffic from web tier, database ports to DB tier
- **Database Security Group**: Allow traffic only from app tier

---

## Use Cases and Best Practices

### Network ACL Use Cases
- **Subnet-level protection**: Backup layer of defense
- **Compliance requirements**: Explicit deny rules for regulations
- **Broad traffic filtering**: Block entire IP ranges or protocols
- **Emergency response**: Quickly block malicious traffic at subnet level

### Security Group Use Cases
- **Application-specific rules**: Fine-grained instance protection
- **Service communication**: Control inter-service traffic
- **Dynamic environments**: Easy rule updates without subnet changes
- **Micro-segmentation**: Isolate individual workloads

### Best Practices
1. **Use both layers**: Implement defense in depth
2. **Principle of least privilege**: Allow only necessary traffic
3. **Security group chaining**: Reference groups instead of IP ranges
4. **Regular audits**: Review and update rules periodically
5. **Logging**: Enable VPC Flow Logs for traffic analysis

---

## Example: Trusted Remote Access

### Scenario
Allow administrative access from trusted remote computer while blocking all other internet traffic.

### Configuration
**Network ACL Rules**:
- Allow inbound SSH (port 22) from 172.31.1.2/32
- Allow outbound response traffic
- Deny all other traffic

**Security Group Rules**:
- Allow inbound SSH (port 22) from 172.31.1.2/32
- Allow outbound traffic as needed

### Result
- Instances can communicate within subnet (local route)
- Remote computer (172.31.1.2) can access instances for administration
- All other internet traffic is blocked
- Provides secure administrative access while maintaining isolation

---

## Summary

- **Security Groups** provide stateful, instance-level firewall protection
- **Network ACLs** offer stateless, subnet-level firewall capabilities
- **Layered approach** combines both for comprehensive security
- **Stateful vs Stateless** determines how return traffic is handled
- **Rule evaluation** differs between the two firewall types
- **Best practices** emphasize defense in depth and least privilege

---

## Next Steps

In the next module, you learn how to use compute services in your applications. But first, there is a short knowledge check.

# VPC Traffic Security

## Overview

VPC traffic security is implemented through two main firewall layers:
- **Security Groups**: Instance-level firewalls (stateful)
- **Network ACLs**: Subnet-level firewalls (stateless)

---

## Security Groups

### Definition
A security group acts as a virtual firewall for your instance to control inbound and outbound traffic at the network interface level.

### Key Characteristics
- **Level**: Network interface (ENI) level
- **State**: Stateful firewall
- **Rules**: Allow rules only (deny by default)
- **Assignment**: Must be manually assigned to instances
- **Reusability**: Can be reused across multiple instances

### Stateful Behavior
**Analogy**: Like a security guard at a party who remembers your face
- If inbound traffic is allowed, return traffic is automatically allowed
- No need for explicit outbound rules for response traffic
- Tracks connection state and allows related traffic

### Default Behavior
- **Inbound**: All traffic denied by default
- **Outbound**: All traffic allowed by default (can be modified)
- **New security groups**: No inbound rules, allow all outbound

### Rule Components
- **IP Protocol**: TCP, UDP, ICMP, etc.
- **Port Range**: Specific ports or port ranges
- **Source/Destination**: IP addresses, CIDR blocks, or other security groups
- **Action**: Allow only (implicit deny for unmatched traffic)

### Security Group Chaining
Create layered defense by referencing security groups within rules:

```
Web Tier SG → App Tier SG → Database SG
```

**Example Chain**:
- **Web Security Group**: Allows inbound HTTP/HTTPS from internet
- **App Security Group**: Allows inbound traffic only from Web Security Group
- **Database Security Group**: Allows inbound traffic only from App Security Group

### Custom Security Group Rules Example

**Web Server Security Group**:

| Direction | Protocol | Port | Source/Destination | Purpose |
|-----------|----------|------|-------------------|---------|
| Inbound | TCP | 80 | 0.0.0.0/0 | HTTP traffic |
| Inbound | TCP | 443 | 0.0.0.0/0 | HTTPS traffic |
| Outbound | TCP | 1433 | Database SG | SQL Server |
| Outbound | TCP | 3306 | Database SG | MySQL |

---

## Network Access Control Lists (NACLs)

### Definition
A network ACL is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.

### Key Characteristics
- **Level**: Subnet level
- **State**: Stateless firewall
- **Rules**: Both Allow and Deny rules supported
- **Assignment**: Automatically applied to all instances in subnet
- **Evaluation**: Rules processed in numerical order

### Stateless Behavior
**Analogy**: Like a checkpoint that doesn't remember previous interactions
- Inbound and outbound traffic evaluated independently
- Explicit rules required for both directions
- No connection tracking or state memory

### Default NACL
- **Behavior**: Allows all inbound and outbound IPv4 traffic
- **Modifiable**: Can be customized as needed
- **Automatic**: Every VPC comes with a default NACL

### Custom NACL
- **Default behavior**: Denies all traffic until rules are added
- **Association**: Must be explicitly associated with subnets
- **Flexibility**: Can be configured to allow or deny specific traffic

### NACL Rule Components
- **Rule Number**: Determines evaluation order (lowest first)
- **Type**: Traffic type (SSH, HTTP, custom, etc.)
- **Protocol**: Standard protocol number
- **Port Range**: Listening ports for traffic
- **Source**: For inbound rules (CIDR range)
- **Destination**: For outbound rules (CIDR range)
- **Action**: Allow or Deny

### Rule Evaluation
1. **Order**: Rules evaluated starting with lowest number
2. **First Match**: First matching rule is applied
3. **Asterisk Rule**: Default deny rule (cannot be modified)
4. **Processing**: Stops at first match, ignoring higher-numbered rules

---

## Security Groups vs Network ACLs Comparison

| Feature | Security Groups | Network ACLs |
|---------|----------------|--------------|
| **Level** | Instance (ENI) level | Subnet level |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Default** | Deny inbound, allow outbound | Allow all (default NACL) |
| **Assignment** | Manual to instances | Automatic for subnet instances |
| **Rule Evaluation** | All rules evaluated | First match wins |
| **Return Traffic** | Automatically allowed | Requires explicit rule |
| **Use Case** | Primary instance protection | Subnet-level backup defense |

---

## Layered Defense Architecture

### Defense in Depth Strategy
Implement multiple security layers for comprehensive protection:

1. **Network ACL**: First line of defense at subnet boundary
2. **Security Group**: Second line of defense at instance level
3. **Instance Firewall**: Optional third layer on the OS
4. **Application Security**: Fourth layer within applications

### Typical Multi-Tier Architecture

```
Internet → Internet Gateway → Public Subnet (Web Tier)
                                    ↓
                              Private Subnet (App Tier)
                                    ↓
                              Private Subnet (Database Tier)
```

**Security Implementation**:
- **Public Subnet NACL**: Allow HTTP/HTTPS, deny other protocols
- **Web Security Group**: Allow 80/443 from internet, database ports to app tier
- **App Security Group**: Allow traffic from web tier, database ports to DB tier
- **Database Security Group**: Allow traffic only from app tier

---

## Use Cases and Best Practices

### Network ACL Use Cases
- **Subnet-level protection**: Backup layer of defense
- **Compliance requirements**: Explicit deny rules for regulations
- **Broad traffic filtering**: Block entire IP ranges or protocols
- **Emergency response**: Quickly block malicious traffic at subnet level

### Security Group Use Cases
- **Application-specific rules**: Fine-grained instance protection
- **Service communication**: Control inter-service traffic
- **Dynamic environments**: Easy rule updates without subnet changes
- **Micro-segmentation**: Isolate individual workloads

### Best Practices
1. **Use both layers**: Implement defense in depth
2. **Principle of least privilege**: Allow only necessary traffic
3. **Security group chaining**: Reference groups instead of IP ranges
4. **Regular audits**: Review and update rules periodically
5. **Logging**: Enable VPC Flow Logs for traffic analysis

---

## Example: Trusted Remote Access

### Scenario
Allow administrative access from trusted remote computer while blocking all other internet traffic.

### Configuration
**Network ACL Rules**:
- Allow inbound SSH (port 22) from 172.31.1.2/32
- Allow outbound response traffic
- Deny all other traffic

**Security Group Rules**:
- Allow inbound SSH (port 22) from 172.31.1.2/32
- Allow outbound traffic as needed

### Result
- Instances can communicate within subnet (local route)
- Remote computer (172.31.1.2) can access instances for administration
- All other internet traffic is blocked
- Provides secure administrative access while maintaining isolation

---

## Summary

- **Security Groups** provide stateful, instance-level firewall protection
- **Network ACLs** offer stateless, subnet-level firewall capabilities
- **Layered approach** combines both for comprehensive security
- **Stateful vs Stateless** determines how return traffic is handled
- **Rule evaluation** differs between the two firewall types
- **Best practices** emphasize defense in depth and least privilege

---

## Next Steps

In the next module, you learn how to use compute services in your applications. But first, there is a short knowledge check.

# Module 3: Networking - Knowledge Check

## Question 1: VPC Regional Scope

**True or false: A single Amazon VPC can span multiple regions.**

**Answer: False**

**Explanation**: 
- VPCs are region-scoped services and cannot span multiple regions
- Each VPC exists within a single AWS Region
- Cross-region communication requires VPC peering between separate VPCs in different regions

---

## Question 2: Making Subnets Public

**What action must you take to make a subnet public?**

A. Route outbound traffic from the subnet  
B. Route inbound traffic from the internet gateway  
C. Route outbound traffic to the internet gateway  
D. Subnets are public by default

**Answer: C - Route outbound traffic to the internet gateway**

**Explanation**:
- Subnets are private by default
- Public subnets require route table configuration with a route to an internet gateway
- Route table must include: 0.0.0.0/0 → Internet Gateway
- This routing configuration makes the subnet accessible from the internet

---

## Question 3: NAT Gateway Function

**What function does the NAT gateway serve?**

A. Load balancing incoming traffic to multiple instances  
B. Allows internet traffic initiated by private subnet instances  
C. Allows instances to communicate between subnets  
D. Increases security for instances in a public subnet

**Answer: B - Allows internet traffic initiated by private subnet instances**

**Explanation**:
- NAT gateways enable outbound internet access for instances in private subnets
- Load balancing is handled by Elastic Load Balancers, not NAT gateways
- Inter-subnet communication uses local routes in VPC route tables
- Public subnet instances already have direct internet access

---

## Question 4: Subnet Traffic Filtering

**What should you use to create traffic filtering rules for a subnet?**

A. NAT gateway  
B. Route table  
C. Security group  
D. Network ACL

**Answer: D - Network ACL**

**Explanation**:
- Network ACLs operate at the subnet level and provide traffic filtering
- NAT gateways route traffic but don't filter it
- Route tables determine traffic paths, not access control
- Security groups operate at the instance level, not subnet level

---

## Question 5: Default Security Group Ports

**Which ports are open by default when you create a new security group? (Select two)**

A. Nothing is allowed inbound  
B. Nothing is allowed outbound  
C. Anything is allowed inbound  
D. Anything is allowed outbound  
E. Inbound traffic is allowed on public subnets

**Answer: A and D - Nothing is allowed inbound, Anything is allowed outbound**

**Explanation**:
- Security groups are not subnet-aware and don't differentiate between public/private subnets
- New security groups have restrictive inbound defaults
- **Inbound**: All traffic denied by default (explicit allow rules required)
- **Outbound**: All traffic allowed by default (can be modified as needed)

---

## Key Concepts Summary

1. **VPC Scope**: Regional service limitation
2. **Public Subnets**: Require internet gateway routing
3. **NAT Gateways**: Enable private subnet outbound internet access
4. **Network ACLs**: Subnet-level traffic filtering
5. **Security Groups**: Instance-level firewall with default deny inbound policy