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