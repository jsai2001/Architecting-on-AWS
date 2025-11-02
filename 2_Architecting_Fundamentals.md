# Module 2: Architecting Fundamentals

## Module Overview

This module introduces fundamentals of architecting applications using AWS, beginning with a business request and exploring different methods to meet business requirements.

### Learning Objectives

- Use AWS services while architecting applications
- Use AWS infrastructure in applications
- Use the Well-Architected Framework to build applications on AWS infrastructure

### Module Topics

1. **AWS Services**: Understanding cloud value proposition and why cloud is today's standard
2. **AWS Infrastructure**: Data centers, Availability Zones, edge locations, and Local Zones
3. **Well-Architected Framework**: Best practices for running workloads in the cloud
4. **Knowledge Check**: Assessment of module concepts

---

## AWS Services

### What is AWS?

AWS is the world's most comprehensive and adopted cloud platform, offering:

- **200+ Services**: Compute, database, storage, messaging, containers, serverless
- **Global Reach**: Data centers worldwide for optimal user proximity
- **Security & Reliability**: Robust, secure infrastructure
- **Pay-as-you-go Model**: Cost-effective alternative to traditional infrastructure

### Traditional vs. Cloud Infrastructure

**Traditional (On-premises)**:
- Hardware procurement and configuration
- Physical rack installation
- Cooling, venting, networking, cabling management
- High upfront costs and maintenance

**Cloud (AWS)**:
- Pay-as-you-go model
- Instant resource provisioning
- Managed infrastructure
- Shared responsibility model

### Why Customers Choose AWS

**Primary Drivers**:
- Increase agility
- Reduce complexity and risk
- Lower time to market

**Key Advantages**:
- **Rapid Deployment**: EC2 instances and containers in minutes
- **Global Operations**: Worldwide scaling capabilities
- **Cost Optimization**: Pay only for what you use
- **Innovation Support**: Quick idea-to-implementation cycle

### AWS Service Portfolio

AWS provides managed services across multiple categories:

- **Compute**: EC2 (virtual machines), Lambda (serverless), containers
- **Database**: Relational and NoSQL options
- **Storage**: Object, file, and block storage
- **Messaging**: Queue and notification services
- **Networking**: VPC, load balancing, content delivery

### Customer Diversity

AWS serves all organization types:
- Enterprise customers
- Startups
- Small and large corporations
- Government and public sector

---

## Benefits of AWS Services

1. **Accelerate time to market**
2. **Increase innovation**
3. **Scale seamlessly**
4. **Optimize costs**
5. **Minimize security vulnerabilities**
6. **Reduce management and complexity**

---

## AWS Service Categories

AWS offers a comprehensive set of global cloud-based products across multiple categories:

- **Compute**
- **Storage**
- **Database**
- **Analytics**
- **Networking**
- **Mobile**
- **Developer Tools**
- **Management Tools**
- **Internet of Things (IoT)**
- **Security**
- **Enterprise Applications**

These services help organizations move faster, scale effectively, and lower IT costs by covering infrastructure, foundation, and application layers.

---

## Next Steps

The following lesson will introduce the underlying AWS infrastructure on which applications are hosted.

AWS Infrastructure
This lesson will introduce you to the organization of the AWS infrastructure, and how you can leverage it to build effective applications.

To watch the instructor video choose the play button:

Now it's time to talk about AWS infrastructure or how is AWS global infrastructure organized. Well, you can imagine yourself in a conversation where someone in a project asks, "How is AWS global infrastructure organized?"

After watching this module, you will be better prepared to answer these questions. So I am going to talk about data centers, Availability Zones, AWS Regions, AWS Local Zones, and edge locations. All of these are physical locations where you can run AWS services. Starting with the AWS data centers, they are undisclosed facilities. It doesn't have AWS logo on the data center. They are undisclosed facilities. And AWS operates data centers. These data centers, they host thousands of servers, and each location uses AWS proprietary network and hardware equipment.

Now data centers are organized into Availability Zones. So Availability Zone is the minimum granularity that you use when you want to spin up an AWS resource, such as for example, an EC2 instance or a storage volume. Availability Zones are clusters of data centers. So we don't talk about I'm launching an instance in one specific data center. The minimal granularity, the location that you specified, that is the finest possible, is an Availability Zone in AWS. Data centers makes an Availability Zone. And an Availability Zone, multiple Availability Zones make a Region. So an Availability Zone is a cluster of data centers, and they are designed for fault isolation. And Availability Zones are interconnected within themselves using high-speed, low-latency, private links. So you can have resources running in one Availability Zone in one Region, communicating in high speed and low latency with other resources in another Availability Zone as part of the same Region. You can and should use multiple Availability Zones to leverage a higher availability.

At the end of this course, we will also talk about some strategies to use multi-Region. So what is a Region? Well, a Region is a geographical location where AWS implements the Availability Zones. Each Region is completely independent from each other and you can have multiple Regions in a country. For example, the United States has some Regions in the East Coast, some Regions in the West Coast. You can have Regions in Latin America. You can have Regions in Asia. And in this course, I am going to spin up some resources in other Regions to show you how easy it is to pivot from one Region to another. So a Region is a geographical location that you should choose to implement your workloads. And by your workloads, I mean virtual machines, storage, serverless, all the services available on that large spectrum of services. Regions are completely independent. And you can sometimes spin up resources in one specific Region without specifying an AZ. That's because this service is Region scoped, or sometimes, you need to specify a specific AZ within a Region. So when do you know when a service has a Region scope level versus an Availability Zone scope level?

Let me show you something on the whiteboard. So every time you see something, dash, something, dash, a number, we are talking about the API name of a Region. If you see us-east-1, we are talking about the first Region of the east of the US, also known as the Virginia Region. This is the name of an AWS Region. Now, when you see a letter, A, B, C, D, E right after the number, b, c, a, e, d, right? We are talking about an Availability Zone. So if you see us-east-1a, us-east-1b, someone is talking about one Availability Zone within the Virginia Region, which is us-east-1. Sometimes you deploy services in an Availability Zone. Sometimes you deploy services in the Region. How do you discover if a service you are working with has an AZ scope level, meaning that you must also replicate this service in another Availability Zone for higher availability, or a Region scope level? Well, there is a very simple trick. When you are creating your resource, if the service asks you to specify an Availability Zone, then the service operates within an Availability Zone scope level. If the service, just like Amazon S3, DynamoDB, if just asks you to specify a Region, it means that this service has a Region scope level, and it is very likely to encompass multiple AZs inside that Region for you.

Now, how do I choose which Region I am going to use? There are Regions in the east coast of the US, in the west coast, in Sao Paulo, in Mumbai, right? So there are mainly four factors that we use to determine which Region we are going to use. The first one or the most obvious one is the latency between that specific AWS Region, that Region location and your end users. It is very common that users, for example, they give up shopping in a shopping website if the requests take too long to load. So if you have users in the east coast of the US, you would l# AWS Global Infrastructure

## Overview

The AWS Global Cloud Infrastructure is a secure, extensive, and reliable cloud platform offering 200+ fully featured services from data centers globally.

---

## Infrastructure Components

### AWS Data Centers

- **Founded**: 2006 to provide rapid and secure infrastructure
- **Innovation**: Continuous design improvements to protect against man-made and natural risks
- **Scale**: Large, global scale operations
- **Security**: Automated systems, controls, and third-party audits
- **Trust**: Used by highly-regulated organizations worldwide

### Availability Zones (AZs)

**Definition**: One or more discrete data centers with redundant power, networking, and connectivity within an AWS Region.

**Key Features**:
- Multiple isolated areas within geographic locations
- Redundant infrastructure design
- Fault tolerance through distribution
- Automatic or manual AZ selection for instances

**Best Practice**: Distribute instances across multiple AZs for high availability

### Regions

**Definition**: Geographic areas containing multiple, isolated, and physically separate Availability Zones.

**Benefits**:
- Greatest possible fault tolerance and stability
- Reduced latency to end users
- No upfront expenses or long-term commitments
- Scalable without global infrastructure maintenance

### AWS Local Zones

**Purpose**: Extension of Availability Zones with limited service scope for ultra-low latency applications.

**Capabilities**:
- Compute, storage, and database services
- Single-digit millisecond latency
- Connected to parent region via private network
- High bandwidth, secure access to other AWS services

**Use Cases**:
- Live video streaming
- Real-time gaming
- Media and entertainment content creation
- Machine learning hosting and training

### AWS Edge Locations

**Purpose**: Nearest points to requesters for caching and content delivery.

**Key Services**:
- **Route 53**: Global DNS resolver
- **Amazon CloudFront**: Content caching and delivery

**How It Works**:
1. Content stored in origin (e.g., S3 in São Paulo)
2. Customer requests content from edge location
3. CloudFront retrieves from origin and caches at edge
4. Subsequent requests served from cache

---

## Region Selection Factors

### 1. Latency
- **Goal**: Close proximity to customers for better performance
- **Strategy**: Choose regions near your user base
- **Benefit**: Lower response times and improved user experience

### 2. Cost
- **Variation**: AWS service costs differ by region
- **Factors**: Local internet costs, connectivity expenses, equipment pricing
- **Strategy**: Compare pricing across regions for optimal cost-effectiveness
- **Example**: Production in one region, Dev/Test/QA in cost-effective region

### 3. Governance and Compliance
- **Requirements**: Data sovereignty and privacy laws
- **Constraint**: Store citizen data within specific countries
- **Solution**: Use storage services in compliant regions

### 4. Service Availability
- **Reality**: Not all services available in every region
- **Popular Services**: EC2, S3, EBS, DynamoDB available globally
- **Resource**: AWS service availability table by region

---

## Local Zones vs. Edge Locations

| Feature | Local Zones | Edge Locations |
|---------|-------------|----------------|
| **Purpose** | Ultra-low latency compute | Content caching and delivery |
| **Services** | Compute, storage, databases | Route 53, CloudFront |
| **Use Case** | Real-time applications | Content distribution |
| **Latency** | Single-digit milliseconds | Fast content delivery |
| **Scope** | Limited AWS services | Specific caching services |

---

## Edge Location Use Case Example

**Scenario**: Video file stored in Amazon S3 in South America

**Process**:
1. Content originates in S3 (South America)
2. Customer in Asia requests video
3. Edge location caches content locally
4. Faster delivery to Asian customers
5. Improved performance through proximity

---

## Next Steps

The following lesson covers the Well-Architected Framework for building efficient cloud applications on AWS infrastructure.

# AWS Well-Architected Framework

## Overview

The AWS Well-Architected Framework helps cloud architects build secure, high-performing, resilient, and efficient application infrastructure. It provides a consistent approach and best practices for evaluating architectures and implementing scalable designs.

**Key Question**: "How do I know the best practices for using the cloud with hundreds of services and multiple infrastructure arrangements?"

**Answer**: The AWS Well-Architected Framework (WAF)

---

## AWS Architect Responsibilities

Solutions architects manage an organization's cloud computing architecture across storage, networking, compute, data lakes, analytics, and big data.

### Three-Phase Approach

#### 1. Plan
- Set technical cloud strategy with business leads
- Analyze ROI requirements
- Evaluate solutions for business needs and requirements

#### 2. Research
- Investigate cloud services specifications and workload requirements
- Review existing workload architectures
- Create infrastructure diagrams
- Map customer requirements to AWS services
- Design prototype solutions

#### 3. Build
- Design transformation roadmap with milestones, work streams, and owners
- Manage adoption and migration processes
- Handle transitions from traditional (on-premises) infrastructure

---

## Well-Architected Framework Structure

### Framework Approach
- **Method**: Question-based evaluation
- **Purpose**: Encourage reflection on architecture and cloud infrastructure
- **Format**: Document containing questions across six pillars
- **Usage**: Common in consultant-customer conversations

### Six Pillars

#### 1. Security
**Focus**: Protect data, systems, and assets using cloud technologies

**Key Areas**:
- Build security policies and processes
- Enable auditing and traceability
- Monitor, alert, and audit actions in real-time
- Manage encryption keys effectively

#### 2. Cost Optimization
**Focus**: Run systems to deliver business value at the lowest price point

**Key Areas**:
- Use appropriate services, resources, and configurations
- Achieve cost efficiency with fluctuating resource needs
- Minimize resource usage during low demand periods

#### 3. Reliability
**Focus**: Workload performs intended functions correctly and consistently

**Key Areas**:
- Operate and test workloads through total lifecycle
- Support recovery from failures
- Handle increased demand
- Mitigate disruptions (e.g., Availability Zone failures)

#### 4. Performance Efficiency
**Focus**: Use computing resources efficiently to meet system requirements

**Key Areas**:
- Maintain efficiency as demand changes and technologies evolve
- Optimize instances, storage, databases, space, and time
- Combine multiple approaches for optimal solutions

#### 5. Operational Excellence
**Focus**: Support development and run workloads effectively

**Key Areas**:
- Gain insight into operations
- Continuously improve supporting processes and procedures
- Deliver business value through effective operations

#### 6. Sustainability
**Focus**: Address long-term environmental, economic, and societal impact

**Key Areas**:
- Identify opportunities to reduce sustainability impact
- Update systems for performance efficiencies
- Learn, measure, and improve using environmental best practices

---

## AWS Well-Architected Tool

### Overview
- **Cost**: Free in AWS Management Console
- **Purpose**: Self-service tool for workload evaluation
- **Audience**: Architects and managers (no AWS SA required)

### Capabilities
- Review existing workloads against latest AWS best practices
- Identify high-risk issues
- Record improvements
- Generate comprehensive reports
- Focus on specific pillars (e.g., security, high availability)

### Usage Statistics
- Used in tens of thousands of workload reviews
- Common tool for solutions architects conducting reviews
- Central place for best practices and guidance

### Tool Features
- **Workloads**: Manage and evaluate multiple workloads
- **Reviews**: Conduct regular assessments
- **Best Practices**: Access latest architectural guidance

---

## Summary

The Well-Architected Framework provides:
- Structured approach to cloud architecture evaluation
- Question-based methodology for reflection
- Six comprehensive pillars covering all aspects of cloud operations
- Practical tool for implementation and ongoing assessment
- Foundation for building scalable, secure, and efficient cloud solutions

---

## Next Steps

Complete the Knowledge Check and practice concepts through AWS Labs.

# Module 1: Architecting Fundamentals - Knowledge Check

## Question 1: AWS Architect Responsibilities

**Which of the following is the best example of one responsibility of an AWS architect?**

A. Monitoring alarms for disaster response  
B. Maintain application-level code in the AWS cloud  
C. Manage access to a group of AWS accounts  
D. Analyze solutions for business needs and requirements  

**Answer: D**

**Explanation:**
- **A**: More aligned with DevOps/CloudOps roles
- **B**: Developer responsibility in DevOps model
- **C**: Security analyst responsibility for governance standards
- **D**: Core solutions architect responsibility - analyzing business requirements

---

## Question 2: AWS Infrastructure Components

**Which of the following is a cluster of data centers within a geographic location with low latency network connections?**

A. Availability Zone  
B. Region  
C. Edge Location  
D. Outposts  

**Answer: A**

**Explanation:**
- **A**: Correct - AZ is a cluster of data centers within a geographic location
- **B**: Region contains multiple AZs across a geographic area
- **C**: Edge locations are spread across multiple geographic locations globally
- **D**: Outposts is physical hardware (rack or server), not a cluster of data centers

---

## Question 3: Region Selection Factors

**Which of the following factors do you consider when picking an AWS region? (Select TWO)**

A. Local data regulations  
B. Operating system requirements  
C. Latency to end users  
D. Support for hybrid networking  
E. Programming language of your application  

**Answer: A and C**

**Explanation:**
- **A**: Valid - compliance with data sovereignty and privacy laws
- **B**: Not relevant - regions are OS-agnostic
- **C**: Valid - proximity to users reduces latency
- **D**: Not a primary region selection factor
- **E**: Not relevant - regions are programming language-agnostic

---

## Question 4: Multi-AZ Deployment Benefits

**What is the primary benefit of deploying your applications into multiple Availability Zones?**

A. Stronger security policies for resources  
B. Decreased latency to resources  
C. High availability for resources  
D. There is no benefit to this design  

**Answer: C**

**Explanation:**
- **A**: Security policies remain the same regardless of AZ deployment
- **B**: AZs are already connected with high-speed, low-latency links
- **C**: Correct - distributing across AZs provides fault tolerance and high availability
- **D**: Incorrect - multiple benefits exist for multi-AZ design

---

## Question 5: Well-Architected Framework Pillars

**The principle of least privilege is a principle under which Well-Architected Framework pillar?**

A. Operational Excellence  
B. Security  
C. Reliability  
D. Performance Efficiency  

**Answer: B**

**Explanation:**
- **A**: Not directly related to access controls
- **B**: Correct - least privilege is a core security principle for access controls
- **C**: Not connected to access privilege concepts
- **D**: Not related to user access management

---

## Summary

This knowledge check covers key concepts from Module 1:
- AWS architect roles and responsibilities
- Infrastructure components (AZs, Regions, Edge Locations)
- Region selection criteria
- Multi-AZ deployment benefits
- Well-Architected Framework security principles

# AWS Management Console and CLI Lab

## Task 1: AWS Management Console Navigation

### 1.5: Dashboard Widgets Management

**Add Widgets:**
1. Choose **+ Add widgets**
2. Drag widget from Add widgets menu to console page

**Rearrange Widgets:**
1. Choose widget title bar (e.g., Favorites)
2. Drag to new location

**Resize Widgets:**
1. Select Recently Visited widget
2. Drag bottom-right corner to resize

**Remove Widgets:**
1. Choose Welcome to AWS widget
2. Select widget actions ellipsis (⋮)
3. Choose **Remove widget**

---

## Task 2: Create S3 Bucket (Console)

### Steps:
1. **Navigate to S3:**
   - Services → All Services → Storage → S3
   - Or search "S3" in search bar

2. **Create Bucket:**
   - Choose **Buckets** → **Create bucket**
   - **Bucket name**: `labbucket-NUMBER` (replace NUMBER with random digits)
   - **Region**: Match LabRegion value
   - Leave default settings
   - Choose **Create bucket**

### Requirements:
- Bucket names must be globally unique and DNS compliant
- Example: `labbucket-987987`

---

## Task 3: Upload Object to S3 (Console)

### Steps:
1. **Download Image:**
   - Right-click provided image link
   - Save as `HappyFace.jpg`

2. **Upload to S3:**
   - Select `labbucket-xxxxx` bucket
   - Choose **Upload** → **Add files**
   - Select `HappyFace.jpg`
   - Choose **Upload** → **Close**

---

## Task 4: S3 Operations via AWS CLI

### 4.1: Connect to Command Host

1. **Access EC2:**
   - Search and choose **EC2**
   - Choose **Instances** → Select **Command Host**
   - Choose **Connect** → **Session Manager** tab
   - Choose **Connect**

### 4.2: AWS CLI S3 Commands

**List all buckets:**
```bash
aws s3 ls
```

**Create bucket:**
```bash
aws s3 mb s3://labclibucket-NUMBER
```
*Replace NUMBER with random digits*

**Verify bucket creation:**
```bash
aws s3 ls
```

**Upload file:**
```bash
aws s3 cp /home/ssm-user/HappyFace.jpg s3://labclibucket-NUMBER
```

**List bucket contents:**
```bash
aws s3 ls s3://labclibucket-NUMBER
```

---

## Key Concepts

### Amazon S3
- Object storage service with industry-leading scalability
- Use cases: data lakes, websites, mobile apps, backup, IoT, big data analytics
- Globally unique bucket naming required

### AWS CLI
- Open-source tool for command-line AWS service interaction
- High-level S3 commands built on S3 API operations
- Simplifies S3 object management

### Session Manager
- Connect to EC2 instances without exposing SSH ports
- No firewall or VPC security group modifications needed

---

## Lab Completion

### Achievements:
- ✅ Explored AWS Management Console
- ✅ Created resources via Console
- ✅ Used AWS CLI for resource management
- ✅ Created and managed S3 buckets and objects

### Cleanup:
1. Return to AWS Management Console
2. Choose **AWSLabsUser** → **Sign out**
3. Choose **End Lab** → Confirm