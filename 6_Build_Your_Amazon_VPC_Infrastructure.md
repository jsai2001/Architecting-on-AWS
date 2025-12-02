# Build Your Amazon VPC Infrastructure - Lab Guide

## Lab Overview

This lab demonstrates creating a VPC with both public and private subnets, providing flexibility to launch tasks and services in either environment. Private subnet resources can access the internet through a NAT gateway while remaining isolated from direct internet access.

---

## Session Manager Connection Verification

### Connecting to Private Instance
1. Close the Session Manager tab and return to the console
2. Verify successful connection to private instance using Session Manager

**Result**: Successfully connected to a private instance using Session Manager without requiring direct internet access or SSH keys.

---

## Optional Task 1: Troubleshooting Connectivity Between Instances

### Purpose
Use Internet Control Message Protocol (ICMP) to validate network reachability between public and private instances.

### Steps

#### 1. Prepare Private Instance Information
1. Navigate to **EC2 Console** → **Instances**
2. Select **Private Instance**
3. On **Details tab**, copy the **Private IPv4 address**
4. Note the private IP for later use

#### 2. Connect to Public Instance
1. Unselect Private Instance
2. Select **Public Instance**
3. Choose **Connect** → **Session Manager** → **Connect**
4. New browser tab opens with connection to Public Instance

#### 3. Test Web Application Connectivity
**Command**: Replace `PRIVATE_IP` with actual private IPv4 address
```bash
curl PRIVATE_IP
```

**Expected Output**: HTML content from web application
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<title>Amazon EC2 InstanceA</title>
<link rel="stylesheet" href="css/screen.css" type="text/css" media="screen" title="default" />
</head>
<body>
<div id="content-outer">
<center>
<div id="content">
<table border="0" width="50%" cellpadding="0" cellspacing="0" id="content-table">
<tr>
<th rowspan="3" class="sized"><img src="images/shared/side_shadowleft.jpg" width="20" height="300" alt="" /></th>
<th class="topleft"></th>
<td id="tbl-border-top">&nbsp;</td>
<th class="topright"></th>
<th rowspan="3" class="sized"><img src="images/shared/side_shadowright.jpg" width="20" height="300" alt="" /></th>
</tr>
<tr>
<td id="tbl-border-left"></td>
<td>
<div id="content-table-inner">
<div id="table-content">
<center>
<img src="images/reinvent_mark_white.png" width="300"/>
<br/>
<br/>
<h2>EC2 Instance ID: i-01a5b999bd67645cf</h2>
<h2>Zone: us-east-1a</h2>
</center>
</div>
<div class="clear"></div>
</div>
</td>
<td id="tbl-border-right"></td>
</tr>
<tr>
<th class="sized bottomleft"></th>
<td id="tbl-border-bottom">&nbsp;</td>
<th class="sized bottomright"></th>
</tr>
</table>
<div class="clear">&nbsp;</div>
</div>
</center>
<div class="clear">&nbsp;</div>
</div>
<div class="clear">&nbsp;</div>
</body>
</html>
```

#### 4. Test ICMP Connectivity
**Command**: Replace `PRIVATE_IP` with actual private IPv4 address
```bash
ping PRIVATE_IP
```

**Example Command**:
```bash
ping 10.0.2.131
```

**Expected Result**: Ping request fails initially due to security group configuration

**Action**: Stop ping request with `CTRL+C` after a few seconds

### Troubleshooting Challenge
**Problem**: Ping to private instance fails
**Task**: Determine correct inbound rule for Private Security Group to allow successful ping

---

## Optional Task 2: Retrieving Instance Metadata

### Purpose
Learn to retrieve instance metadata using AWS CLI and cURL commands from within running EC2 instances.

### Steps

#### 1. Connect to Public Instance
1. Return to **AWS Management Console**
2. Navigate to **EC2** → **Instances**
3. Select **Public Instance**
4. Choose **Connect** → **Session Manager** → **Connect**

#### 2. Retrieve All Metadata Categories
**Command**: Get authentication token and list all metadata categories
```bash
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` \
&& curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/
```

**Purpose**: 
- Creates authentication token for metadata service
- Lists all available metadata categories
- Uses IMDSv2 (Instance Metadata Service version 2) for enhanced security

#### 3. Retrieve Specific Metadata Item
**Command**: Get public hostname using the token from previous command
```bash
curl http://169.254.169.254/latest/meta-data/public-hostname -H "X-aws-ec2-metadata-token: $TOKEN"
```

**Important Notes**:
- IP address `169.254.169.254` is a link-local address
- Only valid from within the instance
- Provides access to instance-specific information
- Useful for scripting and automation within instances

### Instance Metadata Use Cases
- **Dynamic configuration**: Retrieve instance-specific settings
- **Automation scripts**: Get instance details for processing
- **Service discovery**: Obtain network and placement information
- **Security**: Access IAM role credentials (when assigned)

---

## Optional Task Solution

### Problem Resolution: Enable ICMP Ping
When ping fails between public and private instances, the issue is typically security group configuration.

#### Steps to Fix ICMP Connectivity
1. **Navigate to Security Groups**
   - AWS Management Console → Search "EC2" → Security Groups

2. **Edit Private Security Group**
   - Select **Private SG**
   - Choose **Actions** → **Edit inbound rules**

3. **Add ICMP Rule**
   - Choose **Add rule**
   - **Type**: Custom ICMP - IPv4
   - **Source**: Custom
   - **Source Value**: Type "sg" and select **Public SG** from list
   - Choose **Save rules**

4. **Test Connectivity**
   - Return to Optional Task 1
   - Re-run ping command
   - Public Instance should now successfully ping Private Instance

### Why This Works
- **Security Group Reference**: Using `sg-xxxxx` (Public SG) as source
- **ICMP Protocol**: Allows ping/echo requests
- **Principle of Least Privilege**: Only allows traffic from specific security group
- **Bidirectional**: Security groups are stateful (return traffic automatically allowed)

---

## Lab Conclusion

### Achievements
✅ **Created Amazon VPC** with custom network configuration
✅ **Created public and private subnets** for different access patterns
✅ **Created internet gateway** for public internet access
✅ **Configured route tables** and associated with appropriate subnets
✅ **Created EC2 instances** with public accessibility
✅ **Isolated EC2 instance** in private subnet for security
✅ **Created and assigned security groups** for traffic control
✅ **Connected using Session Manager** for secure access without SSH

### Key Learning Points

#### VPC Architecture Benefits
- **Flexibility**: Launch resources in public or private environments
- **Security**: Private subnets provide isolation from internet
- **Connectivity**: NAT gateway enables outbound internet access for private resources
- **Control**: Security groups and NACLs provide layered security

#### Session Manager Advantages
- **No SSH keys required**: Eliminates key management overhead
- **No bastion hosts needed**: Direct secure access to private instances
- **Audit trail**: All sessions logged for compliance
- **Browser-based**: No additional client software required

#### Security Group Best Practices
- **Stateful firewall**: Return traffic automatically allowed
- **Reference other security groups**: Use SG IDs instead of IP ranges
- **Principle of least privilege**: Only allow necessary traffic
- **Regular audits**: Review and update rules periodically

---

## Additional Resources

### AWS Documentation
- [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Subnets for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html)
- [Connect to the internet using an internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- [Configure route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Control traffic to resources using security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
- [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [Public IPv4 addresses](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html)
- [Understanding the basics of IPv6 networking on AWS](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ipv6.html)

### Training Resources
- [AWS Training and Certification](https://aws.amazon.com/training/)
- [AWS Training and Certification Contact Form](https://aws.amazon.com/training/contact-form/)

---

## Lab Cleanup

### End Lab Process
1. **Return to AWS Management Console**
2. **Sign out**: Upper-right corner → AWSLabsUser → Sign out
3. **End Lab**: Choose "End Lab" and confirm termination
4. **Confirmation**: Verify lab resources are cleaned up

### Important Notes
- All lab resources are automatically cleaned up
- No manual resource deletion required
- Lab environment is temporary and will be terminated
- Save any important information before ending lab