# Module 5: Storage Services

## Module Overview

This module introduces you to various storage services offered by AWS. You learn about specific storage scenarios that can help you identify which storage service best suits your application requirement.

## Learning Objectives

In this module, you learn how to:
- Compare AWS storage products and services based on business scenarios
- Choose the right Amazon Simple Storage Service (Amazon S3) object storage solution given a use case
- Choose the right shared file system storage solution given a use case
- Identify the AWS Data migration tools

## Module Introduction

Welcome to module number five on Architecting on AWS. In this module, I will talk about storage, starting by approaching some storage services, then transitioning to Amazon S3 or Simple Storage Service. I will also talk about some shared file systems that you can have in AWS, then some data migration tools, and last but not least, a knowledge check.

## Module Topics

### 1. Storage Services Overview
Introduction to AWS storage offerings and their use cases

### 2. Amazon S3 (Simple Storage Service)
- Object storage fundamentals
- Storage classes and lifecycle management
- Use case scenarios and best practices

### 3. Shared File Systems
- Amazon EFS (Elastic File System)
- Amazon FSx options
- Use cases for shared storage

### 4. Data Migration Tools
- AWS migration services and tools
- Migration strategies and best practices
- Choosing the right migration approach

### 5. Knowledge Check
Assessment of storage concepts and service selection

# Storage Services Overview

## Introduction

What are some services to consider when looking at block, file and object storage? Let's take a look.

Starting with the storage services options in AWS. If you're asking, what are some services to consider when looking at block, file, and object storage, you came to the right place because this is the module where I am going to talk about these technologies in AWS. But before start talking about AWS services, let me differ block storage, file storage, and object storage.

---

## Storage Types Explained

### Block Storage

So block storage is basically when you have a bag full of bits. You get these bits, also known as raw storage, and you attach somewhere, such as a storage area network, a SAN, or a storage arrays. In AWS, they are EBS volumes. So block storage, when you think about block storage, think about a bag full of bits where you can attach to something and then you can mount your own file system on that. Because it is more raw, it allows you to have block level granularity in terms of editing. So you can edit these blocks. So if you want to edit a 5-megabyte (MB) text file, and this 5-MB text file is in a block storage, you can only change the blocks that corresponds to the edits you want to do, because you have access to these blocks.

**Key Characteristics:**
- Raw storage as "bag full of bits"
- Attaches to systems like SAN or storage arrays
- AWS service: Amazon EBS (Elastic Block Store)
- Block-level granularity for editing
- Can mount your own file system
- Efficient editing - only change specific blocks needed

**Use Cases:**
- Operating systems
- Databases requiring low-latency access
- Applications needing frequent updates
- Scenarios requiring direct block manipulation

### Object Storage

In the other side of the spectrum, we have object storage which basically stores virtual containers that encapsulate the data and it abstracts the data for you. Some examples are OpenStack Swift, Ceph, and in Amazon, as you will see, Amazon S3. Object storage, you basically correct with object storage by doing APIs. In our case, PUT object, GET object. There is no data editing capability out of an object storage.

**Key Characteristics:**
- Virtual containers that encapsulate data
- Data abstraction layer
- API-based interaction (PUT object, GET object)
- No direct data editing capability
- Examples: OpenStack Swift, Ceph, Amazon S3

**Use Cases:**
- Large-scale unstructured data
- Media files and backups
- Static web content
- Applications requiring excellent scalability and durability

### File Storage

Now file storage stays in the middle. And file storage gives you more block level granularity, but it's not a block storage level per se, because you don't have access to the raw storage. You have access to a file system. So services that work with file storage will already give you a managed file system, such as NAS or Network Attached Storage, appliances, or Windows file servers. In our case we have Amazon EFS, or Elastic File System which is a file storage.

**Key Characteristics:**
- Middle ground between block and object storage
- More granularity than object, less than block
- No access to raw storage - access to file system
- Managed file system provided
- Examples: NAS, Windows file servers, Amazon EFS
- Hierarchical folder structure

**Use Cases:**
- Content management systems
- Web serving applications
- Development environments
- Scenarios requiring traditional file system access
- Shared access across multiple instances or applications

---

## AWS Storage Services Comparison

### Block Storage Services
- **Amazon EBS (Elastic Block Store)**: As discussed by Russ in the previous module
- High-performance block storage for EC2 instances
- Multiple volume types for different performance needs

### Object Storage Services
- **Amazon S3 (Simple Storage Service)**: Primary object storage service
- **Amazon S3 Glacier**: Long-term archival and backup
- Scalable, durable, and highly available object storage

### File Storage Services
- **Amazon EFS (Elastic File System)**: Managed NFS file system
- Shared file storage across multiple EC2 instances
- Fully managed and scalable

---

## Cloud Storage Overview Summary

Block, object, and file storage represent three distinct approaches to data storage, each suited for different use cases.

**Block storage**, like AWS EBS, divides data into fixed-size blocks and is ideal for operating systems and databases requiring low-latency access and frequent updates, as it allows direct manipulation of individual blocks.

**Object storage**, such as AWS S3, treats data as discrete objects with metadata and unique identifiers, making it perfect for large-scale unstructured data like media files, backups, and static web content, offering excellent scalability and durability.

**File storage**, exemplified by AWS EFS, organizes data in a hierarchical folder structure familiar to most users, providing shared access to files across multiple instances or applications, making it particularly suitable for content management systems, web serving, and development environments where traditional file system access is needed.

---

## Module Focus

In this slide, there are some block storage, file storage, and object storage, AWS services you can use. Russ already talked about block storage on the last module with Amazon EBS, or Elastic Block Store. In this module, I am going to talk about Amazon S3, Amazon S3 Glacier on the object storage category, and Amazon EFS on the file storage.

---

## Next Steps

Now that you've identified what block, file, and object storage are, the next lesson dives into Amazon S3.

# Amazon Simple Storage Service (Amazon S3)

## Introduction

Amazon S3 accelerates innovation, increases agility, reduces cost, and strengthens security. And you're going to understand why after watching the next few slides.

---

## Amazon S3 Use Cases

Here are some use cases for Amazon S3. You can use as a backup and restore service by sending your files to Amazon S3. They are replicated and Amazon S3 is designed to provide high data durability. So you can send your data to Amazon S3 and then you can restore the data from Amazon S3 in case something occurs with your data. Amazon S3 is also very well known for being the primary storage for data lakes, for big data operations and analytics. It can also be used to serve static websites, that is a feature that can host your HTML files straight out of Amazon S3. Also useful for archiving and compliance and media storage and streaming.

Basically, you can imagine an S3 bucket as a place to securely store your data in a replicated fashion. It has a large number of users accessing your content, growing datasets. And if you have dealing with a data that has a pattern of write once, read many, Amazon S3 certainly can be the best candidate here.

### Detailed Use Cases

#### Backup and Restore
You can use Amazon S3 to store and retrieve any amount of data, at any time. You can use Amazon S3 as the durable store for your application data and file-level backup and restore processes. Amazon S3 is designed for 99.999999999 percent durability, or 11 9s of durability.

#### Data Lakes for Analytics
Run big data analytics, artificial intelligence (AI), machine learning (ML), and high performance computing (HPC) applications to unlock data insights.

#### Media Storage and Streaming
You can use Amazon S3 with Amazon CloudFront edge locations to host videos for on-demand viewing in a secure and scalable way. Video-on-demand (VOD) streaming means that your video content is stored on a server and viewers can watch it at virtually any time. You'll learn more about Amazon CloudFront later in this course.

#### Static Website
You can use Amazon S3 to host a static website. On a static website, individual webpages include static content. They might also contain client-side scripts. Amazon S3 object storage makes it easier to manage data access, replications, and data protection for static files.

#### Archiving and Compliance
Replace your tape with low-cost cloud backup workflows, while maintaining corporate, contractual, and regulatory compliance requirements.

---

## Buckets and Objects Relationship

Now, the relationship between buckets and objects. Buckets, as I said, is the logical storage units where you put your information inside. So you can go to Amazon S3, create a bucket. And bucket names, they must be globally unique. And the reason why bucket names must be globally unique is because there is a feature called static website hosting, where the bucket name goes to a URL. And you cannot have two URLs in the world pointing to two different locations with the same name. So bucket names, they must be globally unique.

### Amazon S3 Storage Structure

Amazon S3 stores data as objects within buckets. An object is composed of a file and any metadata that describes that file. To store an object in Amazon S3, upload the file into a bucket.

You can have one or more buckets in your account. When you create a bucket, remember the bucket name should be globally unique. For each bucket:

- You can control who can create, delete, and list objects in the bucket
- Choose the geographical AWS Region where Amazon S3 will store the bucket and its contents
- You can access logs for the bucket and its objects

Amazon S3 allows up to 100 buckets in each account. When you upload a file, you can set permissions on the object and add metadata.

### Object Keys and Addressing

An object key is the unique identifier for an object in a bucket. The combination of a bucket, key, and version ID uniquely identifies each object. Every object can be uniquely addressed through the combination of the web service endpoint, bucket name, key, and optionally, a version.

**Example URL Structure:**
```
https://my-bucket.s3.amazonaws.com/2006-03-01/pup.jpg
```

In this URL:
- `my-bucket` is the name of the bucket
- `2006-03-01/pup.jpg` is the key
- `2006-03-01/` portion of the object key is called the prefix

---

## Data Access Methods

To access data from Amazon S3, you can access by doing direct APIs to the service authenticated by an IAM entity, which can be a user or a role, one of these credentials that Russ talked about. Or you can have Amazon S3 serving contents out of an HTTPS frontend. You can control access to the objects and its buckets. And an object includes the file and any metadata that describes the file, right? So objects can have metadata. And when this object is served using this HTTPS layer, this metadata become HTTP response headers. So when you do a get request out of these objects, this metadata that you have in the objects, they become HTTP response headers.

### Access Methods
1. **Direct API Access**: Authenticated by IAM entities (users or roles)
2. **HTTPS Frontend**: Web-based access with metadata as HTTP response headers

---

## Practical S3 Bucket Creation

Amazon S3 stores data as objects within buckets. Let me go to my AWS Management Console and explore a little bit of the Amazon S3 by creating a bucket, which I can easily do by choosing an option on the upper right corner that says, Create bucket. So let me click with my mouse in that Create buckets, and then I can specify some attributes. Let me make my font size a little bigger here. 

### Bucket Creation Process

**Bucket name**: Remember, it must be globally unique. So I'm going to invent something called raf-bucket-architecting-course. 

**Regional Consideration**: Now, remember when I said that there are some AWS services that are Regional and some AWS services that operates in an Availability Zone. That's what I mean. The AWS Management Console is asking me to specify the Region where I want this bucket to be created, which in this case is already prepopulated with California, US west zone. I don't need to specify an Availability Zone to create the bucket.

**Simple Creation**: Buckets are Regional, so I just specify the Region and creating a bucket is as simple as choosing Create bucket. I can scroll down, leave everything under the folds because I am going to explore these options later. And then the bucket is created. From this moment, I can upload data to that bucket. I can do that programmatically or I can do that from within the AWS Management Console. But that is how easy you can create buckets.

---

## Object Storage Concepts

Now, this bucket can have objects inside. This objects are composed of an object key, which is actually something that looks like a folder because you're using slash, but it is actually not a folder, it is a prefix. So when you see something on Amazon S3 that seems to be inside a folder, we don't have the concepts of files and folders in Amazon S3 because it's not a block storage solution. It is an object storage solution where each one of these objects are individual and they have a full name. That full name can have a prefix, which may give you the impression that it is a folder but it's not because Amazon S3 is an object storage service. It's not a block storage service. It doesn't have a file system where you can interact with, right?

### Key Object Storage Principles
- **No traditional file/folder structure**: Objects have keys with prefixes that appear like folders
- **Individual objects**: Each object is independent with a full name
- **Prefix system**: Forward slashes create logical organization without actual folders
- **Object storage nature**: Not a file system - pure object storage

So when you serve content via HTTP by having the static website feature, or Amazon S3, the full web servers, you can have the content served via HTTPS. Now how to, how can you secure your objects? Let me talk a little bit about access control. In other words, who can access what, inside your buckets, and also some options about encryption.

---

## Amazon S3 Technical Overview

### Core Characteristics

Amazon S3 is object-level storage. An object includes file data, metadata, and a unique identifier. Object storage does not use a traditional file and folder structure.

Amazon S3 storage tiers are all designed to provide 99.999999999 percent (11 9s) of data durability of objects over a given year. By default, data in Amazon S3 is stored redundantly across multiple facilities and multiple devices in each facility. Amazon S3 can be accessed through the web-based AWS Management Console, programmatically through the API and SDKs, or with third-party solutions (which use the API and SDKs).

### Key S3 Features

#### Accelerate Innovation
Integrate S3 buckets as storage solutions for static files and rely less on traditional file systems.

#### Increase Agility
With hosted object storage, you won't need to expand your storage as the quantity and size of data grows. Individual objects cannot be larger than 5 TB and you can store as much total data as you need.

#### Reduce Cost
Use the variety of storage tiers in Amazon S3 to spend less on infrequently accessed data. Archive data in S3 for your long-term storage needs.

#### Strengthen Security
Amazon S3 can help you meet regulatory requirements for compliance programs such as PCI-DSS, HIPAA/HITECH, FedRAMP, the EU Data Protection Directive, and FISMA.

---

## S3 Bucket URL Structure

The diagram contains a virtual-hosted–style access URL made from a bucket and an object key:

**S3 bucket URL format:**
```
https://bucket-name.s3.amazonaws.com/object-key
```

**Example:**
```
https://my-bucket.s3.amazonaws.com/2006-03-01/pup.jpg
```

Where:
- `my-bucket` = bucket name
- `2006-03-01/pup.jpg` = object key
- `2006-03-01/` = prefix (appears like a folder path)

---

## Summary and Next Steps

This lesson explored the basics of Amazon S3, covering:
- Use cases and benefits of S3
- Bucket and object relationships
- Regional considerations and global uniqueness requirements
- Object storage concepts vs traditional file systems
- Access methods and URL structures
- Practical bucket creation process

Next, learn about how to secure objects stored in Amazon S3.

# Amazon S3 Access Control and Security

## Access Control Overview

By default, all Amazon S3 resources—buckets, objects, and related resources (for example, lifecycle configuration and website configuration)—are private. Only the resource owner, an AWS account that created it, can access the resource. The resource owner can grant access permissions to others by writing access policies.

You can make a resource in Amazon S3 public which will allow anyone access. However, most Amazon S3 use cases do not require public access.

---

## Bucket Policies

### Resource-Based Access Control

You can create and configure bucket policies to grant permission to your Amazon S3 buckets and objects. Bucket policies are resource-based policies for your S3 buckets. Access control for your data is based on policies, such as IAM policies, S3 bucket policies, and AWS Organizations service control policies (SCPs).

### Bucket Policy Implementation

So you can go in the bucket, and say this specific IAM group, or these individuals, IAM users, or roles, can do specific actions in this bucket. In IAM, you are specifying which services the user can interact with. In the bucket, you are specifying which users can interact with that specific bucket. In this case, we are allowing everyone to do two specific actions, which are list buckets, and get object. To everywhere is in this bucket. So represented by the star here. So this is in the bucket side, and it's allowing everybody in the AWS accounts. That's why we have the principal here. The principal is the actor that we wants to allow or deny the specific actions. So bucket policies, they are resource based policies for S3, and they control access to a bucket without managing permissions in IAM.

### JSON-Based Policy Language

Use JSON-based access policy language to write your bucket policy. You can use it to add or deny permissions for the objects in your bucket.

In the example, the bucket policy allows any principal to list the bucket and get any object from the bucket. You should consider limiting public access to buckets and objects like this. Amazon S3 has tools that help you to prevent overly-permissive public buckets.

### Block Public Access Settings

They are in the bucket side, so there is also an S3 configuration that can block public access change. This does not prevent the bucket from being accessed. This prevents the configuration from being changed. If you go to the AWS Management Console, and if you go to Amazon S3. In the Amazon S3 console there is an option that says block public access settings for this account. This prevents the configurations from being changed, and by the default it's all enabled, So blocking public access is on. If you want to be able to change a bucket policy, you have to come to this location here, and first disable that. That's how it's done. Once that configuration is disabled, then you will be able to go in buckets and change their bucket policies. So, this is basically the same thing that I have on this slide. So, this is basically the same thing I have here. You can block public access to configurations.

---

## Amazon S3 Access Points

### Network-Based Access Control

Amazon S3 access points is a form of doing access control based on network. So, you can have things like, if the request is for /finance, then authorize the request. But, if the request is for /sales or /marketing, then use an access point policy that denies that access. So, each access points on Amazon S3 access points is a feature of Amazon S3. Each of these access points will provide you a unique DNS name, and an ARN, an Amazon Resource Name. And it will give you distinct permissions and network controls.

### Access Points Overview

Amazon S3 access points simplify managing data access at scale for shared datasets in S3. S3 access points are named network endpoints that you can use to perform S3 object operations, such as GetObject and PutObject. Access points are attached to buckets. Each access point has distinct permissions and network controls that Amazon S3 applies for any request made through that access point.

### Access Point Policy Integration

Each access point enforces a customized access point policy that works in conjunction with the bucket policy attached to the underlying bucket. To restrict Amazon S3 data access to a private network, you can configure an access point to accept requests only from a VPC. You can also configure custom block public access settings for each access point.

### Practical Example

In the example, a finance employee assumes the finance team IAM role and sends a GetObject request to your finance access point. The access point policy allows the finance role to get objects in doc-example-bucket with the prefixes /finance and /tax. The finance role does not have access to your sales and marketing prefixed objects or any other objects in your S3 bucket. The S3 bucket policy allows the finance access point to have access to your bucket.

---

## Data Encryption

### Encryption Types Overview

Now, let's talk about data encryption. And, the first thing I would like to talk about, when talking about data encryption, is the difference between encryption in transit and encryption at rest. These are different things, and they should be looked differently, when talking about storage.

#### Encryption at Rest
Encryption at rest is when the data that you are storing is encrypted in the volumes, in the hard disks, or in the SSD devices, in the physical media. This is encryption at rest.

#### Encryption in Transit
Encryption in transit is when data is being provided to users with encryption along the way, right? So here we are basically talking about encryption at rest in this slide deck, with Amazon S3 managed keys, with SSE-S3, AWS KMS keys, and customer-provided keys. And the reason why this slide is talking about these three encryption methods is because Amazon S3 already provides encryption in transit, with the SSL certificates managed by the service endpoints.

### Default Encryption in Transit

So, every single operation that you do through Amazon S3, regardless if it is PUT objects, GET objects, DELETE objects, LIST objects, it goes with encryption in transit provided by SSL.

---

## Server-Side Encryption Key Types

### AWS KMS Keys (SSE-KMS)

Now, for the encryption at rest, you can have Amazon S3 getting encryption keys from a service called KMS or Key Management Service. So, when you do the upload, you can say, I want Amazon S3 to go on KMS, get a key, encrypt this object, store it encrypted, right, and discard the encryption key. In order to make that get object, your user must be authorized to use that Customer Master Key on KMS.

Amazon S3 managed keys and AWS keys, meaning SSE-S3 and SSE-KMS are very similar. The difference of them is that on the Amazon KMS keys, you can specify which encryption key you wants to use. And, KMS is also one of these services that provides a service based policy. So it can go on KMS and have an extra layer of permissioning in a way that if you wants to allow a user to access an object that had been encrypted with the SSE-KMS key, the user must not only be able to GET object, but also interact with that KMS key.

**Key Features:**
- KMS keys stored in SSE-KMS are similar to SSE-S3, but with some additional benefits and charges
- There are separate permissions for the use of a KMS key that provides added protection against unauthorized access of your objects in Amazon S3
- SSE-KMS also provides you an audit trail that shows when your KMS key was used, and by whom

### Amazon S3 Managed Keys (SSE-S3)

**Key Features:**
- When you use SSE-S3, each object is encrypted with a unique key
- As an additional safeguard, it encrypts the key itself with a primary key that it regularly rotates
- Amazon S3 server-side encryption uses 256-bit Advanced Encryption Standard (AES-256) to encrypt your data
- Provided by default

### Customer-Provided Keys (SSE-C)

You can also use your custom encryption key, and remember they go with encryption in transits. In this case, when you do the PUT object operation, you must also provide the encryption key, right? Amazon S3 uses that encryption key, and then it does the server side encryption. The encryption is made on the server side, right? So you provide the key, Amazon S3 uses that key, discards that key and saves a salted HMAC of that key. And it saves that salted HMAC, because if you want to do the get object, you need to provide the very same encryption key that was used. Amazon S3 will see that it is the same key, because that's why it saves the salted HMAC, and then it decrypts the objects, and sends you the objects, right? The data.

**Key Features:**
- With SSE-C, you manage the encryption keys and Amazon S3 manages the encryption as it writes to disks
- Also, Amazon S3 manages decryption when you access your objects
- You must provide the same encryption key for both PUT and GET operations
- Amazon S3 saves a salted HMAC of the key for verification

### Server-Side Encryption Summary

So you can have server side encryption, meaning that encryption is being done on the S3 side, with three different ways:
1. **Amazon S3 managed keys (SSE-S3)** - provided by default
2. **Amazon AWS KMS keys (SSE-KMS)** - with additional permissions and audit capabilities
3. **Customer provided keys (SSE-C)** - you manage the keys, S3 manages encryption/decryption

---

## Key Concepts Summary

### Access Control Methods
- **Default**: All resources are private by default
- **IAM Policies**: Control which services users can interact with
- **Bucket Policies**: Control which users can interact with specific buckets
- **Access Points**: Network-based access control with unique DNS names and ARNs
- **Block Public Access**: Prevents configuration changes to public access settings

### Security Layers
- **Resource-based policies**: Bucket policies and access point policies
- **Identity-based policies**: IAM policies for users, groups, and roles
- **Network controls**: VPC-only access points and custom block public access settings
- **Encryption**: Both in transit (SSL) and at rest (SSE-S3, SSE-KMS, SSE-C)

### Best Practices
- Use principle of least privilege
- Limit public access unless specifically required
- Use access points for complex access patterns
- Enable appropriate encryption based on security requirements
- Regularly audit access policies and permissions

---

## Next Steps

Now that you understand how to secure objects stored in an S3 bucket, let's explore what storage classes are available in S3.

# Amazon S3 Storage Classes and Data Management

## Storage Classes Overview

Amazon S3 Glacier is a service that gives you low cost, powerful, and flexible data storage solutions. The storage is purpose-built for your archived data. You can choose between Deep Archive, Instant Retrieval, or Flexible Retrieval. It is secure in compliance, because it leverages the same mechanisms of encryption in transit and encryption at rest that I just talked. And it's scalable and durable, because it meets from gigabytes to exabytes with 11 9s of durability. When I say 11 9s of durability, I'm mentioning 99.999999999, 11 times, which is a very high durability.

---

## Understanding Durability vs Availability

When we talk about storage, we need to consider durability and availability. So if you hear these two terms, here's an explanation that will make it easier for you to differentiate durability and availability.

### Practical Analogy

So imagine that I have a friend, and that friend is my neighbor, and I trust my friend. And I want my friend to store something that belongs to me, such as the spare key of my house. So I go to my friend, and I send him this spare key of my house, and I say, "Hey, can you put this spare key of my house in the drawer in your home?" And my friend says, "Yeah, sure." So I'm using my friend to store something, right? 

Now let's say I need to access that spare key of my house, right, and then I call my friend, and I say, "Hey, I'm locked outside, and I need to access something that you were storing for me. Can I have the spare key?" And one thing is my friend saying me, "Hey Raf, I still have your key, it is in a drawer in my house, but I'm traveling, and I can't give you the key right now." In this case, my friend was durable but not available, because he didn't lose what I asked him to store, but he wasn't available to provide it to me. In the other hand, my friend could be right here in front of me, we could have access to his house, but he lost my key. In this case, he's being available but not durable.

### Amazon S3 Implementation

Well, Amazon S3 provides both high durability and high availability, and it does that because it replicates every single objects across multiple servers in multiple facilities within an AWS Region.

---

## S3 Storage Classes Detailed

### S3 Standard
For general-purpose storage of frequently accessed data.

### S3 Standard-Infrequent Access (Standard-IA)
For long-lived, but less frequently accessed data.

### S3 One Zone-Infrequent Access (One Zone-IA)
For long-lived, less frequently accessed data that can be stored in a single Availability Zone.

### S3 Glacier Instant Retrieval
For archive data that is rarely accessed but requires a restore in milliseconds.

### S3 Glacier Flexible Retrieval
Retrieval for the most flexible retrieval options that balance cost with access times ranging from minutes to hours. Your retrieval options permit you to access all the archives you need, when you need them, for one low storage price. This storage class comes with multiple retrieval options:
- **Expedited retrievals**: restore in 1–5 minutes
- **Standard retrievals**: restore in 3–5 hours  
- **Bulk retrievals**: restore in 5–12 hours (available at no additional charge)

### S3 Glacier Deep Archive
For long-term cold storage archive and digital preservation. Your objects can be restored in 12 hours or less.

### S3 Intelligent-Tiering
An additional storage class that provides flexibility for data with unknown or changing access patterns. It automates the movement of your objects between storage classes to optimize cost.

---

## Amazon S3 Intelligent-Tiering

Amazon S3 Intelligent-Tiering is the only storage class that delivers you automatic storage cost savings when data access patterns change, with virtually no performance impact or operational overhead. Your data moves between access tiers as usage patterns change.

### How Intelligent-Tiering Works

When you assign an object to S3 Intelligent-Tiering, it is placed in the Frequent Access tier which has the same storage cost as S3 Standard. Objects not accessed for 30 days are then moved to the Infrequent Access tier where the storage cost is the same as S3 Standard-IA. After 90 days of no access, an object is moved to the Archive Instant Access tier, which has the same cost as S3 Glacier Instant Retrieval.

### Use Cases for Intelligent-Tiering

S3 Intelligent-Tiering is the storage class that works well for data with unknown, changing, or unpredictable access patterns, independent of object size or retention period. You can use S3 Intelligent-Tiering as the default storage class for virtually any workload, especially:
- Data lakes
- Data analytics
- New applications
- User-generated content

---

## Amazon S3 Glacier Storage Class Benefits

### Cost-effective Storage
Offers lowest cost for specific data access patterns

### Flexible Data Retrieval
Provides three different storage classes with variable access options

### Secure and Compliant
Provides encryption at rest, AWS CloudTrail integration and retrieval policies

### Scalable and Durable
Meets scalability requirements from gigabytes to exabytes with 11 9s of durability

---

## Lifecycle Policies

### Overview

You can have lifecycle policies on Amazon S3 to automatically transition your objects from one storage class to another. And an interesting strategy could be move objects older than, let's say, 30 days from Amazon S3 Standard to Amazon S3 Standard-IA, Infrequent Access. And then after a year, you can move objects to Amazon S3 Glacier Deep Archive. Speaking like that, you may think it's complex, but this is actually how you configure that.

### Practical Implementation

You go on Amazon S3, you choose a bucket, such as the bucket I just created. Let me find the bucket here in the list to see the bucket that I just created, raf-bucket-architecting-course. You go on Management. There is an option of creating a lifecycle rule. I just click in Create a lifecycle rule. I can give that rule a name, ArchivalAfter1Year. I can apply this lifecycle rules to every single object in my bucket. And I can move current versions of objects between storage classes. I can move to Standard-IA after 30 days. I'm doing the very same thing as the slide deck, 30 days. And I can add another transition, to move it to Glacier Deep Archive after 365 days. And then I review the lifecycle policy options. It will contain a reviewing phase, so it tells me that Day 0, objects are uploaded, then after 30 days, these objects transition to another storage class. After 1 year, it got moved to Glacier Deep Archive. So I can easily create that archival rule, which is the very same thing that we have on the slide deck right now.

### Lifecycle Policy Benefits

Use S3 Lifecycle polices to transition objects to another storage class based on the age of the data object. You should automate the lifecycle of your data stored in Amazon S3. Using S3 Lifecycle policies, you can have data cycled at regular intervals between different Amazon S3 storage types.

---

## Replicating S3 Objects

### High Availability and Durability

With Amazon S3, customers get a high level of availability and durability for their data in every AWS Region. Data stored in any Amazon S3 storage class is stored across a minimum of three Availability Zones, each separated by miles within a Region. For this reason, many AWS customers choose Amazon S3 to store their business-critical and application-critical data. The only exception is S3 One Zone-IA, which, as its name indicates, is a one-zone service.

### Cross-Region and Same-Region Replication

You can also replicate Amazon S3 objects by enabling something called Cross-Region Replication, or Same-Region Replication. CRR, Cross-Region Replication, or Same-Region Replication, SRR. This is suitable for when you want to have high availability over your data, and can be a good disaster recovery plan. You can replicate your objects to another bucket that lives in another Region. So I can have my primary bucket in US East-1, Virginia Region, and I can replicate the data to another bucket in another continent. Remember when I said that you can go global in minutes, and explore a global architecture on a very easy way with AWS. You can, for example, enable Amazon S3 Cross-Region Replication to have your objects replicated across overseas to another bucket. And this finishes the Amazon S3 Replicating Objects.

### Replication Features

#### Replicate Objects While Retaining Metadata
Ensure that your replica is identical to the source object if it is necessary.

#### Replicate Objects Into Different Storage Classes
Use replication to directly put objects into S3 Glacier Flexible Retrieval, S3 Glacier Deep Archive, or another storage class in the destination buckets.

#### Maintain Object Copies Under Different Ownership
Tell Amazon S3 to change replica ownership to the AWS account that owns the destination bucket.

#### Keep Objects Stored Over Multiple AWS Regions
Meet compliance requirements by replicating data to another AWS Region.

---

## Key Concepts Summary

### Storage Class Selection Criteria
- **Access frequency**: How often data is accessed
- **Retrieval time requirements**: Milliseconds to hours
- **Cost optimization**: Balance storage cost with access costs
- **Durability requirements**: All classes provide 11 9s durability
- **Availability needs**: Consider single vs multi-AZ requirements

### Automation Features
- **Lifecycle policies**: Automatic transitions between storage classes
- **Intelligent-Tiering**: Automatic cost optimization based on access patterns
- **Replication**: Cross-region and same-region data replication
- **Metadata preservation**: Maintain object properties during transitions

### Best Practices
- Use lifecycle policies to automate cost optimization
- Consider Intelligent-Tiering for unpredictable access patterns
- Implement replication for disaster recovery and compliance
- Choose appropriate storage class based on access requirements
- Monitor and adjust policies based on actual usage patterns

---

## Next Steps

Amazon S3 provides several features to enable customers with effective storage options. Now that you have explored available S3 storage classes, review some useful features for your applications in the next lesson.

# Additional Amazon S3 Features

## Introduction

In this lesson, you learn about the additional S3 features you can leverage to build storage for your applications.

And here are some additional Amazon S3 features.

---

## Amazon S3 Event Notifications

### Overview

Starting with Amazon S3 event notifications, which is one of my favorite features in Amazon S3, meaning that you can run code powered by AWS Lambda in response to activity in a bucket. So you can, for example, run a Lambda code that resizes an image every time you have an image upload in the bucket. You can even work with AWS Step Functions to develop a whole state machine of things you must do based on a PUT object operation. So with Amazon S3 events notifications, you can get notifications sent when events happen in your S3 bucket and these notification can run Lambda functions for you.

### Practical Use Cases

A very typical usage for that is creating Lambda functions that resizes images and creates thumbnails out of your images. Sometimes you are running a news portal and you want to have a serverless logic that will receive images in your bucket and then run Lambda functions that would use a Python library or a Node.js library that will get that image resized and saved in the same bucket or in another bucket. And you can do that programmatically from within the Lambda code by using one of the AWS SDKs.

### Event-Driven Architecture Benefits

With Amazon S3 event notifications, you can receive notifications when certain object events happen in your bucket. Event-driven models like this mean that you no longer have to build or maintain server-based polling infrastructure to check for object changes. Nor do you have to pay for idle time of that infrastructure when there are no changes to process.

### Supported Destinations

Amazon S3 can send event notification messages to the following destinations:
- Amazon Simple Notification Service (Amazon SNS) topics
- Amazon Simple Queue Service (Amazon SQS) queues
- AWS Lambda function

---

## Amazon S3 Multipart Upload

### Overview

Another feature is Amazon S3 multipart upload, meaning that when you PUT an object to Amazon S3, you can initiate multiple upload threads to make this upload faster. You initiate the upload, you can upload object parts, and you can interrupt the upload and resume later. With the usage of AWS SDKs you can configure things like the chunk size and the number of parallel threads that you want to upload. So you can custom tailor if you are using mobile applications or if you're using computers uploading data with stable internet connections. So you can fine-tune the chunk size and the number of upload threads, but you cannot perform multipart uploads manually using the AWS Management Console. This is something that you are more interested in doing if you're interacting programmatically with Amazon S3.

### Three-Step Process

Amazon S3 allows multipart upload, where you can consistently upload large objects in manageable parts. The process involves three steps:

1. **Initiate the upload**
2. **Upload the object parts**
3. **Complete the multipart upload**

### How It Works

When the multipart upload request is completed, Amazon S3 will re-create the full object from the individual pieces.

### Configuration Options

- **Chunk size**: Customizable based on application needs
- **Parallel threads**: Adjustable number of concurrent uploads
- **Resume capability**: Interrupt and resume uploads as needed
- **Application-specific tuning**: Different settings for mobile vs stable connections

### Important Note

Multipart uploads are designed for programmatic use with AWS SDKs and cannot be performed manually through the AWS Management Console.

---

## Amazon S3 Transfer Acceleration

### Overview

Another feature is using edge locations with S3 Transfer Acceleration. Meaning that if you have an operation where users are uploading contents to your bucket all over the world, a closest and nearest edge location to that user can get that request and use the Amazon network to upload data to Amazon S3, without having the package traversing over the internet according to the internet service provider from that user. So it's something like AWS puts a nearest edge location that has more chances of being closer to these users, and then they upload to that agile location. And from there it quickly gets to the object because it leverages the Amazon private network, which tends to be more optimized from one endpoint to another endpoint, both Amazon endpoints, than having to traverse the internet with the routes provided by the user's internet service provider. That feature is called Amazon S3 Transfer Acceleration, and it's very useful to move data faster over long distances and reduce network variability.

### How Transfer Acceleration Works

Amazon S3 Transfer Acceleration uses AWS globally-distributed edge locations to facilitate fast data transfer into an S3 bucket. The data is routed to Amazon S3 over an optimized network path.

### When to Use Transfer Acceleration

Use Transfer Acceleration when you:
- Have customers all over the world who upload to a centralized bucket
- Transfer gigabytes or terabytes of data across continents on a regular basis
- Underutilize the available bandwidth when uploading to Amazon S3 over the internet

### Benefits

- **Faster uploads**: Leverages AWS's optimized network infrastructure
- **Reduced variability**: More consistent performance than internet routing
- **Global reach**: Edge locations worldwide provide local access points
- **Network optimization**: Uses Amazon's private network for better performance

---

## Amazon S3 Cost Factors

### Overview

There are also other factors in Amazon S3 and cost is an important part of choosing the Amazon S3 storage solutions. So, these are also aspects that will impact in the cost replication request and retrievals, storage types, data transfer and Versioning. So, keep in mind that some of these features may make your S3 buckets more or less expensive. For example, if you enable Versioning, which is a feature that will not delete an object after you replace that object, you start creating version IDs and AWS Amazon S3 is a service that charges you per amount of data that you are using in the bucket. So if you have Versioning enabled, that could impact on the cost as well.

### Detailed Cost Components

#### Storage
Per-gigabyte cost to hold your objects. You pay for storing objects in your S3 buckets. The rate you're charged depends on your objects' size, how long you stored the objects during the month, and the storage class. There are per-request ingest charges when using PUT, COPY, or lifecycle rules to move data into any S3 storage class.

#### Requests and Retrievals
The number of API calls: PUT and GET requests. You pay for requests made against your S3 buckets and objects. S3 request costs are based on the request type, and are charged on the quantity of requests. When you use the Amazon S3 console to browse your storage, you incur charges for GET, LIST, and other requests that are made to facilitate browsing.

#### Data Transfer
Usually no transfer fee for data-in from the internet and, depending on the requestor location and medium of data transfer, different charges for data-out.

#### Management and Analytics
You pay for the storage management features and analytics that are activated on your account's buckets. These features are not discussed in detail in this course.

#### S3 Replication and Versioning
Both replication and versioning create multiple copies of your objects and you pay for each PUT request in addition to the storage tier charge. S3 Cross-Region Replication also requires data transfer between AWS Regions.

### Cost Impact Considerations

- **Versioning**: Creates multiple object versions, increasing storage costs
- **Replication**: Duplicates data across regions or within regions
- **Storage class selection**: Different classes have different pricing models
- **Request patterns**: Frequent access vs infrequent access affects costs
- **Data transfer**: Cross-region and internet egress charges apply

---

## Feature Integration and Best Practices

### Event Notifications Best Practices
- Use Lambda functions for serverless processing
- Implement error handling and retry logic
- Consider using SQS for reliable message processing
- Monitor function execution and costs

### Multipart Upload Best Practices
- Use for objects larger than 100 MB
- Configure appropriate chunk sizes for your network
- Implement retry logic for failed parts
- Clean up incomplete multipart uploads

### Transfer Acceleration Best Practices
- Test acceleration benefits for your use case
- Monitor transfer speeds and costs
- Consider for global user bases
- Evaluate against direct uploads

### Cost Optimization Strategies
- Choose appropriate storage classes
- Implement lifecycle policies
- Monitor and analyze usage patterns
- Use cost management tools and alerts

---

## Summary

Amazon S3 provides powerful additional features that enhance functionality and performance:

- **Event Notifications**: Enable serverless, event-driven architectures
- **Multipart Upload**: Improve upload performance and reliability for large objects
- **Transfer Acceleration**: Optimize global data transfer using edge locations
- **Cost Factors**: Multiple pricing components require careful consideration

These features work together to provide a comprehensive object storage solution that can be tailored to specific application requirements and usage patterns.

---

## Next Steps

That wraps up S3. In the next lesson, you review options for building secure and scalable storage using shared file systems.

# Shared File Systems

## Module Overview
This lesson covers shared file systems in AWS, focusing on Amazon Elastic File System (EFS) and Amazon FSx services.

**Video Duration**: 4 minutes, 27 seconds

---

## Introduction to Shared File Systems

### Transition from Object Storage
Shared file systems provide access to file systems where you can put files and edit these files, transitioning from object storage solutions.

### Use Case Scenario
Consider the following requirements:
- Instances on different Availability Zones need to use the same storage
- Storage cannot be object storage like Amazon S3 (S3 is object storage, not built for file systems)
- Instances may be across different AWS Regions
- On-premises servers and EC2 instances need to access the same file system

### Limitations of Existing Solutions
- **EBS Volumes**: Usually attached to one instance
  - EBS Multi-Attach feature exists but requires instances to stay in the same Availability Zone
- **Amazon S3**: Object storage, not built for file systems

### Solution: AWS File System Services
The answer lies in **Amazon EFS (Elastic File System)** and **Amazon FSx**, which are ideal for these requirements.

---

## Amazon Elastic File System (Amazon EFS)

### Overview
Amazon EFS provides a scalable, elastic file system for Linux-based workloads for use with AWS Cloud services and on-premises resources.

### Key Features
- **Distributed Architecture**: File system distributed across a Region
- **VPC Integration**: Sits in a VPC and builds nodes in multiple subnets you specify
- **Protocol**: Uses NFSv4 (Network File System version 4) protocol
- **Multi-Access**: Multiple EC2 instances can access the file system simultaneously
- **Cross-Location Access**: Accessible from different AZs, Regions, and on-premises (with Site-to-Site VPN)

### How It Works
1. **File System Creation**: Create a file system in Amazon EFS
2. **Mount Targets**: Service provides mount targets per Availability Zone
3. **DNS Names**: Mount targets have DNS names for connection
4. **Client Connection**: Use NFS client on Windows or Linux machines to connect and mount

### Architecture Example
- VPC with Amazon EFS standard storage class
- Three private subnets, each in different AZ
- Each subnet has its own mount target with standard storage class
- EC2 instances in each subnet access file system through local AZ mount target

### Benefits
- **Automatic Scaling**: Automatically grows and shrinks without provisioning
- **Fully Managed**: Transparent operation - create file system, get endpoints, start using
- **Pay-per-Use**: Charges based on space occupied, not predetermined cost
- **Burst Throughput**: Scales throughput based on storage usage (bigger file system = more throughput)
- **Cost Effective**: Lower total cost of ownership, pay only for what you use
- **Shared Environment**: Provides scalable shared storage environment

### File Operations
- Create file system
- Mount on Amazon EC2 instance
- Read and write data to/from file system
- Mount through NFS versions 4.0 and 4.1 (NFSv4) protocol
- No action needed to expand as storage needs grow

---

## Amazon FSx

### Overview
Amazon FSx allows you to quickly launch and run feature-rich and high-performing file systems using industry-standard technologies.

### Service Benefits
- **Familiar Technologies**: Uses industry technologies highly adopted in on-premises architectures
- **Minimal Migration**: Customers can migrate existing clusters (Lustre, OpenZFS, etc.) to AWS with minimal configuration
- **Fully Managed**: AWS manages hardware provisioning, patching, and backups
- **Industry Standards**: Launch, run, and scale high-performing file systems using familiar technologies

### Four File System Options

#### 1. FSx for Windows File Server
- **Technology**: Fully managed Microsoft Windows file servers
- **Backend**: Native Windows file system built on Windows Server
- **Features**: 
  - Data deduplication
  - End-user file restore
  - Microsoft Active Directory integration
  - Wide range of administrative features

#### 2. FSx for Lustre
- **Technology**: Fully managed high-performance, cost-effective storage
- **Compatibility**: Works with popular Linux-based AMIs including:
  - Amazon Linux
  - Amazon Linux 2
  - Red Hat Enterprise Linux (RHEL)
  - CentOS
  - SUSE Linux
  - Ubuntu

#### 3. FSx for NetApp ONTAP
- **Technology**: Fully managed shared storage in AWS Cloud
- **Features**: Data access and management capabilities of ONTAP
- **Integration**: Native ONTAP features and functionality

#### 4. FSx for OpenZFS
- **Technology**: Fully managed shared file storage built on OpenZFS file system
- **Infrastructure**: Powered by AWS Graviton family of processors
- **Protocol**: Accessible via NFS protocol (v3, v4, v4.1, v4.2)
- **Performance**: Optimized for high-performance workloads

### Use Cases
- **Migration Scenarios**: Move existing on-premises file system clusters to AWS
- **Hybrid Environments**: Support both cloud and on-premises workloads
- **High-Performance Computing**: Leverage industry-standard high-performance file systems
- **Enterprise Applications**: Support enterprise-grade file system requirements

---

## Storage Navigation Summary

This comprehensive storage discussion provides guidance for navigating different AWS storage options:

### Block Storage
- **Amazon EBS**: Volumes attached to EC2 instances
- **Use Case**: Instance-level storage with high performance

### File Systems
- **Amazon EFS**: Distributed, scalable file systems for Linux workloads
- **Amazon FSx**: Managed file systems using industry-standard technologies
- **Use Case**: Shared file access across multiple instances and locations

### Object Storage
- **Amazon S3**: Scalable object storage with multiple storage classes
- **Amazon Glacier**: Long-term archival and backup
- **Use Case**: Web applications, content distribution, backup and archiving

---

## Next Steps

**Data Migration**: The next lesson covers data migration tools for moving data among different file systems and storage solutions.

# Data Migration and Transfer Services

## Module Overview
This lesson covers AWS services for data migration and transfer, including hybrid storage solutions and offline data transfer options.

---

## AWS Storage Gateway

### Overview
AWS Storage Gateway is a service that gives your applications seamless and secure integration between on-premises environments and AWS storage. It connects an on-premises software appliance with cloud-based storage to provide seamless integration with data security features between your on-premises IT environment and the AWS storage infrastructure.

### How It Works
Storage Gateway acts as an intermediary service that interacts with AWS storage services. It presents itself in the network as a file system or virtual tape library while using AWS services under the hood to store the information. You maintain all the backing-up capabilities of your backup software, but Storage Gateway stores data in Amazon S3 or Amazon Glacier.

### Architecture Components

#### Storage Gateway Appliance
- Can run as hardware appliance or software on local area network
- Runs in one of four modes: Amazon S3 File Gateway, Amazon FSx File Gateway, Tape Gateway, or Volume Gateway
- Provides seamless connection between on-premises and AWS storage

#### Transfer Protocols Supported
The Storage Gateway Appliance supports the following protocols to connect to your local data:
- **NFS or SMB** for files
- **iSCSI** for volumes  
- **iSCSI VTL** for tapes

#### Storage Services Integration
Data moved to AWS using Storage Gateway can be sent to the following destinations:
- **Amazon S3** (Amazon S3 File Gateway, Tape Gateway)
- **Amazon S3 Glacier** (Amazon S3 File Gateway, Tape Gateway)
- **Amazon FSx for Windows File Server** (Amazon FSx File Gateway)
- **Amazon EBS** (Volume Gateway)

### Storage Gateway Types

#### 1. Amazon S3 File Gateway
- **Most Popular Option**: Stores information in an Amazon S3 bucket
- **File Interface**: Presents a file interface to store files as objects in Amazon S3
- **Protocols**: Uses industry-standard NFS and SMB file protocols
- **Access Methods**: 
  - Access files via NFS and SMB from data center or Amazon EC2
  - Access files as objects directly in Amazon S3
- **Operation**: Hardware appliance or software runs on local network, everything stored goes to S3 bucket

#### 2. Volume Gateway
- **Function**: Provides block-level backup of volumes with EBS Snapshots
- **Integration**: AWS Backup integrations and cloud recovery
- **Operation**: Storage Gateway has volumes, stores information in volumes, synchronizes with AWS services
- **Protocol**: Presents block storage volumes using iSCSI protocol
- **Backup**: Asynchronously back up data as point-in-time snapshots stored as Amazon EBS snapshots

#### 3. Tape Gateway
- **Function**: Virtual tape library (VTL) using Amazon S3 archive tiers for long-term retention
- **Compatibility**: Works with existing backup software
- **Interface**: Presents iSCSI-based virtual tape library with virtual tape drives and virtual media changer
- **Management**: Stores virtual tapes in Amazon S3, creates new ones automatically
- **Benefits**: Simplifies management and transition to AWS while maintaining backup software compatibility

#### 4. Amazon FSx File Gateway
- **Integration**: Connects to Amazon FSx for Windows File Server
- **Use Case**: Hybrid file system access for Windows environments

### Benefits
- **Flexibility**: Lots of flexibility integrating on-premises data solutions with AWS managed services
- **Seamless Integration**: Maintains existing backup software and processes
- **Multiple Protocol Support**: SMB, NFS, and iSCSI protocols
- **Secure**: Built-in data security features

---

## AWS DataSync

### Overview
AWS DataSync is a data transfer service that facilitates moving data between on-premises storage and Amazon S3, Amazon EFS, or FSx for Windows File Server. It offers a middle tier between AWS storage resources and shared file systems running on-premises.

### Key Features
- **Encryption**: Encryption in transit powered by TLS 1.2 by default
- **Purpose-Built Protocols**: Uses protocols specifically created for DataSync
- **Management**: Manage from console or CLI (Command Line Interface)
- **Automation**: Automatically handles scripting copy jobs, scheduling and monitoring transfers, validating data, and optimizing network usage

### Architecture and Deployment
- **DataSync Agent**: Deploy on-premises DataSync agents running on a server
- **Single Agent**: Deploys as single software agent that can connect to multiple shared file systems
- **Multiple Tasks**: Can run multiple tasks simultaneously
- **VM Deployment**: Software agent typically deployed on-premises through virtual machine
- **WAN Transfer**: Handles transfer of data over wide area network (WAN) to AWS
- **Service Infrastructure**: On AWS side, agent connects to DataSync service infrastructure
- **No Cloud Infrastructure**: No infrastructure for customers to set up or maintain in cloud
- **Console Management**: DataSync configuration managed directly from console

### Supported Storage Services
- **Amazon S3**: Read or write data from/to S3
- **Amazon EFS**: Integration with Elastic File System
- **FSx for Windows File Server**: Support for Windows file systems

---

## AWS Transfer Family

### Overview
AWS Transfer Family permits the transfer of files into and out of Amazon S3 using the Secure File Transfer Protocol (SFTP).

### Key Features
- **Protocol Support**: SFTP (Secure File Transfer Protocol)
- **Integration**: Direct integration with Amazon S3
- **Security**: Secure file transfer capabilities

---

## AWS Snow Family

### Overview
The AWS Snow Family is a collection of physical devices that help migrate large amounts of data into and out of the cloud without depending on networks. This helps you apply the wide variety of AWS services for analytics, file systems, and archives to your data.

### When to Use Snow Family
- **Large Datasets**: For petabytes of data that would take forever using standard internet links
- **Limited Connectivity**: Even with dedicated connectivity, massive data transfers are impractical
- **Offline Transfer**: Physical transport is more efficient than network transfer for large volumes

### Snow Family Devices

#### AWS Snowcone
- **Capacity**: Smaller capacity device
- **Rugged Design**: Rugged device for harsh environments
- **Edge Computing**: Some processing capability powered by AWS Lambda
- **Code Execution**: Can run code for every data put in Snowcone device
- **Use Case**: Smaller scale data migration and edge computing

#### AWS Snowball Edge
- **Capacity**: Petabyte-scale data transport option
- **No Coding Required**: Doesn't require writing code or purchasing hardware
- **Simple Process**: 
  1. Create job in console
  2. Snowball appliance shipped to you
  3. Attach to local network
  4. Transfer files directly onto device
  5. E Ink shipping label automatically updates return address
- **Edge Processing**: Snowball Edge Optimized works well for edge processing use cases
- **Enhanced Capabilities**: Additional computing, memory, and storage power
- **Environment**: Suitable for remote, disconnected, or harsh environments
- **Capacity Range**: Varies in size, more capacity than Snowcone

#### AWS Snowmobile
- **Scale**: Exabyte scale data migration service
- **Capacity**: Can transfer 100 petabytes (PB) per Snowmobile
- **Physical Form**: Ruggedized shipping container pulled by semi-trailer truck
- **Process**: 
  - Contact AWS sales representative (not afternoon request)
  - Truck with drivers and GPS connects to data center
  - Use Snowmobile software to load data
- **Security Features**:
  - Multiple layers of security designed to protect data
  - Encryption at rest for data
  - Encryption in transit (moving encrypted data via highways)
- **Use Case**: Very large datasets from on-premises to AWS

### Security Considerations
- **Encryption at Rest**: Data encrypted while stored on devices
- **Encryption in Transit**: Data remains encrypted during physical transport
- **Physical Security**: Devices designed for secure physical transport
- **Multiple Security Layers**: Comprehensive security approach for data protection

---

## Service Comparison Summary

### Online Transfer Services
- **AWS Storage Gateway**: Hybrid integration with on-premises applications
- **AWS DataSync**: Automated data transfer with optimization and validation
- **AWS Transfer Family**: SFTP-based file transfers to S3

### Offline Transfer Services (Snow Family)
- **Snowcone**: Small-scale, edge computing capable
- **Snowball Edge**: Petabyte-scale with edge processing options
- **Snowmobile**: Exabyte-scale for massive datasets

### Selection Criteria
- **Data Volume**: Determines online vs offline approach
- **Network Capacity**: Influences transfer method choice
- **Processing Requirements**: Edge computing needs
- **Integration Needs**: Existing infrastructure compatibility
- **Security Requirements**: Encryption and compliance needs

---

## Module Transition

This wraps up the review of storage services and data migration options. 

**Next**: Short Knowledge Check followed by database services module.

# Storage Knowledge Check - Module 5

## Knowledge Check Overview
This is the knowledge check for Module 5, covering storage services. The session includes questions and detailed explanations for each answer.

**Participants**: Instructor (Raf) and Rose (Question Presenter)

---

## Question 1: Amazon S3 Cross-Region Replication

### Question
Which of the following Amazon S3 features would you use automatically to copy new objects to a bucket in a different region?

**Options:**
- A) Same-Region Replication
- B) Amazon S3 Versioning  
- C) AWS DataSync
- D) Cross-Region Replication

### Answer Analysis
**Instructor's Reasoning:**
- **Option A (Same-Region Replication)**: Can be discarded because it's same region replication, not different region
- **Option B (Amazon S3 Versioning)**: Does not replicate objects across regions
- **Option C (AWS DataSync)**: Serves another purpose, not automatic S3 replication
- **Option D (Cross-Region Replication)**: This is the correct feature that replicates objects to another bucket in another region

**Selected Answer**: D) Cross-Region Replication

**Result**: ✅ Correct

**Key Learning**: Cross-Region Replication (CRR) is the specific S3 feature designed to automatically copy new objects to a bucket in a different AWS region.

---

## Question 2: Amazon S3 Event Notifications

### Question
Which Amazon S3 feature can force an action to occur after an event takes place within a bucket?

**Options:**
- A) Invoking
- B) Event Notification
- C) Lambda
- D) Alarm

### Answer Analysis
**Instructor's Reasoning:**
- **Option C (Lambda)**: Very likely to be the compute unit that will process the action, but Lambda is not the name of the S3 feature itself
- **Option D (Alarm)**: Related to CloudWatch (covered in module 7), not the S3 feature
- **Option A (Invoking)**: More related to Lambda functions, not the S3 feature name
- **Option B (Event Notification)**: This is the correct S3 feature name that triggers actions when events occur in buckets

**Additional Context**: Although you are very likely to invoke Lambda functions after activity on S3 bucket, the correct term for the S3 feature is "event notification."

**Selected Answer**: B) Event Notification

**Result**: ✅ Correct

**Key Learning**: S3 Event Notifications is the feature that can trigger actions (often Lambda functions) when events occur within S3 buckets.

---

## Question 3: Shared File Systems for Linux Applications

### Question
You have two Linux applications in different availability zones that must share a common file system. Which of the following is the best solution for this use case?

**Options:**
- A) Storage Gateway
- B) FSx for Windows File Server
- C) Amazon S3
- D) Amazon EFS

### Answer Analysis
**Instructor's Reasoning:**
- **Option C (Amazon S3)**: Immediately discarded because S3 is object storage, not file systems. It's a regional scope service but offers object storage, not file systems
- **Option A (Storage Gateway)**: This is a device, appliance, or software that runs on-premises, mostly used to import data to AWS. Not correct for this use case
- **Option B (FSx for Windows File Server)**: While this provides file storage, the question specifically asks about Linux applications
- **Option D (Amazon EFS)**: Most native solution for Linux applications. Provides NFS v4 file system that can be mounted on Linux servers in different availability zones

**Additional Context**: Amazon EFS is regional - once you create a file system, it gives you endpoints per AZ where you can mount on corresponding instances in these AZs.

**Selected Answer**: D) Amazon EFS

**Result**: ✅ Correct

**Key Learning**: Amazon EFS (Elastic File System) is the ideal solution for Linux applications across different AZs that need shared file system access.

---

## Question 4: Storage Gateway Appliance Modes

### Question
Which of the following are modes available in Storage Gateway Appliance? (Select three options)

**Options:**
- A) Memory Gateway
- B) Tape Gateway
- C) Volume Gateway
- D) Amazon EBS File Gateway
- E) Amazon S3 File Gateway
- F) Amazon S3 Glacier File Gateway

### Answer Analysis
**Instructor's Reasoning (as of October 2022):**
- **Option B (Tape Gateway)**: ✅ Valid Storage Gateway mode according to documentation
- **Option C (Volume Gateway)**: ✅ Valid Storage Gateway mode according to documentation  
- **Option E (Amazon S3 File Gateway)**: ✅ Valid Storage Gateway mode according to documentation
- **Option A (Memory Gateway)**: Does not exist in Storage Gateway, doesn't make sense
- **Option D (Amazon EBS File Gateway)**: Not something that exists in Storage Gateway
- **Option F (Amazon S3 Glacier File Gateway)**: Not a separate mode

**Selected Answers**: B) Tape Gateway, C) Volume Gateway, E) Amazon S3 File Gateway

**Result**: ✅ Correct

**Key Learning**: The three main Storage Gateway Appliance modes are Tape Gateway, Volume Gateway, and Amazon S3 File Gateway.

---

## Knowledge Check Results

**Final Score**: Full Points (4/4 correct)

**Module Completion**: This wraps up Module 5 Storage Knowledge Check

---

## Key Takeaways from Knowledge Check

### Amazon S3 Features
- **Cross-Region Replication**: Automatically copies objects to different regions
- **Event Notifications**: Triggers actions when bucket events occur
- **Object Storage Nature**: S3 is object storage, not file system storage

### File System Solutions
- **Amazon EFS**: Best for Linux applications needing shared file systems across AZs
- **Regional Scope**: EFS provides endpoints per AZ for mounting
- **NFS v4 Protocol**: Native file system protocol for Linux environments

### Storage Gateway
- **Three Main Modes**: Tape Gateway, Volume Gateway, Amazon S3 File Gateway
- **Hybrid Solution**: Connects on-premises environments with AWS storage
- **Documentation Reference**: Always refer to current AWS documentation for latest features

### Service Differentiation
- **Storage Gateway**: Hybrid on-premises to AWS integration
- **DataSync**: Data transfer service with different purpose than S3 replication
- **Lambda**: Compute service often triggered by S3 events, but not the S3 feature itself