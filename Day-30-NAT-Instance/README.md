📖 Overview
The Nautilus DevOps team needed to enable internet access for an EC2 instance running in a private subnet. The instance needed to upload a test file to a public S3 bucket. To minimize costs, a NAT Instance was used instead of a NAT Gateway.

## Objectives

1-Create a new public subnet datacenter-pub-subnet
2-Launch a NAT Instance datacenter-nat-instance in the public subnet
3-Configure iptables for NAT/masquerading
4-Disable Source/Destination check on NAT instance
5-Update private subnet route table to route via NAT
6-Verify datacenter-test.txt appears in S3 bucket



## Architecture
┌─────────────────────────────────────────────────────────────────────┐
│                        datacenter-priv-vpc                          │
│                                                                     │
│  ┌──────────────────────────────┐  ┌──────────────────────────────┐ │
│  │   Private Subnet             │  │   Public Subnet              │ │
│  │   datacenter-priv-subnet     │  │   datacenter-pub-subnet      │ │
│  │   (e.g. 10.0.1.0/24)        │  │   (e.g. 10.0.2.0/24)        │ │
│  │                              │  │                              │ │
│  │  ┌─────────────────────┐     │  │  ┌─────────────────────┐    │ │
│  │  │ datacenter-priv-ec2 │     │  │  │  datacenter-nat-    │    │ │
│  │  │  (cron job runs     │─────┼──┼─►│     instance        │    │ │
│  │  │   every minute)     │     │  │  │  (iptables NAT +    │    │ │
│  │  └─────────────────────┘     │  │  │   IP forwarding)    │    │ │
│  │                              │  │  └──────────┬──────────┘    │ │
│  │  Route: 0.0.0.0/0 →         │  │             │               │ │
│  │    NAT Instance              │  │  Route: 0.0.0.0/0 → IGW    │ │
│  └──────────────────────────────┘  └─────────────┼──────────────┘ │
│                                                   │                │
└───────────────────────────────────────────────────┼────────────────┘
                                                    │
                                         ┌──────────▼──────────┐
                                         │  Internet Gateway   │
                                         │   (datacenter-igw)  │
                                         └──────────┬──────────┘
                                                    │
                                               INTERNET
                                                    │
                                         ┌──────────▼──────────┐
                                         │  S3 Bucket          │
                                         │  datacenter-nat-    │
                                         │      19979          │
                                         └─────────────────────┘


   ## Step-by-Step Guide
Step 1: Gather Existing Environment Info
Log into the AWS Console and collect information about the existing environment.
Navigate to VPC Dashboard:

Go to VPC → Your VPCs
Find datacenter-priv-vpc
Note the VPC ID and CIDR block (e.g., 10.0.0.0/16)

Check existing subnet:

Go to Subnets
Find datacenter-priv-subnet
Note the Availability Zone (e.g., us-east-1a) and CIDR (e.g., 10.0.1.0/24)


You'll need this info to create the public subnet in the same AZ and with a non-overlapping CIDR.


 ## Step 2: Create Public Subnet

Go to VPC Dashboard → Subnets → Create subnet
Configure:

VPC ID:              datacenter-priv-vpc
Subnet name:         datacenter-pub-subnet
Availability Zone:   us-east-1a  (same as private subnet)
IPv4 CIDR block:     10.0.2.0/24  (non-overlapping with private subnet)

Click Create subnet 


 ## Step 3: Create & Attach Internet Gateway

Skip if an IGW already exists and is attached to the VPC.

Create IGW:

Go to VPC → Internet Gateways → Create internet gateway
Name: datacenter-igw
Click Create internet gateway

Attach to VPC:

Select the new IGW
Click Actions → Attach to VPC
Select datacenter-priv-vpc
Click Attach internet gateway 


 ## Step 4: Configure Route Table for Public Subnet
Create a new Route Table:

Go to VPC → Route Tables → Create route table
Configure:

Name:   datacenter-pub-rt
VPC:    datacenter-priv-vpc

Click Create route table

Add internet route:

Select datacenter-pub-rt → Routes tab → Edit routes
Click Add route:

Destination:  0.0.0.0/0
Target:       Internet Gateway → datacenter-igw

Click Save changes

Associate with public subnet:

Go to Subnet associations tab → Edit subnet associations
Check datacenter-pub-subnet
Click Save associations 


## Step 5: Create Security Group for NAT Instance

Go to EC2 → Security Groups → Create security group
Configure:

Name:         datacenter-nat-sg
Description:  Security group for NAT instance
VPC:          datacenter-priv-vpc
Inbound Rules:
TypeProtocolPort RangeSourceDescriptionAll trafficAllAll10.0.1.0/24 (private subnet CIDR)Allow from private subnetSSHTCP220.0.0.0/0 or Your IPManagement access
Outbound Rules:

Keep default: Allow all outbound traffic


Click Create security group 


## Step 6: Launch NAT Instance

Go to EC2 → Instances → Launch instances

Basic Settings:
Name:           datacenter-nat-instance
AMI:            Amazon Linux 2023 AMI (64-bit x86)
Instance type:  t2.micro  (or t3.micro)
Key pair:       Select existing or create new
Network Settings (click Edit):
VPC:                    datacenter-priv-vpc
Subnet:                 datacenter-pub-subnet
Auto-assign public IP:  Enable 
Security group:         datacenter-nat-sg (existing)

##  User Data :
#!/bin/bash
# Update system packages
yum update -y

# Install iptables-services (NOT installed by default on Amazon Linux 2023)
yum install -y iptables-services

# Enable IP forwarding at the kernel level
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p

# Configure iptables MASQUERADE rule for NAT
# This rewrites the source IP of outgoing packets to the NAT instance's IP
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Allow forwarded traffic
iptables -F FORWARD

# Save iptables rules to persist across reboots
service iptables save

# Enable and start iptables service
systemctl enable iptables
systemctl start iptables

echo "NAT Instance configuration complete!"


## Step 7: Update Private Subnet Route Table
Direct all internet-bound traffic from the private subnet through the NAT instance.

Go to VPC → Route Tables
Find the route table associated with datacenter-priv-subnet
Select it → Routes tab → Edit routes
Click Add route:

Destination:  0.0.0.0/0
Target:       Instance → datacenter-nat-instance

Click Save changes 


 The private EC2's outbound internet traffic will now be routed through the NAT instance.


 ## Step 8: Verify NAT Configuration

SSH into the NAT instance to confirm the setup is correct.
bash# SSH into NAT instance (using its public IP)
ssh -i your-key.pem ec2-user@<NAT-Instance-Public-IP>
Verify IP forwarding is enabled:
bashcat /proc/sys/net/ipv4/ip_forward
# Expected output: 1
