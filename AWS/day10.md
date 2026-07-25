## What is a VPC?
- With Amazon Virtual Private Cloud (Amazon VPC), you can launch AWS resources in a logically isolated virtual network that you've defined.
- https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html

- A **Virtual Private Cloud (VPC)** is an isolated, logically separated virtual network in AWS where you can launch AWS resources (such as EC2 instances, RDS databases, and load balancers). It gives you full control over your network configuration, including IP addressing, subnets, routing, and security.

## Key Components of a Basic VPC

A minimal, commonly used VPC design contains the following components:

1. **VPC (CIDR Block)**
   - Example CIDR: `10.0.0.0/16`
   - This defines the private IP address range for the entire VPC.

2. **Subnets**
   - **Public Subnet**
     - Example CIDR: `10.0.1.0/24`
     - Has a route to the Internet Gateway, so resources in this subnet can have public internet access (typically via public IPs or Elastic IPs).
   - **Private Subnet** (optional in the most basic design but very common)
     - Example CIDR: `10.0.2.0/24`
     - No direct route to the internet; often used for databases or application servers.
