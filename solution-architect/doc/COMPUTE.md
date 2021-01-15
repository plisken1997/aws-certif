COMPUTE
=========

* instance size
* instance attached storage (root volume / EBS / instance store)
* snapshots (point in time copy of a volume; incrementally stored on s3)
* duplicate an instance (eg. new AZ): snapshot > AMI > new based EC2 instance 
* data tranfer: snowball/snowmobile, AWS DataSync, EFS, …, Direct Connect
* placement groups (cluster / spread / partition)

### EC2
* instance store / EBS backed
* ENI / ENA / EFA
    * ENI => secondary NETWORK
    * enhanced networking: single root io virtualization (SR-IOV) => higher IO performance, lowest CPU utilization
        * ENA => higher throughput
        * EFA => HPC, OS bypass
* spot fleets => can mix spot and on demande
* hibernate: < 60 days


---
[Back](/solution-architect)
