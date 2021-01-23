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
* logical datacenter in AWS
* 5 VPCs per Region
* 200 Subnets per VPC
* 5 Elastic IP addresses (IPv4) per Region
* 5 Internet gateways per Region
* 200 Route tables per VPC
* 2500 VPC security groups per Region
* 50 Active VPC peering connections per VPC
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
* 5 NAT gateways per Availability Zone
* => must be in a public subnet
* => there must be a route out of the private subnet to the NAT instance to work
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
    * redundant inside the AZ
    * AWS managed service
    * bound to a single subnet
    * create a route with 0.0.0.0 (internet) destination and the Nat gateway as the target
    * throuput starts at 5Gbps and scale to 45Gbps
    * no security group
    * assigned a public IP address
    * AZ-independant architecture: create a NAT Gateway for each AZ and routing to ensure resources use the NAT in the same AZ

### BASTION
* inbound traffic: access to a private ec2 instance (private subnet) by forwarding the connection
* used to securely administrate instances
* HA 
    * network load balancer (level 4) with a static IP address forwarding traffic to multiple bastions accross multiples AZ (expensive $$$ !)
    * autoscalling with a minimum of 1 instance (downtime during the new server provisioning if a server is lost)
    * 

### NACL (Network Access Control Lists)
* must associate a subnet to only one NACL at a time
* associate to the default NACL on subnet creation (allow everything)
* can block IP address
* inbound & outbound rules
* rules are evaluate in chronological order
* 200 Network ACLs per VPC
* 20 Rules per network ACL
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
    * [tuto here](https://www.youtube.com/watch?v=dhpTTT6V1So&feature=youtu.be)

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
* 20 Gateway VPC endpoints per Region

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

### Cost model
* use private IP over public IP address as it utilize the AWS backbone network
* group EC2 instances into the same AZ and use private IP addresses to save cost /!\ less resilient /!\


## AWS WAF
* control http access to the content
* layer 7 of web firewall
* example: parse query string parameters
* allow IP addresses doing request
* 3 behaviors:
    * allow all request except the specified
    * block all request except the specified
    * count the request that match the properties you specify
* can filter on:
    * IP
    * countries
    * values in headers, query strings, etc.
    * lenght of request
    * SQL injection and XSS


## Elastic Load Balancer (ELB)
* can `handle X-forwarded-For` header for tracing the real referer of a request
* 504 when the app stops responding
* route request to a target group
* use sticky sessions to bind a user's session to a specific EC2 instance
* bound to a specific AZ (???)
* cross zone load balancing:
    * need to be enabled
    * ELB can send traffic to another AZ
* Path Pattern:
    * path based routing (eg. micro service context)
    * direct traffic to differents ec2 instances (eventually accross AZ) based on the URL
* Application Load Balancer
    * operate at layer 7 (http & https)
* Network Load Balancer
    * operate at layer 4 (TCP traffic)
    * when extreme performance is needed
* Classic Load Balancer (legacy)
    * application layer
* ??? check FAQ for quotas and metrics (eg. timeout)

## Route53


---
[Back](/solution-architect)
