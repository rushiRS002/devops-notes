## vpc 
With Amazon Virtual Private Cloud (Amazon VPC), you can launch AWS resources in a logically isolated virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS.
- https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
  

## subnet 
Subnet basics
Each subnet must reside entirely within one Availability Zone and cannot span zones. By launching AWS resources in separate Availability Zones, you can protect your applications from the failure of a single Availability Zone.
- https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html

## nat 
- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html
 NAT instance provides network address translation (NAT). You can use a NAT instance to allow resources in a private subnet to communicate with destinations outside the virtual private cloud (VPC), such as the internet or an on-premises network. The resources in the private subnet can initiate outbound IPv4 traffic to the internet, but they can't receive inbound traffic initiated on the internet.

## IGW 
- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html
Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet.
- An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. It supports IPv4 and IPv6 traffic. It does not cause availability risks or bandwidth constraints on your network traffic.
- https://medium.com/awesome-cloud/aws-vpc-difference-between-internet-gateway-and-nat-gateway-c9177e710af6

## Route table 
- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html
A route table serves as the traffic controller for your virtual private cloud (VPC). Each route table contains a set of rules, called routes, that determine where network traffic from your subnet or gateway is directed. When you create a VPC, we also create the main route table for the VPC. You can create additional route tables for your VPC, so that you have more granular control over the network paths for your VPC.

## Lab [link](./EC2/lab-2)

---

## ARN 
```
arn:partition:service:region:account-id:resource-type:resource-id
arn:aws:ec2:ap-northeast-1:905690002273:instance/i-0afcf6c6c11f281ae
```









