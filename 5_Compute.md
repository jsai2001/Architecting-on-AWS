# Module 4: Compute Services

## Module Overview

This module provides an overview of AWS compute services. You'll explore various compute services and how each of them is relevant to different application use cases.

## Learning Objectives

In this module, you learn how to:
- List the AWS compute services
- Explain what to consider when deploying new and existing servers to Amazon Elastic Cloud Compute (Amazon EC2)
- Indicate which volume type to attach to an EC2 instance given a use case
- Explain Amazon EC2 pricing options
- Describe the benefits of AWS Lambda

## What is Compute?

Compute services provide CPU, memory, and disk resources to run code or applications on AWS infrastructure.

## Module Topics

### 1. Introduction to Compute Services
Overview of AWS compute offerings and their use cases

### 2. Amazon Elastic Compute Cloud (EC2)
- Virtual machine service within AWS Regions
- Instance types and configurations
- Deployment considerations for new and existing servers

### 3. EC2 Instance Storage
- Storage options available to EC2 instances
- Volume types and use case selection
- Performance characteristics

### 4. Amazon EC2 Pricing Options
- Payment models and pricing structures
- Cost optimization through commitment-based pricing
- Benefits of reserved capacity

### 5. AWS Lambda
- Serverless compute service
- Function-based code execution
- Support for multiple programming languages
- Event-driven architecture

### 6. Knowledge Check
Assessment of compute service concepts and implementation

# AWS Compute Services Evolution

## Overview

This lesson provides an overview of the evolution of compute services over time, from physical servers to modern serverless and containerized solutions.

---

## Evolution Timeline

### Pre-Cloud Era (Before 2006)
**Physical On-Premises Servers**
- Manual hardware provisioning and management
- Fixed capacity and long deployment cycles
- High upfront capital expenditure

### 2006: Virtualization Era
**Amazon EC2 Launch**
- Virtual machines with single API call provisioning
- On-demand compute capacity
- Pay-as-you-go pricing model
- Foundation of cloud computing

### 2014: Container and Serverless Era
**Amazon Elastic Container Service (ECS)**
- Containerization technology introduction
- Lighter form of virtualization (OS-level)
- Multiple containers per operating system
- Platform independence and consistent runtime

**AWS Lambda**
- Serverless compute service launch
- Function-as-a-Service (FaaS) model
- Event-driven code execution
- No server management required

### 2017: Serverless Containers
**AWS Fargate**
- Combination of serverless and containerization
- Serverless compute engine for containers
- Works with ECS and EKS orchestration
- Infrastructure managed by AWS

**Amazon EKS Launch**
- Managed Kubernetes service
- Container orchestration at scale
- Support for cloud-native applications

### 2020: Specialized Processors
**AWS Graviton Processors**
- ARM-based custom silicon
- Improved cost-per-performance ratio
- AI and ML workload optimization
- Available across EC2 and other compute services

---

## Core Compute Services

### Amazon EC2 (Elastic Compute Cloud)
**Overview**: Virtual servers in the cloud

**Key Features**:
- Wide variety of instance types
- Scalable compute capacity
- Multiple operating system support
- Integration with other AWS services

**Use Cases**:
- Web applications
- Development environments
- High-performance computing
- Enterprise applications

### Amazon EBS (Elastic Block Store)
**Overview**: Virtual disk storage for EC2 instances

**Key Features**:
- Persistent block storage
- Independent lifecycle from instances
- Multiple volume types for different performance needs
- Snapshot and backup capabilities

**Benefits**:
- Data persistence beyond instance lifecycle
- Flexible storage scaling
- High availability and durability

### AWS Lambda
**Overview**: Serverless compute service

**Key Features**:
- **Continuous scaling**: Automatic capacity management
- **Built-in fault tolerance**: High availability by design
- **Pay for value**: Charge only for execution time
- **Zero maintenance**: No server management required

**Use Cases**:
- Event processing
- API backends
- Data transformation
- Microservices architecture

---

## Containerization Benefits

### Platform Independence
- Consistent behavior across environments
- Reduced "works on my machine" issues

### Resource Efficiency
- Higher resource utilization than VMs
- Lightweight compared to full virtualization

### Deployment Advantages
- Faster deployment cycles (seconds vs. minutes)
- Easier rollbacks and updates
- Immutable infrastructure patterns

### Isolation and Security
- Application sandboxing
- Process and resource isolation
- Consistent security boundaries

---

## Service Comparison

| Service | Launch Year | Type | Management Level | Use Case |
|---------|-------------|------|------------------|----------|
| **EC2** | 2006 | Virtual Machines | Infrastructure | Full control applications |
| **ECS** | 2014 | Container Orchestration | Platform | Containerized applications |
| **Lambda** | 2014 | Serverless Functions | Function | Event-driven processing |
| **EKS** | 2017 | Kubernetes | Platform | Cloud-native applications |
| **Fargate** | 2017 | Serverless Containers | Platform | Containerized serverless |

---

## Key Innovations

### Serverless Computing Benefits
- **No server management**: Focus on code, not infrastructure
- **Automatic scaling**: Handle traffic spikes seamlessly
- **Cost optimization**: Pay only for actual usage
- **Built-in reliability**: Fault tolerance included

### Containerization Advantages
- **Portability**: Run anywhere containers are supported
- **Efficiency**: Better resource utilization
- **Speed**: Rapid deployment and scaling
- **Consistency**: Same behavior across environments

### Graviton Processors
- **Performance**: Optimized for cloud workloads
- **Cost**: Better price-performance ratio
- **Efficiency**: Lower power consumption
- **AI/ML**: Specialized for modern workloads

---

## Module Focus Areas

This module will cover:
1. **Amazon EC2**: Virtual machine fundamentals
2. **Amazon EBS**: Storage options and configurations
3. **AWS Lambda**: Serverless compute implementation

---

## Next Steps

Now that you understand the evolution of compute services, let's learn more about Amazon EC2 instances in the following lesson.

# Amazon EC2 Instances

## Overview

Elastic cloud computing provides scalable resources, free from the constraints of fixed IT infrastructure. It redefines approaches to change management, testing, reliability, and capacity planning.

---

## EC2 Launch Considerations

### 1. Name and Tags
**Purpose**: Instance identification and resource management

**Tags Overview**:
- Metadata labels with customer-defined keys and optional values
- Filter and organize resources
- Manage access control and track costs
- Automate tasks and maintain organization

**Common Tag Categories**:
- Purpose (web server, database, etc.)
- Owner (team or individual)
- Environment (dev, test, prod)
- Cost center or project

### 2. Application and OS Image
**Consideration**: What application and operating system will run on the instance?

### 3. Instance Type and Size
**Consideration**: Technical requirements for CPU, memory, storage, and network performance

### 4. Key Pair
**Consideration**: Authentication and connection methods for secure access

### 5. Network and Security
**Consideration**: VPC, subnet, and security group configuration

### 6. Configure Storage
**Consideration**: Block storage type selection based on use case requirements

### 7. Placement and Tenancy
**Consideration**: Physical placement and isolation requirements

### 8. Scripts and Metadata
**Consideration**: Launch automation through user data and metadata

---

## Amazon Machine Image (AMI)

### Definition
An AMI provides the information required to launch an instance (virtual server in the cloud). Multiple instances can be launched from a single AMI for identical configurations.

### AMI Components
- **Root volume template**: OS, application server, and applications
- **Launch permissions**: Controls which AWS accounts can use the AMI
- **Block device mapping**: Specifies volumes to attach at launch

### AMI Sources

| Source | Description | Use Case |
|--------|-------------|----------|
| **AWS-provided AMIs** | Pre-built images from AWS | Quick deployment, standard configurations |
| **AWS Marketplace** | Thousands of third-party solutions | Specialized software, vendor solutions |
| **Custom AMIs** | Self-created images | Specific configurations, organizational standards |

### Custom AMI Creation
1. Launch and customize an instance
2. Save configuration as custom AMI
3. Launch new instances with customizations
4. Publish for internal, private, or public use

---

## Instance Type Naming Convention

### Format: `[Family][Generation][Properties].[Size]`

**Example**: `c5g.large`

| Component | Example | Description |
|-----------|---------|-------------|
| **Family** | `c` | Instance family (compute optimized) |
| **Generation** | `5` | Hardware generation number |
| **Properties** | `g` | Additional features (Graviton2 processor) |
| **Size** | `large` | CPU, memory, storage, network performance |

---

## EC2 Instance Families

### General Purpose
- **Characteristics**: Balanced compute, memory, and networking
- **Use Cases**: Diverse workloads, web applications
- **Examples**: t3, m5, a1

### Compute Optimized
- **Characteristics**: High-performance processors
- **Use Cases**: Compute-bound applications, media transcoding, scientific modeling, machine learning
- **Examples**: c5, c6g

### Memory Optimized
- **Characteristics**: Fast delivery of large datasets in memory
- **Use Cases**: Database servers, web caches, data analytics
- **Examples**: r5, x1e, z1d

### Accelerated Computing
- **Characteristics**: GPU and FPGA acceleration
- **Use Cases**: High-graphics processing, machine learning, HPC, autonomous vehicles
- **Examples**: p3, g4, f1

### Storage Optimized
- **Characteristics**: High sequential read/write performance
- **Use Cases**: Large datasets, NoSQL databases, Amazon OpenSearch Service
- **Examples**: i3, d2, h1

---

## AWS Compute Optimizer

### Purpose
Machine learning-based service that analyzes resource configuration and CloudWatch usage data to provide optimization recommendations.

### Benefits
- **Cost reduction**: Right-size instances for actual usage
- **Performance improvement**: Optimize for workload requirements
- **Integration**: Export to S3, integrate with Cost Explorer and Systems Manager

### Recommendations Include
- Instance type optimization
- CPU and memory utilization analysis
- Performance risk assessment

---

## EC2 Key Pairs

### Definition
Security credentials consisting of private and public keys for instance authentication.

### Key Management
- **Public key**: Stored by Amazon EC2
- **Private key**: Stored securely by user
- **Authentication**: Private key replaces password for secure access

### Security Best Practices
- Store private keys securely
- Limit access to private key files
- Use different key pairs for different environments

---

## Tenancy Options

### Shared Tenancy (Default)
- **Description**: Multiple AWS accounts share physical hardware
- **Cost**: Most cost-effective option
- **Use Case**: Standard workloads without compliance requirements

### Dedicated Instance
- **Description**: Physically isolated at host hardware level
- **Isolation**: Separated from other AWS accounts
- **Use Case**: Compliance requirements, regulatory needs

### Dedicated Host
- **Description**: Physical server with full EC2 capacity dedicated to your use
- **Control**: Full server configuration control
- **Options**: AWS automatic placement or manual server selection
- **Use Case**: Licensing requirements, compliance, performance consistency

---

## Placement Groups

### Purpose
Influence instance placement on underlying hardware to meet workload requirements.

### Types

#### Cluster Placement Groups
- **Benefit**: Low network latency, high network throughput
- **Use Case**: HPC workloads, tightly coupled applications
- **Limitation**: Single Availability Zone

#### Spread Placement Groups
- **Benefit**: Maximum fault tolerance
- **Use Case**: Critical instances requiring separation
- **Example**: Medical health record systems
- **Limitation**: Maximum 7 instances per AZ

#### Partition Placement Groups
- **Benefit**: Large distributed workloads
- **Use Case**: Distributed and replicated applications
- **Protection**: Avoid correlated hardware failures

---

## User Data

### Purpose
Automate instance configuration during launch process.

### Capabilities
- Patch and update instance AMI
- Install software and license keys
- Configure applications and services
- Set up monitoring and logging

### Implementation
- **Linux**: Executed by cloud-init
- **Windows**: Executed by EC2Launch service
- **Privileges**: Runs with root/administrator privileges
- **Timing**: Executes after launch, before network accessibility

### Format Options
- Shell scripts (Linux)
- PowerShell scripts (Windows)
- Cloud-init directives

---

## Key Considerations Summary

1. **Instance Selection**: Choose appropriate family and size for workload
2. **AMI Strategy**: Use AWS, Marketplace, or custom images
3. **Security**: Implement proper key pair management
4. **Placement**: Consider tenancy and placement group requirements
5. **Automation**: Leverage user data for consistent deployments
6. **Optimization**: Use Compute Optimizer for ongoing efficiency

---

## Next Steps

In the following lesson, you learn how to configure storage for EC2 instances.

# EC2 Storage Options

## Overview

This lesson discusses storage configuration for EC2 instances, covering different types of storage volumes and their use cases.

---

## Storage Types Overview

### Amazon Elastic Block Store (EBS)
- **Primary storage**: Boot volumes and additional persistent storage
- **Durability**: Data persists independently of instance lifecycle
- **Flexibility**: Attach, detach, and reattach to different instances
- **Replication**: Automatic replication within Availability Zone

### Instance Store
- **Temporary storage**: Physically attached to host computer
- **Performance**: High-performance local storage
- **Persistence**: Data lost when instance stops, hibernates, or terminates
- **Cost**: Included in instance usage cost

---

## Amazon EBS Volumes

### Key Characteristics
- **Durable**: Block-level storage that persists beyond instance lifecycle
- **Detachable**: Can be moved between instances in same AZ
- **Low latency**: Extremely fast access for database workloads
- **Flexible**: Dynamic resizing, IOPS modification, and type changes

### EBS Volume Management
- **Multiple volumes**: Attach multiple EBS volumes to single instance
- **Same AZ requirement**: Volume and instance must be in same Availability Zone
- **Multi-Attach**: Mount single volume to multiple instances (specific types)
- **Snapshots**: Point-in-time copies stored in Amazon S3

### EBS Volume Lifecycle
- **Primary boot volume**: Created automatically at instance launch
- **Secondary volumes**: Created independently with controllable lifecycle
- **Persistence options**: Same as instance or long-lived independent storage

---

## EBS Volume Types

### SSD-Backed Volumes
**Optimized for**: Transactional workloads with frequent read/write operations

#### General Purpose SSD (gp2, gp3)
- **Use Cases**: Boot volumes, small-medium databases, dev/test environments
- **Performance**: Cost-effective storage for broad range of applications
- **IOPS**: Baseline performance with burst capability

#### Provisioned IOPS SSD (io1, io2)
- **Use Cases**: I/O intensive workloads, performance-sensitive databases
- **Performance**: Consistent IOPS rate (99.9% of the time)
- **Control**: Specify exact IOPS requirements at creation

### HDD-Backed Volumes
**Optimized for**: Large streaming workloads where throughput is key

#### Throughput Optimized HDD (st1)
- **Use Cases**: Large sequential workloads (EMR, ETL, data warehouses, log processing)
- **Performance**: Defines performance by throughput rather than IOPS
- **Cost**: Low-cost magnetic storage for frequently accessed data

#### Cold HDD (sc1)
- **Use Cases**: Large sequential cold-data workloads, infrequent access
- **Performance**: Throughput-based performance model
- **Cost**: Lowest cost HDD volume for archived data

---

## Volume Type Comparison

| Volume Type | Storage Media | Use Case | Performance Metric | Cost |
|-------------|---------------|----------|-------------------|------|
| **gp3/gp2** | SSD | General purpose, boot volumes | Balanced IOPS | Moderate |
| **io2/io1** | SSD | High-performance databases | Provisioned IOPS | High |
| **st1** | HDD | Big data, data warehouses | Throughput | Low |
| **sc1** | HDD | Cold storage, archives | Throughput | Lowest |

---

## Instance Store Volumes

### Characteristics
- **Location**: Physically attached to host computer
- **Performance**: High-performance local storage
- **Temporary**: Non-persistent storage
- **Lifecycle**: Tied to instance virtualization lifecycle

### Use Cases
- **Caching**: High-speed temporary data storage
- **Buffers**: Application temporary storage needs
- **Scratch data**: Processing temporary files
- **Replicated data**: Data replicated across instance fleet (web servers)

### Limitations
- **Data loss**: All data lost when instance stops/terminates
- **No detachment**: Cannot move to another instance
- **Launch-time specification**: Must specify at instance launch
- **Manual setup**: Requires formatting and mounting before use

### Instance Store Considerations
- **Size**: Determined by instance type
- **Hardware**: Type varies by instance family
- **Availability**: Some instance types include by default (NVMe)
- **Cost**: Included in instance pricing

---

## Storage Decision Matrix

### Choose EBS When:
- Data persistence required beyond instance lifecycle
- Need to move storage between instances
- Database storage requirements
- Backup and snapshot capabilities needed
- Flexible performance tuning required

### Choose Instance Store When:
- Highest performance local storage needed
- Temporary data processing
- Caching and buffering applications
- Cost optimization for temporary storage
- Data is replicated or easily recreated

---

## EBS Best Practices

### Performance Optimization
- **Right-sizing**: Choose appropriate volume type for workload
- **IOPS provisioning**: Use io1/io2 for consistent high performance
- **Multi-volume**: Distribute I/O across multiple volumes
- **Placement**: Keep volumes in same AZ as instances

### Data Protection
- **Regular snapshots**: Create point-in-time backups
- **Cross-region copying**: Replicate snapshots for disaster recovery
- **Encryption**: Enable encryption for sensitive data
- **Monitoring**: Track performance metrics and utilization

### Cost Management
- **Volume type selection**: Match performance needs to cost
- **Snapshot lifecycle**: Automate old snapshot deletion
- **Unused volumes**: Identify and remove unattached volumes
- **Right-sizing**: Monitor and adjust volume sizes

---

## Storage Architecture Patterns

### Database Storage
- **Primary**: Provisioned IOPS SSD (io1/io2) for database files
- **Logs**: General Purpose SSD (gp3) for transaction logs
- **Backups**: Cold HDD (sc1) for backup storage

### Web Application
- **Boot**: General Purpose SSD (gp3) for OS and application
- **Cache**: Instance store for temporary caching
- **Static content**: Throughput Optimized HDD (st1) for large files

### Analytics Workload
- **Processing**: Instance store for temporary processing
- **Input data**: Throughput Optimized HDD (st1) for large datasets
- **Results**: General Purpose SSD (gp3) for output storage

---

## Key Takeaways

1. **EBS provides persistent**, detachable block storage
2. **Instance store offers high-performance** temporary storage
3. **Volume type selection** impacts both performance and cost
4. **SSD volumes** optimize for IOPS, **HDD volumes** for throughput
5. **Storage architecture** should match workload requirements

---

## Next Steps

Pricing options for these services are covered next.

# Amazon EC2 Pricing Options

## Overview

EC2 offers multiple pricing models to optimize costs for different workload patterns and commitment levels. Understanding these options helps balance commitment, flexibility, and cost.

---

## Pricing Models Overview

### On-Demand Instances
- **Payment**: Per second or hour usage
- **Commitment**: No long-term commitments
- **Use Case**: Variable workloads, testing, short-term projects

### Savings Plans
- **Commitment**: 1 or 3-year terms
- **Payment Options**: All Upfront, Partial Upfront, No Upfront
- **Discount**: Significant savings over On-Demand pricing

### Spot Instances
- **Pricing**: Fluctuating market-based pricing
- **Savings**: Up to 90% off On-Demand prices
- **Availability**: Uses spare EC2 capacity
- **Risk**: Can be interrupted with 2-minute notice

---

## Savings Plans

### Compute Savings Plans
**Coverage**: EC2, AWS Fargate, and AWS Lambda

**Flexibility**:
- Instance families (any family)
- Instance sizes (any size)
- Tenancy types (shared, dedicated)
- Availability Zones (any AZ)
- Regions (any region)
- Operating systems (any OS)

**Commitment**: Dollar amount per hour (e.g., $10/hour)

### EC2 Instance Savings Plans
**Coverage**: EC2 instances only

**Flexibility**:
- Instance sizes within selected family
- Operating systems
- Tenancy types
- Availability Zones within specified region

**Restrictions**:
- Specific instance family (e.g., M5 family)
- Specific region

**Savings**: Up to 70% off On-Demand rates

---

## Spot Instances

### Key Characteristics
- **Infrastructure**: Same as On-Demand instances
- **Capacity**: Uses spare EC2 capacity
- **Pricing**: Market-driven, fluctuating prices
- **Interruption**: 2-minute warning before termination

### Interruption Scenarios
1. **Price exceeds bid**: When spot price rises above your maximum price
2. **Capacity unavailable**: When AWS needs capacity back
3. **Notification**: 2-minute advance warning provided

### Spot Fleet Features
- **Diversification**: Multiple instance types and AZs
- **Base capacity**: Maintain minimum On-Demand instances
- **Automatic replacement**: Configure replacement strategies
- **Risk mitigation**: Spread across different instance types and AZs

---

## Spot Instance Use Cases

### Image and Media Rendering
- **Benefit**: Scale rendering workloads cost-effectively
- **Flexibility**: Add capacity when spot prices are favorable
- **Interruption tolerance**: Rendering jobs can be restarted

### Big Data and Analytics
- **Benefit**: Accelerate data processing with additional nodes
- **Scalability**: Add cluster capacity when prices drop
- **Fault tolerance**: Distributed processing handles node loss

### Web Services
- **Architecture**: Spot Fleet behind load balancer
- **Scalability**: Scale to thousands of instances
- **Stateless**: Web servers can be easily replaced
- **Base capacity**: Maintain On-Demand instances for stability

---

## Pricing Comparison

| Pricing Model | Commitment | Flexibility | Savings | Interruption Risk |
|---------------|------------|-------------|---------|-------------------|
| **On-Demand** | None | High | None | None |
| **Compute Savings Plan** | 1-3 years | High | Moderate | None |
| **EC2 Instance Savings Plan** | 1-3 years | Medium | High (70%) | None |
| **Spot Instances** | None | Medium | Highest (90%) | Yes |

---

## Combined Pricing Strategy

### Layered Approach
```
Steady Baseline → Savings Plans
Variable Load → On-Demand
Flexible Workloads → Spot Instances
Peak Spikes → On-Demand
```

### Example Architecture
1. **Base Layer**: EC2 Instance Savings Plan for predictable $8/hour usage
2. **Variable Layer**: Compute Savings Plan for additional capacity
3. **Burst Layer**: Spot Instances when prices are favorable
4. **Peak Layer**: On-Demand for unpredictable spikes

---

## Payment Options for Savings Plans

### All Upfront
- **Payment**: 100% upfront payment
- **Discount**: Highest discount rate
- **Cash flow**: Large initial payment, no monthly charges

### Partial Upfront
- **Payment**: Portion upfront + monthly payments
- **Discount**: Medium discount rate
- **Cash flow**: Balanced approach

### No Upfront
- **Payment**: Monthly payments only
- **Discount**: Lowest discount rate
- **Cash flow**: No initial payment required

---

## Workload Suitability

### Ideal for Spot Instances
- **Fault-tolerant**: Can handle interruptions gracefully
- **Flexible**: No strict timing requirements
- **Loosely coupled**: Components can operate independently
- **Stateless**: No persistent state on instances

### Not Suitable for Spot Instances
- **Critical applications**: Cannot tolerate interruptions
- **Stateful workloads**: Maintain important state
- **Time-sensitive**: Strict completion deadlines
- **Single-instance**: No redundancy or failover

---

## Cost Optimization Strategies

### Right-Sizing
- Monitor utilization metrics
- Match instance types to workload requirements
- Use AWS Compute Optimizer recommendations

### Mixed Instance Types
- Combine different pricing models
- Use Spot Instances for flexible workloads
- Maintain base capacity with Savings Plans

### Regional Considerations
- Compare pricing across regions
- Consider data transfer costs
- Evaluate compliance requirements

### Monitoring and Automation
- Set up billing alerts
- Use AWS Cost Explorer for analysis
- Implement auto-scaling policies
- Monitor Spot Instance interruption rates

---

## Best Practices

### Savings Plans
1. **Analyze usage patterns** before committing
2. **Start with Compute Savings Plans** for maximum flexibility
3. **Consider payment options** based on cash flow
4. **Monitor utilization** to ensure value

### Spot Instances
1. **Design for interruption** from the start
2. **Use diverse instance types** and AZs
3. **Implement graceful shutdown** procedures
4. **Monitor spot price trends** for optimization

### Combined Strategy
1. **Layer pricing models** based on workload characteristics
2. **Use Reserved Instances** for predictable baseline
3. **Add Spot capacity** for flexible workloads
4. **Keep On-Demand** for unpredictable spikes

---

## Key Takeaways

1. **Multiple pricing models** available for different use cases
2. **Savings Plans provide flexibility** with commitment discounts
3. **Spot Instances offer maximum savings** for fault-tolerant workloads
4. **Combined strategies** optimize both cost and reliability
5. **Workload characteristics** determine optimal pricing model

---

## Next Steps

The next lesson covers serverless compute services provided by AWS.

# AWS Lambda

## Overview

AWS Lambda is a serverless, event-driven compute service that lets you run code for virtually any type of application or backend service without provisioning or managing servers. With serverless computing, you can build and run applications without thinking about server infrastructure.

---

## Serverless Computing Concept

### Traditional Computing vs Serverless

| Traditional Computing | Serverless Computing |
|----------------------|---------------------|
| Manage virtual servers | No server management |
| Provision infrastructure | Higher level of abstraction |
| Scale manually | Automatic scaling |
| Pay for server uptime | Pay only for execution time |

### Key Benefits
- **No server management**: Focus on code, not infrastructure
- **Automatic scaling**: From few requests per day to thousands per second
- **Pay-per-use**: Only charged for compute time consumed
- **High availability**: Built-in fault tolerance and redundancy

---

## AWS Lambda Features

### Supported Languages
- **Node.js**: JavaScript runtime
- **Java**: JVM-based applications
- **Python**: Popular for data processing and automation
- **C#**: .NET Core applications
- **Go**: High-performance applications
- **PowerShell**: Windows automation and scripting
- **Ruby**: Web applications and scripting

### Custom Runtimes
- **Custom runtime API**: Support for any programming language
- **Container images**: Supply custom container for function execution
- **Flexibility**: Use languages not natively supported

### Function Execution
- **Event-driven**: Triggered by events from AWS services or applications
- **API invocation**: Direct function calls via AWS APIs
- **Automatic scaling**: Concurrent executions based on demand

---

## Event Sources

### AWS Service Integration

#### Amazon S3
- **Trigger**: Object creation, deletion, or modification
- **Use Case**: Process uploaded files, generate thumbnails, data transformation

#### Amazon EventBridge
- **Trigger**: AWS service events (EC2 launches, security group changes)
- **Use Case**: Infrastructure monitoring, compliance automation

#### Amazon Kinesis
- **Trigger**: Stream data processing
- **Use Case**: Real-time analytics, log processing

#### Amazon Cognito
- **Trigger**: User authentication events
- **Use Case**: Custom user registration flows, password validation

### External Sources
- **API Gateway**: HTTP requests to Lambda functions
- **Application Load Balancer**: Direct HTTP integration
- **CloudWatch Events**: Scheduled executions
- **Custom applications**: Direct SDK invocation

---

## Lambda Use Cases

### Web Applications
- **Static websites**: Dynamic content generation
- **Complex web applications**: Full-stack serverless applications
- **Framework support**: Flask (Python), Express (Node.js)
- **API backends**: RESTful services and microservices

### Backend Services
- **Mobile applications**: Backend logic and data processing
- **IoT applications**: Device data processing and response
- **Microservices**: Distributed application components
- **API integrations**: Third-party service connections

### Data Processing
- **Real-time processing**: Stream data analysis
- **Batch processing**: Large dataset transformation
- **MapReduce operations**: Distributed data processing
- **AWS Batch integration**: Parallel job execution

### Chatbots and Voice
- **Amazon Lex**: Chatbot logic implementation
- **Amazon Alexa**: Voice-activated applications
- **Alexa Skills Kit**: Custom voice interactions
- **Natural language processing**: Text and speech analysis

### IT Automation
- **Policy engines**: Compliance and governance automation
- **AWS service extensions**: Custom functionality for AWS services
- **Infrastructure management**: Resource provisioning and monitoring
- **Event-driven workflows**: Automated response to system events

---

## Lambda Architecture Patterns

### Event-Driven Processing
```
Event Source → Lambda Function → Action/Response
```

### API Backend Pattern
```
Client → API Gateway → Lambda Function → Database/Service
```

### Data Pipeline Pattern
```
Data Source → Lambda (Transform) → Lambda (Process) → Data Store
```

### Microservices Pattern
```
Multiple Lambda Functions ↔ API Gateway ↔ Client Applications
```

---

## Core Components

### Lambda Function
- **Custom code**: Your application logic
- **Event processing**: Handles incoming events
- **Stateless**: No persistent state between invocations
- **Timeout**: Maximum execution time (15 minutes)

### Event Source
- **Event publisher**: AWS services or external applications
- **Event data**: Information passed to Lambda function
- **Trigger configuration**: Rules for when to invoke function

### Execution Environment
- **Runtime**: Language-specific execution environment
- **Memory allocation**: 128 MB to 10,240 MB
- **Temporary storage**: /tmp directory (512 MB to 10,240 MB)
- **Network access**: VPC integration available

---

## Lambda Benefits

### Operational Benefits
- **No server management**: AWS handles infrastructure
- **Automatic scaling**: Handles traffic spikes automatically
- **Built-in monitoring**: CloudWatch integration
- **Security**: IAM integration and VPC support

### Cost Benefits
- **Pay-per-request**: No charges when not running
- **No idle costs**: Only pay for execution time
- **Free tier**: 1 million requests per month
- **Cost-effective**: Especially for variable workloads

### Development Benefits
- **Fast deployment**: Quick code updates
- **Multiple versions**: Support for function versioning
- **Environment variables**: Configuration management
- **Dead letter queues**: Error handling and retry logic

---

## Integration Ecosystem

### AWS Services Integration
- **Storage**: S3, EFS, DynamoDB
- **Databases**: RDS, Aurora, DocumentDB
- **Analytics**: Kinesis, EMR, Athena
- **Machine Learning**: SageMaker, Rekognition, Comprehend
- **Messaging**: SQS, SNS, EventBridge

### Development Tools
- **AWS SAM**: Serverless Application Model
- **CloudFormation**: Infrastructure as Code
- **CodePipeline**: CI/CD integration
- **X-Ray**: Distributed tracing

---

## Best Practices

### Function Design
- **Single responsibility**: One function, one purpose
- **Stateless design**: No persistent state in function
- **Idempotent operations**: Safe to retry
- **Error handling**: Proper exception management

### Performance Optimization
- **Memory allocation**: Right-size for workload
- **Cold start mitigation**: Keep functions warm
- **Connection reuse**: Reuse database connections
- **Efficient code**: Optimize for execution time

### Security
- **Least privilege**: Minimal IAM permissions
- **Environment variables**: Secure configuration storage
- **VPC integration**: Network isolation when needed
- **Encryption**: Data encryption in transit and at rest

---

## Key Takeaways

1. **Serverless computing** eliminates server management overhead
2. **Event-driven architecture** enables reactive applications
3. **Automatic scaling** handles variable workloads efficiently
4. **Pay-per-use model** optimizes costs for sporadic workloads
5. **Rich integration** with AWS services and external systems

---

## Next Steps

The next lesson features a tech talk discussion on launching an EC2 instance.

# EC2 Instance Launch Demonstration

## Overview

This demonstration walks through launching an EC2 instance, covering key configuration options and practical implementation of concepts discussed in the module.

---

## Instance Configuration Steps

### 1. Instance Naming and Tags
- **Instance Name**: `raf-server`
- **Purpose**: Identification and resource management

### 2. AMI Selection
**Available Options**:
- Amazon Linux (selected)
- macOS
- Ubuntu  
- Windows
- Red Hat Enterprise Linux

**Architecture Choice**: ARM (Graviton processor)
- Alternative: x86_64 architecture
- Benefits: Cost-performance advantages with Graviton processors

### 3. Instance Type Selection
**Selected**: `a1.medium`
- **Family**: a1 (ARM-based)
- **vCPUs**: Displayed in console
- **Memory**: Shown with instance specifications
- **Use Case**: Simple web server (minimal requirements)

### 4. Key Pair Configuration
**Decision**: Proceed without key pair
- **Reason**: No remote administration planned
- **Alternative**: SSH key pair for Linux administration

### 5. Network Settings
**VPC Configuration**:
- VPC selection
- Subnet assignment
- Public IP address allocation

**Security Group Creation**:
- **Name**: `http-only`
- **Description**: HTTP-only security group
- **Inbound Rule**: 
  - Protocol: HTTP (port 80)
  - Source: Anywhere (0.0.0.0/0)

### 6. Storage Configuration
- **Type**: EBS volume (default)
- **Attachment**: Primary storage for instance

### 7. Advanced Configuration - User Data
**Purpose**: Automate instance setup during launch

**Script Content**:
```bash
#!/bin/bash
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Hello Raf from my EC2 instance" > /var/www/html/index.html
```

**Script Actions**:
1. Install Apache Web Server (httpd)
2. Start the web service
3. Enable service for automatic startup
4. Create custom HTML file

---

## User Data Implementation

### What is User Data?
- **Definition**: Bootstrap script executed during instance launch
- **Execution**: Runs with root/administrator privileges
- **Service**: Executed by cloud-init (Linux) or EC2Launch (Windows)
- **Timing**: Runs after instance launch, before network accessibility

### How User Data Works
1. Instance boots up
2. Cloud-init service queries instance metadata
3. Retrieves user data script
4. Executes script with appropriate privileges
5. Instance becomes available with configured services

### Use Cases
- Software installation and configuration
- Service startup automation
- Custom application deployment
- System configuration and tuning

---

## ARM Architecture and Graviton Processors

### ARM vs x86 Architecture
- **ARM**: Reduced Instruction Set Computing (RISC)
- **x86**: Complex Instruction Set Computing (CISC)
- **Graviton**: AWS custom ARM-based processors

### Graviton Benefits
- **Cost-performance**: Better price-to-performance ratio
- **Energy efficiency**: Lower power consumption
- **AWS optimization**: Designed specifically for cloud workloads

### Graviton Availability
- **EC2**: Multiple instance families (A1, M6g, C6g, R6g, etc.)
- **RDS**: Database instances
- **Lambda**: Serverless functions
- **Other services**: Expanding availability

### Software Compatibility
- **Requirement**: Applications must be compiled for ARM architecture
- **Operating Systems**: Linux distributions with ARM support
- **Performance**: Optimized for ARM instruction set

---

## Security Group Configuration

### HTTP-Only Security Group
**Inbound Rules**:
- **Protocol**: TCP
- **Port**: 80 (HTTP)
- **Source**: 0.0.0.0/0 (anywhere)

**Security Considerations**:
- Allows public web traffic
- No SSH access configured
- Minimal attack surface for web server

### Best Practices
- **Principle of least privilege**: Only necessary ports open
- **Source restrictions**: Limit access where possible
- **Regular reviews**: Audit security group rules periodically

---

## Instance Launch Results

### Successful Deployment
- **Public IP**: 54.186.14.131
- **Web Service**: Apache HTTP Server running
- **Content**: Custom HTML page displaying "Hello Raf from my EC2 instance"
- **Access**: Publicly accessible via HTTP

### Verification Steps
1. Instance launched successfully
2. User data script executed
3. Apache web server installed and started
4. Custom HTML file created
5. Web service accessible from internet

---

## Key Learning Points

### Instance Launch Process
1. **Configuration choices** impact functionality and cost
2. **User data** enables automated setup and configuration
3. **Security groups** control network access
4. **Architecture selection** affects performance and compatibility

### ARM/Graviton Advantages
- **Cost optimization** through better price-performance
- **AWS-optimized** hardware for cloud workloads
- **Growing ecosystem** support across AWS services
- **Software compatibility** requires ARM-compiled applications

### Automation Benefits
- **Consistent deployments** through user data scripts
- **Reduced manual configuration** and human error
- **Faster time to service** with automated setup
- **Repeatable processes** for infrastructure as code

---

## Next Steps

This demonstration covered practical EC2 instance deployment. The module continues with:
- Knowledge Check assessment
- Hands-on lab exercise
- Storage implementation in applications (next module)

---

## Summary

The demonstration successfully showed:
- Complete EC2 instance launch process
- User data automation implementation
- ARM/Graviton processor benefits
- Security group configuration
- Web server deployment and verification