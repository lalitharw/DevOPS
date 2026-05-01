
# AWS ALB Setup

### VPC Creation and Configuration
```
1. Write the name of your VPC
2. Give IPv4 CIDR block. Note: The CIDR block size must be between /16 and /28. Better to use /16 in VPC.
```

### Create Internet Gateway and Attach to VPC
```
1. Give name to internet Gateway
2. After creating it will ask to attach VPC. From dropdown select your VPC.

An AWS Internet Gateway (IGW) is the mandatory bridge between your private AWS network (VPC) and the public internet. Think of it as the front door or secure bridge that allows your cloud servers (like EC2) to send data to the internet and receive requests from it. It is highly available, managed by AWS, and enables two-way traffic.
```

### Setting up public subnets
```
1. Select VPC ID
2. Give subnet the name
3. Select Availability Zone
4. Enter IPv4 Subnet CIDR Block
```


### Create Route Table and Assoicate with subnet
```
1. Select VPC
2. Click on subnet tab and select subnet associations and select subnets which were just created
3. Next Click Routes Tab, Edit Route, In destination make it 0.0.0.0/0 Target will be Internet Gateway

An AWS route table is a set of rules (routes) that acts as a traffic controller for your Virtual Private Cloud (VPC), directing network traffic from subnets to destinations like the internet, other VPCs, or gateways
```

### Launching an EC2 Instance


### Add EC2 in target Groups
```
1. Select VPC from the dropdown
2. And then select ec2 instances
```

### Setting Up Application Load Balancer
```
1. Add Listeners here
```
