# Account Security

## Authorization

You must be authorized (allowed) to complete your request. During authorization, AWS uses values from the request context to check for policies that apply to the request. It then uses the policies to determine whether to allow or deny the request.

## IAM Users

By default, a new Identity and Access Management (IAM) user has no permissions assigned to them. The user is not authorized to perform any AWS operations or access any AWS resources. An advantage of having individual IAM users is that you can assign permissions individually to each user.

### Setting Permissions with IAM Policies

To allow IAM users to create or modify resources and perform tasks:

1. Create IAM policies that grant IAM users permission to access the specific resources and API operations they will need
2. Attach the policies to the IAM users or groups that require those permissions

Users only have the permissions specified in the policy. Most users have multiple policies. Together, they represent the permissions for that user.

## IAM Roles

IAM roles deliver temporary AWS credentials. They're easy to manage because multiple employees and applications can use the same role. Use roles to delegate access to users, applications, or services that don't normally have access to your AWS resources.

### Role Assumption Methods

Roles can be assumed through API calls using:
- The console
- AWS CLI
- AssumeRole API
- AWS Security Token Service (AWS STS)

The AssumeRole action returns a set of temporary security credentials consisting of an access key ID, a secret access key, and a security token. AssumeRole is typically used for cross-account access or federation.

You can switch roles from the AWS Management Console. You can assume a role by calling an AWS CLI or API operation or by using a custom URL. The method that you use determines who can assume the role and how long the role session can last. When using AssumeRole* API operations, the IAM role that you assume is the resource. The user or role that calls AssumeRole* API operations is the principal.

### Role Assumption Process

#### 1. Use an API Call to Assume a Role
You assume a role using a trusted entity, such as an IAM user, an AWS service, or a federated user.

IAM users assume roles in the AWS Management Console or AWS Command Line Interface (AWS CLI). This action uses the AssumeRole API. AWS services can use the same API call to assume roles in your AWS accounts. Your federated users use either the AssumeRoleWithSAML or AssumeRoleWithWebIdentity API calls.

#### 2. Return Temporary Security Credentials
The API call is made to AWS Security Token Service (AWS STS). AWS STS is a web service that provides temporary, limited-privilege credentials for IAM or federated users. It returns a set of temporary security credentials consisting of an access key ID, a secret access key, and a security token.

#### 3. Use Temporary Credentials
Once the temporary security credentials are received, the trusted entities can use them to access AWS resources.

## Practical Demo: Cross-Account Role Assumption

### Demo Overview
This demonstration shows how to create and assume a cross-account IAM role between two AWS accounts.

### Step 1: Creating the Role
1. **Create Role**: In the AWS Console, create a new IAM role
2. **Set Trusted Entity**: Select "AWS account" as the trusted entity type
3. **Add Account Number**: Enter the target account number that will assume the role
4. **Security Options**: Optional external ID or MFA requirements can be added
5. **Attach Permissions**: Add appropriate policies (e.g., EC2ReadOnlyAccess)
6. **Name the Role**: Provide a descriptive name (e.g., "raf-ec2-readonly")
7. **Get ARN**: Copy the role's Amazon Resource Name (ARN) for sharing

### Step 2: Assuming the Role
1. **Switch Role**: In the target account's console, click "Switch Role" in the upper right
2. **Enter Details**: Provide the account number and role name from the ARN
3. **Visual Identification**: Choose a color to easily identify the assumed role
4. **Verification**: The console header will show the active role with the chosen color

### Step 3: Testing Permissions
- **Read Access**: The assumed role allows viewing EC2 instances
- **Write Restrictions**: Attempting to terminate instances is denied (as expected with read-only permissions)

### Key Components
- **Trust Relationship**: Defines which accounts can assume the role
- **Permission Policy**: Controls what actions the assumed role can perform
- **Cross-Account Access**: Enables secure resource sharing between AWS accounts

## IAM User Access Methods

An IAM user can access AWS services through two primary methods:

### AWS Management Console Access
- **Authentication**: Uses username and password credentials
- **Interface**: Browser-based graphical interface
- **Use Case**: Interactive management and configuration tasks

### Programmatic Access
- **Methods**: API calls, AWS CLI, AWS Tools for PowerShell, or AWS SDK
- **Credentials**: Access key ID and secret access key pair
- **Use Case**: Automation, scripting, and application integration

## AWS Command Line Interface (AWS CLI)

The AWS CLI is an open source tool that enables command-line interaction with AWS services. It provides functionality equivalent to the AWS Management Console through terminal commands.

### CLI Configuration Requirements

To configure AWS CLI using `aws configure`, you need:

1. **AWS Access Key ID**
2. **AWS Secret Access Key**
3. **Default region name**
4. **Default output format** (json, yaml, yaml-stream, text, table)

### Additional Resources
- [AWS Command Line Interface Documentation](https://docs.aws.amazon.com/cli/)

---

## Summary

This module covered IAM users, roles, and policies, demonstrating how to provide secure access to AWS resources. The next lesson focuses on implementing least privilege access through security policies.

# IAM Policies and Access Control

## Policy Types Overview

AWS policies manage access by attaching to IAM identities or AWS resources. Two main categories exist:

### Grant Permissions
- **Identity-based policies**: Attached to users, groups, or roles
- **Resource-based policies**: Attached to resources (e.g., S3 bucket policies)

### Set Maximum Permissions
- **IAM permissions boundaries**: Define maximum permissions for entities
- **AWS Organizations SCPs**: Set account-level permission limits

---

## Policy Types Details

### Identity-Based Policies
**Purpose**: Grant permissions to IAM identities (users, groups, roles)

**Types**:
- **Managed policies**: AWS managed or customer-managed
- **Inline policies**: Direct attachment to single identity (1:1 relationship)

### Resource-Based Policies
**Purpose**: Grant permissions to specified principals for resource access

**Examples**:
- S3 bucket policies
- IAM role trust policies

**Key Feature**: Principals can be in same or different accounts

### IAM Permissions Boundaries
**Purpose**: Set maximum permissions without granting permissions

**Function**: 
- Uses managed policy as boundary
- Entity can only perform actions allowed by BOTH identity policy AND boundary
- Creates intersection of permissions

### AWS Organizations SCPs
**Purpose**: Define maximum permissions for organization accounts/OUs

**Function**:
- Limit permissions from identity-based and resource-based policies
- Do not grant permissions themselves
- Apply to account members

### Access Control Lists (ACLs)
**Purpose**: Control cross-account principal access to resources

**Characteristics**:
- Similar to resource-based policies
- Only policy type not using JSON format

---

## Defense in Depth

**Strategy**: Multiple layers of security controls

**Application Layers**:
- Network edge
- VPC
- Load balancing
- Instances and compute services
- Operating systems
- Applications
- Code

**Policy Evaluation**: Multiple policies evaluated at different levels:
- User assumes role (identity policy)
- VPC endpoint policy
- Resource policy (e.g., S3 bucket policy)

---

## Policy Elements (JSON Structure)

| Element | Description | Required |
|---------|-------------|----------|
| **Effect** | Allow or Deny access | Yes |
| **Principal** | Account/user/role (resource-based only) | Resource-based |
| **Action** | List of allowed/denied actions | Yes |
| **Resource** | Resources affected by actions | Identity-based |
| **Condition** | Circumstances for policy application | Optional |

### Identity-Based Policy Example
```json
{
  "Version": "2012-10-17",
  "Effect": "Allow",
  "Action": ["s3:ListObject", "s3:GetObject"],
  "Resource": ["arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"]
}
```

### Resource-Based Policy Example
```json
{
  "Principal": "123456789012",
  "Effect": "Allow",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/folder123/*"
}
```

---

## Policy Evaluation Logic

### Default Behavior
- **Implicit Deny**: All requests denied by default

### Evaluation Process
1. **Check for Explicit Deny**: Overrides all allows
2. **Check for Explicit Allow**: Required for access
3. **Final Decision**: Deny if no explicit allow OR any explicit deny exists

### Policy Hierarchy
**Evaluation Order**:
1. Organizations SCPs
2. Permissions boundaries  
3. Identity-based policies
4. Resource-based policies
5. Role session policies

### Key Rules
- **Explicit Deny > Explicit Allow**
- **No Explicit Allow = Implicit Deny**
- **Any Explicit Deny = Final Deny**

---

## Example Scenarios

### Allow Policy
```json
{
  "Effect": "Allow",
  "Action": ["s3:ListObject", "s3:GetObject"],
  "Resource": ["arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"]
}
```

### Deny Policy
```json
{
  "Effect": "Deny",
  "Action": ["ec2:*", "s3:*"],
  "Resource": "*"
}
```

### Resource-Based S3 Example
- **ARN**: `arn:aws:s3:::DOC-EXAMPLE-BUCKET/folder123/*`
- **Effect**: Allows specific account to put objects in folder123
- **Wildcard**: Permits any objects under the prefix

---

## Next Steps

The following lesson covers managing multiple AWS accounts.

# Managing Multiple AWS Accounts

## Introduction

Next, we are going to talk about managing multiple accounts. 

In your organization, there probably are multiple AWS accounts. Maybe that happened organically. Maybe there's a reason for it. Let's look at some reasons why we would have multiple accounts and where AWS can help with multiple accounts.

---

## Why Use Multiple Accounts?

So maybe we are developing a multi-account strategy for different teams within our organization. Let's create an account for the developer team. Let's create an account for the security team, maybe, and an account for the accounting team who are going to be paying the bills.

### Key Reasons for Multiple Accounts

#### 1. Security and Compliance
The developer team, they don't need access to production data. There is customer data in our production systems. We don't want that data in the developer account. So let's make them separate accounts, and by default, developers do not have any access to that production data. That's a good security reason. That's a good compliance reason for separate accounts.

#### 2. Billing
For billing, for accounting purposes, we may want to know how much is the developer infrastructure costing us. If that's all in one account, it's very simple and easy to just look directly at that bill to see how much is developer infrastructure costing us, how much is production infrastructure costing us.

#### 3. Isolation
Isolation is another reason. We can use the same example of the developers and production. By default, different accounts are isolated from each other, and developers won't be able to see production data in there.

#### 4. Business Process
Also, business process reasons. The accounting team wants the ability to look at different accounts and just see how much resources are costing in different accounts. This is going to be refined and changed as your business needs evolved. It also happens that accounts pop up organically. Different teams within a large organization have gone and created accounts for their own teams. They probably have their reasons to do.

### Detailed Reasons to Use Multiple Accounts

**Many Teams**
When you have many teams, a multi-account strategy will enable you to group resources for categorization and discovery

**Security and Compliance Controls**
You can improve your security posture with a logical boundary created as a result of multi-account strategy

**Billing**
Will give you better cost insights

**Isolation**
You can limit potential impact in case of unauthorized access

**Business Process**
Will result in simplified management of user access to different environments

---

## Account Management: Before and After AWS Organizations

### Without AWS Organizations

We are about to introduce AWS Organizations, but let's think back to a time before we had it. I'm writing IAM policies for particular roles inside of my account number one. That's a very nice policy. I would love to be able to use it over on account number two. I cannot. IAM policies, the individual principals belong to a single account. Now I'm rewriting that same policy in different accounts. Policies to enforce restrictions must be managed within each account, and the generation of multiple bills is required. Each of those AWS accounts will be delivered a separate bill. For some customers, they like to see separate bills. For other customers, it's nice to roll that all up into one bill that an accounting team is paying for.

Managing multiple accounts is more challenging without Organizations. As an example, because IAM policies only apply to a specific AWS account, IAM policies must be duplicated and managed in each account to deploy standardized permissions across all accounts.

### With AWS Organizations

Now we introduce AWS Organizations. With AWS Organizations, we can create a hierarchy by grouping our accounts into OUs, or Organizational Units. I can apply policies at a higher layer. Previously, I was saying my policies can only be inside of one account. With AWS Organizations, I can write service control policies. Again, this is a concept of maximum permissions, and I can apply that to an OU. I've written a policy to say these accounts are only allowed to turn on EC2 instances in Australia, for example. That policy can be written at the OU level. It's defining a maximum permission that will apply to all of the accounts under the OU. There was an earlier slide where we said the root account can't be restricted in a single account mode. In AWS Organizations, yes, these service control policies will apply to the root user. I still go with the advice that we don't use our root user for day-to-day administration.

AWS Organizations includes account management and consolidated billing capabilities that enable you to better meet the budgetary, security, and compliance needs of your business. As an administrator of an organization, you can create accounts in your organization and invite existing accounts to join the organization. You can use service control policies (SCPs) to specify the maximum permissions for member accounts in the organization.

---

## AWS Organizations

AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage. You can group your accounts into organizational units (OUs) and attach different access policies to each OU.

### Key Features

- **Centralized management** of all your AWS accounts
- **Consolidated billing** for member accounts
- **Hierarchical grouping** of your accounts to meet your budgetary, security or compliance needs
- **Policies** to centralize control over the AWS services and API operations that each account can access
- **Integration and support** for IAM
- **Integration** with other AWS services

### Consolidated Billing

With AWS Organizations configured, we can take advantage of consolidated billing. The account at the very top of that tree, the management account, we can roll up billing to that one account. So now some accounting teams are going to be happier that only one bill is arriving for all of these different AWS accounts within one organization.

---

## Service Control Policies (SCPs)

### How IAM Policies Interact with SCPs

An SCP is a type of organization policy that you can use to manage permissions in your organization.

Attach identity-based or resource-based policies to IAM users, or to the resources in your organization's accounts. Attach an SCP to an Organizations entity (root, OU, or account) to define a guardrail. The SCP sets limits upon the actions that the IAM users and roles in the affected accounts can perform.

### Policy Intersection

IAM policies and service control policies, or SCPs. SCPs will allow the intersection of the IAM policy and the SCP. We spoke about a very similar concept earlier for our IAM policies. We're defining the maximum that's allowed. SCP says you're allowed to use EC2, you're allowed to use S3. Over on our identity-based policy, we're allowing EC2 and IAM. Only the intersection of that is allowed.

In the following graphic, the permissions allowed (center) are only the ones that are allowed both in the IAM identity-based permissions policy, and the Organizations SCP.

**Service Control Policy defines the organizational boundaries**
In this example, SCP allows all operations on EC2 instances and S3 buckets.

**IAM identity-based permissions allow principals associated with the policy to perform defined operations**
In this example, the principals associated with the current identity-based policy can perform all operations on EC2 instances and perform all IAM operations of creating users, defining roles and so on.

**The effective allowed operation** for a principal based on the SCP and identity-based policy is that they can perform all operations on EC2 instances

### SCP Use Cases

Another example of what I would use a service control policy might be limiting the services that a particular account can use, or we could write policies to limit the regions in which that account can operate.

---

## Layered Defense Example

Okay, in this example, we are using policies for layered defense. It's important to understand at what layer in that defense we are using each policy. So let's walk it through. I want to perform some action. Let's say I'm deleting an object out of an S3 bucket. So we start here. I'm using the API, I'm using the CLI tool, or I'm doing this from inside of the console. We start here. The initial filter is the service control policy. Is this included in my maximum permissions for the service control policy? Next, there is a permissions boundary. That is also a maximum permissions. Am I allowed to perform S3 delete objects? And then the identity-based policy. Have I got an allow statement in there to delete the S3 object? All three of them agree, and I hit the dartboard over there on that side. I'm allowed to delete the object.

---

## Best Practices

You should develop a multi-account strategy early on in your cloud deployment process and then refine it as your business evolves.

# Module 2: Account Security - Tech Talk
## Cross-Account Role Assumption Demo

### Introduction

Okay, we're in module two. We're talking about account security, and I thought Raf and I could get together, and let's assume a role in the console, and just see exactly what that looks like.

---

## Part 1: Creating the Cross-Account Role (Russ)

### Step 1: Get Account Information
**Russ**: Raf, I need to start with your account number. Can you please send it to me?

**Raf**: Sure, yeah. I'm going to send you over Slack. Done. You should have a message there.

**Russ**: Awesome, thank you, Raf.

### Step 2: Create the Role in AWS Console
**Russ**: Over now in the console, I'm going to go over here and create a role. It's asking me what is the trusted entity type for this role? And in this case, it's an AWS account. It's Raf's AWS account.

**Raf**: And that's why I sent you the number. Right?

**Russ**: Perfect.

**Raf**: Because you need that.

**Russ**: Yeah. And I can drop your account number in there. I'm not going to require an external ID or MFA. That's some additional security that we can apply to the role policy. And I click Next.

### Step 3: Attach Permissions Policy
**Russ**: Now it's asking me to attach a permission. And Raf, I'm going to give you EC2 read only.

**Raf**: EC2 read only, okay.

**Russ**: I don't want you to be touching anything inside my account.

**Raf**: Turning off instances, okay.

**Russ**: So let's look for that policy. I see it there. I'm gonna add the policy and hit Next. I need to give the role a name. Let's call it Raf EC2 read only. And I create the role.

### Step 4: Share Role ARN
**Russ**: That has created the role for me. I'm gonna come over here and view the role, and I grabbed the ARN, the Amazon resource name. And that's the only thing I need to give Raf. He's been given permission to the role. He's been given a policy on the role. Let's see what happens next. Raf, I'm sending you that roll ARN.

**Raf**: ARN, all right.

**Russ**: Right now.

---

## Part 2: Assuming the Role (Raf)

### Step 1: Switch Role in Console
**Raf**: Okay, so I want to assume that role. Let me copy that ARN, and let me go to my AWS management console and click on the upper right corner and choose Switch Role. That will ask for the account number and the role name. You sent me the ARN, so the account number is in the ARN, so I can copy it from there. Copy. And that's what I am going to put in the account field. And I will now copy the role name. Raf EC2 read only.

### Step 2: Customize Role Display
**Raf**: I can give it a color. I am going to choose the color. What color do you want?

**Russ**: I like that light blue color.

**Raf**: All right. This will be useful for me to identify in my console, because on the upper right corner now, I can see that very same color, so I can identify. And if I click there, currently active as Raf EC2 read only. Does that mean I am doing API calls on the EC2 for your account?

**Russ**: It looks like it. If you go into that list of instances there, I'll recognize if they're mine.

**Raf**: Okay. And I can describe them, because the role policy is EC2 read only.

---

## Part 3: Testing Role Permissions

### Successful Read Operation
**Russ**: That looks correct, you're in my account. Let's totally test this. What if you try and terminate one of my instances? I only gave you read only privilege.

**Raf**: Okay.

**Russ**: I'm gonna assume that shouldn't work.

### Failed Write Operation (Expected)
**Raf**: Oh yeah. So let me select the instance. Instance state, terminate instance. Yes.

**Russ**: Hey, you are not allowed to do it. That did exactly what we thought it would do.

**Raf**: Yes.

---

## Part 4: Technical Explanation

### Architecture Overview
**Russ**: Let's switch over to the tablet and just go over what it is exactly that we just created.

**Raf**: All right.

**Russ**: Okay, so I'm looking at it over here. We have two accounts. There is both my account and Raf's account. Inside of my account, I created a role. So a role has a trust relationship. That's where I said Raf's account will be allowed to assume this role. I attached one more thing, a policy. Raf, do you remember what that policy was called?

**Raf**: It's something that did not allow me to terminate the instance. Just read.

**Russ**: It was EC2 read only. That controlled what Raf was allowed to do once he had assumed the role. So I think that's a pretty good demo of how we assume roles.

### Demo Benefits
**Raf**: Yes, and it's interesting that we have two people here. So one part can play the one that creates the role, and the other one assumes the role, which is better than having the same instructor doing both and then requiring abstraction. Oh, this is the account that we will assume the role. So having clearly two accounts and two people works better for demonstrating that.

---

## Key Components Demonstrated

### 1. Cross-Account Role Creation
- **Trusted Entity**: Raf's AWS account number
- **Security Options**: External ID and MFA (optional, not used in demo)
- **Permissions Policy**: EC2ReadOnlyAccess
- **Role Name**: "Raf EC2 read only"

### 2. Role Assumption Process
- **Method**: Switch Role in AWS Console
- **Required Information**: Account number and role name (from ARN)
- **Visual Identification**: Color coding for easy console identification
- **Active Status**: Visible in upper-right corner of console

### 3. Permission Testing
- **Read Operations**: Successfully listed and described EC2 instances
- **Write Operations**: Terminate instance operation denied (as expected)
- **Policy Enforcement**: EC2ReadOnlyAccess policy working correctly

### 4. Architecture Elements
- **Two Accounts**: Russ's account (role creator) and Raf's account (role assumer)
- **Trust Relationship**: Defines which account can assume the role
- **Permissions Policy**: Controls what actions can be performed after assuming the role

---

## Conclusion

**Russ**: Okay, that's a wrap for our discussion.

This demonstration effectively showed the complete cross-account role assumption workflow, from creation to testing, highlighting both the security benefits and practical implementation of IAM roles across AWS accounts.

# Module 2: Account Security - Knowledge Check

## Introduction

Okay, it is knowledge check time for module number two. We're going to talk about account security, and this time, Raf, you're going to ask me some questions.

**Raf**: Okay, all right. Should I start?

**Russ**: Let's get started.

---

## Question 1: Policy Attachment

**Raf**: Question number one. Which of the following can be attached to a user, group or role? 

A. Resource-based policies  
B. AWS STS  
C. Security groups  
D. Identity-based policies

### Answer and Explanation

**Russ**: Okay, let me go through them. So what can be attached to a user, group, or role? A resource-based policy, I'm attaching to a resource like an S3 bucket or a dynamo table. AWS STS is the name of a service. I'm not attaching anything to that. Security groups I would associate with an ec2 instance, which leaves us with identity-based policies and I would consider user, groups, and roles a form of identity. So let's say the answer is D.

**Raf**: Can I reveal?

**Russ**: Sure thing.

**Raf**: You got it right.

**Answer: D - Identity-based policies**

---

## Question 2: Resource-Based Policies

**Raf**: Next question is, which of the following sets permissions on a specific resource and requires a principal to be listed in the policy? 

A. Identity-based policies  
B. Service control policies or SCPs  
C. Resource-based policies  
D. Permissions boundaries

### Answer and Explanation

**Russ**: Okay, I think I kind of explained two of these on the previous slide. Identity based policies were attached to users, groups, and roles. I'm going to exclude that. Service control policy is something that we use in AWS organizations. Let's jump over that one. And I see resource-based policies. I think this one matches what we're asking. That's a policy that we attach to something like an S3 bucket or a dynamo table. And permissions boundaries, permissions boundaries are attached to an identity like a policy is, but they set maximum permissions. It's not setting permissions, which would exclude that. Which gives us answer C. What do we think, Raf?

**Raf**: Let's check it out. And you are correct.

**Answer: C - Resource-based policies**

---

## Question 3: Programmatic Access Elements

**Raf**: The next one, which of the following are elements of an IAM user's programmatic access? And for this one, you need to select two answers. 

A. Username  
B. Access Key ID  
C. Password  
D. Secret Access Key  
E. MFA token

### Answer and Explanation

**Russ**: Okay, this is something I do a lot when I'm configuring the AWS CLI, so I'm pretty sure I know the answer, but let's go through it. A username? No, I wouldn't be using that for programmatic access. Access Key ID? Yes, that is part of it. C. a password, My IAM account can have a password associated as a form of authentication but I'd be using that to sign into the console. Secret access key, yes, and I always remind people, please let's keep them secret. I would say B and D so far. MFA token. An IAM user can require MFA token, but that's a different API call that I'd be supplying the MFA token to. So I'm going to go with B and D. What do we think?

**Raf**: I think you are correct. B and D are the correct answers for this one.

**Answer: B and D - Access Key ID and Secret Access Key**

---

## Question 4: Root User Best Practices

**Raf**: True or false: The root user should be used for daily administration tasks of your AWS account. What do you think?

### Answer and Explanation

**Russ**: Okay. This is one that, if I got it wrong, it would be pretty bad news. I remember presenting this slide. We do not use the root user for daily administration in an AWS account because it cannot be restricted. With organizations we can, but we really should be using a user that ties back to an individual. That's a much better security practice. So I'm gonna say false for that one.

**Raf**: You are absolutely correct.

**Answer: False**

**Explanation**: The root user should not be used for daily administration tasks because:
- It cannot be restricted in a single account
- Better security practice is using individual users
- Root user access should be limited and secured

---

## Question 5: AWS Organizations Exclusive Features

**Raf**: Question number five: Which of the following can only be managed with AWS organizations? 

A. Service control policies  
B. Resource-based policies  
C. Permissions boundaries  
D. Identity-based policies

### Answer and Explanation

**Russ**: Okay, Raf, so in AWS organizations, I'm creating like a hierarchy of AWS accounts. That first option there, service control policies, I know that we can use to restrict an OU, or a group of AWS accounts or an individual AWS account. That sounds right to me. Let's go through the rest. A resource based policy, this answer has come up a few times. No, I would be attaching that to an S3 bucket or a dynamo table, for example. Permissions boundaries. No, this gets attached to like a user or group or a role. It's a policy that defines maximum permissions. So does a service control policy, but I would be using the service control policy in AWS organizations. Identity-based policies, no. I would be attaching that to a user, group, or a role in IAM. So I'm gonna go with answer A for this one.

**Raf**: And if you chose answer A, you are correct.

**Answer: A - Service control policies**

**Explanation**: Service control policies (SCPs) are exclusive to AWS Organizations and are used to:
- Restrict organizational units (OUs)
- Control groups of AWS accounts
- Set maximum permissions at the organization level

---

## Module Completion

**Raf**: All right and that closes module number two: account security.

## Summary of Key Concepts

1. **Identity-based policies** attach to users, groups, and roles
2. **Resource-based policies** attach to resources and require principals
3. **Programmatic access** requires Access Key ID and Secret Access Key
4. **Root user** should not be used for daily administration
5. **Service control policies** are exclusive to AWS Organizations