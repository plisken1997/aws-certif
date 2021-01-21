NETWORK
==========

* NAT
    * how to give internet access to a non public subnet ?
    * NAT Instance (EC2) VS NAT Gateway
* Bastion
* AMAZON VPN
    * VPN cloud hub
    * direct connect
    * direct connect Gateway
    * transit connect Gateway
* Global Axelerator


## VPC
* consists of: 
    * Internet Gateways / virtual Private Gateways (one IGW per vpc ; not created on vpc creation)
    * route tables
    * Network Access Control Lists (NACL)
    * securiy groups
    * Subnets (one subnet = one AZ)
* VPC peering
    * connect VPC to another via direct network route using private IP address (same or another account)
    * instances behave as they are in the same VPC
    * peering in a star schema => No transitive peering !
    * can peer accross regions
* aws reserved 5 IP addresses:
    * x.x.x.0: network address
    * x.x.x.1: for the vpc router
    * x.x.x.2: reserved by AWS
    * x.x.x.3: reserved by AWS
    * x.x.x.255: network broadcast address
* a security group is bound to only one VPC

### NAT (Network Adrress Translation)
* outbound traffic: enable a private EC2 instance (private subnet) to access internet by allowing to communicate with the IGW
* NAT instances
    * individual EC2 instance 
    * "source/destination check" ec2 option must be disabled
    * run in a public subnet
    * has a public IP address
    * bridge to the private subnet
    * create a route out of the private subnet with 0.0.0.0 (internet) destination and the Nat EC2 instance as the target (identified by its ENI)
    * /!\ as it is a single instance, it could easily ends as a spof
    * a route pointing to a terminated Nat has a "blackhole" status
* NAT gateways
    * highly available spread accross multi AZ (many instances)
    * AWS managed service
    * bound to a single subnet
    * create a route with 0.0.0.0 (internet) destination and the Nat gateway as the target
    * throuput starts at 5Gbps and scale to 45Gbps
    * no security group
    * assigned a public IP address
    * AZ-independant architecture: create a NAT Gateway for each AZ and routing to ensure resources use the NAT in the same AZ

### BASTION
* inbound traffic: access to a private ec2 instance (private subnet) by forwarding the connection

### NACL (Network Access Control Lists)
* must associate a subnet to one NACL
* associate to the default NACL by default (allow everything)
* inbound & outbound rules
* rules are evaluate in chronological order
* ephemeral port (???)

### ELB
* at least 2 public subnets

### VPC FlowLogs
* inside cloudwatch logs or a S3 bucket
* capture the information about the traffic that is going into the vpc
* 3 levels:
    * VPC
    * subnet
    * Network Interface
* only available for our aws account vpcs
* IAM roles can't be changed
* some traffic is not monitored:
    * aws DNS
    * windows instances
    * 169.254.169.254 (user metadata)
    * DHCP traffic
    * reserved IP addresses

### Direct Connect
* dedicated network solution between onprem and AWS
* dedicated link from the data center to routers to AWS backbone network
* high throughput network
* Set up a Direct Connect:
    * create a virtual interface in the Direct Connect console => this is a public interface
    * create a Customer Gateway (CG) from the VPC console
    * create a Virtual Private Gateway (VPG)
    * attach the Virtual Private Gateway to the desired VPC
    * create a new VPN connection
        * select the VPG and the CG
        * once the VPN is available, set up the VPN on the Customer Gateway

### Global Accelerator
* improve the availability of the app
* Global Accelerator gives 2 static IP addresses; user can bring its own address
* proper DNS name & network zone with its own infrastructure (similar to an AZ)
* directs traffic to optimal endpoint
* endpoint group is associated to an AWS region
* good for performance testing & blue green deployment
* endpoint: AWS resource, eg. an EC2 isntance
* control traffic using traffic dials (done in the endpoint group): define the percentage of the traffic that would be directed to the endpoint group

### VPC Endpoints
* privately connect a VPC to supported AWS services and VPC endpoint services powered by Private Link
* connect a VPC to AWS services without using IGW, Route, etc.
* traffic does not leave the Amazon network
* Interface Endpoints: ENI with a private IP address that serces as an entry point to supported services
* Gateway endpoint: for S3 and DynamoDB (specify region for s3)

### AWS Private Link
* opening a service in a VPC to another VPC without open up the VPC to the internet or VPC peering
* requires a Network Load Balancer on the service VPC and an Elastic Network Interface (ENI) on the customer VPC
* no route tables, no NAT instances, no Gateway...
* a good way to peer VPC to a lots of customer VPC

### AWS Transit Gateway
* allow to have a transitive peering between a thousands of VPCs and on-premises data centers
* hub and spoke model
* works on a region model basis but can be accross multiple regions
* use it accross multiple AWS accounts using Resources Access Manager (RAM)
* use route tables to limit how VPC talks to one another
* use to simplify network topology (eg. supports IP multicast)

### VPN Hub
* single point of contact to connect VPN architecture
* operates on public internet
* good way to manage multiple sites VPN (low cost)

---
[Back](/solution-architect)
