COMPUTE
=========

* instance size
* instance attached storage (root volume / EBS / instance store)
* snapshots (point in time copy of a volume; incrementally stored on s3)
* duplicate an instance (eg. new AZ): snapshot > AMI > new based EC2 instance 
* data tranfer: snowball/snowmobile, AWS DataSync, EFS, …, Direct Connect
* placement groups (cluster / spread / partition)
* HPC
    * industries like genomic, finance, ML, weather prediction, automatic driving
    * enhance data transfer:
        * snowball/snowmobile
        * AWS DataSync
        * Direct Connect ([see NETWORK.md](NETWORK.md))
    * compute instances:
        * EC2 instances that are CPU or GPU optimized
        * EC2 fleets (???)
        * Cluster placement groups
    * enhanced networking:
        * Enhance networking single root I/O virtualization (SR-IOV)
        * ENA (Elastic Network Adapter)
        * Virtual Function /!\ legacy /!\ 
        * Elastic Fabric Adapters: bypass os kernel (Linux only)
    * storage
        * EBS: 64000 IOPS with provisionned IPS
        * instance store: scale to 1 million, low latency
    * orchestration
        * AWS ParallelCluster: automate creation of VPC, subnet, etc.
        * AWS batch
* HA architecture
    * design for failure
    * use multiple AZ and multiple region where ever we can
    * RDS: multi AZ (disaster recovery) ≠ read replica (speed up the read)
    * scaling up (vertical scaling) ≠ scaling out (horizontal scaling)
    * consider the cost element
    * difference storage S3 class

## EC2
* instance store / EBS backed
* ENI / ENA / EFA
    * ENI => secondary NETWORK
    * enhanced networking: single root io virtualization (SR-IOV) => higher IO performance, lowest CPU utilization
        * ENA => higher throughput
        * EFA => HPC, OS bypass
* spot fleets => can mix spot and on demande
* hibernate: < 60 days

### Spot instances & spot fleets
* for flexible and fault tolerant application
* can save up to 90% of the cost of on-demands
* can block Spot instances for being terminated using Spot block (set between 1 to 6h)
* lifecycle:
    * create a request (including max price & ec2 configuration)
        * one time: terminate when max price is reached
        * persistent: terminate when max price is reached, then recreated when the max price is above our max price
* spot fleets
    * collection of spot instances
    * may include on-demand instances
    * strategies:
        * lowestPrice (default strategy)
        * capacityOptimized
        * diversified: spots instances are distributed accross all pools
        * instancePoolsToUseCount: spot instances distributed accross the number of instance pool specied; use in combination with lowestPrice

### AMI
* based on region, OS, architecture, launch permissions and storage volume for the root device
* the root device is of 2 types:
    * Instance store (ephemeral storage)
        * created from a template stored in s3
        * restrict the range of instance type to choose
        * can not be stopped (terminate or reboot only)
        * /!\ can add volumes only on ec2 instance creation /!\
    * EBS backed volume
        * create from an AWS EBS snapshot

## Autoscaling
* 3 differents components:
    * groups:
        * logical component
        * webserver group | application group | database group | etc.
    * configuration templates
        * launch configuration for the EC2 instances
        * specify size, storage, instance type, AMI, key pair, etc.
    * scalling options (way to scale):
        * maintain current instance level at any time: fix a number of healthy instances
        * scale manually: specify the min and the max capacity of our autoscaling group and autoscaling will reduce/expand automatically
        * scale based on a schedule: usefull when we now exactly when to increase and decrease
        * scale based on demande:
            * use scaling policies
            * define parameters that define the scaling process, eg. a CPU threshold
        * use predictive scaling: based on the previous performance using the other scaling options

## Cloudformation
* scripting the cloud environment
* use CloudFormation templates built by AWS Solutions
* JSON/YAML based

## Elastic Beanstalk
* quickly deploy and manage applications in the AWS Cloud without worrying about the infrastructure that runs those applications. 
* reduces management complexity without restricting choice or control. 
* simply upload your application, and AWS Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring. 
* setup EC2, autoscalling, Load Balancer, Database


## AWS ParallelCluster
* cluster management tool that helps you to deploy and manage high performance computing (HPC) clusters in the AWS Cloud. 
* automatically sets up the required compute resources and shared filesystem. 
* You can use AWS ParallelCluster with batch schedulers, such as AWS Batch and Slurm
* facilitates quick start proof of concept deployments and production deployments
* build higher level workflows, such as a genomics portal that automates an entire DNA sequencing workflow


## On-prem strategy: AWS services that can be used on prem
* Database Migration Service
    * move data to and from aws
* Server Migration Service
    * incremental replication of on-prem services to AWS
    * backup strategy or multiple site strategy
* AWS Application discovery server
    * plan migration project
    * install AWS Application discovery Agentless connector as a VM on the on-rem DC
    * => will collect data about utilization
* VM Import/Export
    * migrate existing VM to EC2
    * DR strategy between DC and AWS
* Download Amazon Linux 2 as an ISO to run a EC2 instance on prem

---
[Back](/solution-architect)
