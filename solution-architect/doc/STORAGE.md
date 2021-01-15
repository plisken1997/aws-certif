STORAGE
=========

* store data 
    * version
    * lifecycle
    * replication
    * access (ACL, policies) & action (lock policies)
    * security & encryption
* share data
    * public access
    * accross multiple aws account
    * on premise to AWS 
 
 ### EBS
 * AZ replication ???
 * instance store: clear on stop
 * 1GiB to 16 TiB
 * same AZ as the EC2 instance
 * encryption 
    * done with default key or KMS
    * at rest AND in transit AND all snapshot based on
    * envelope encryption
    * store key with the volume metadata
    * store the key on EC2 memory when esb is mounted (never persisted)
    * copy and unencrypted snapshot to a encrypted one will result of a full copy (not an incremental one)
 * delete a volume: should be in an available state
 * volume options allow to be modified:
    * change instance type (gp2 <-> sc1)
    * increase volume size
    * increase/decrease provisioned IOPS
    * use cloudwatch & lambda to automatically manage these options !
    * increase the volume size using OS commands (linux) to extends the volume's fs
 * pricing:
    * volume (amount of GB provisioned per month)
    * throughput for provisionned IOps SSD (io1)
    * snapshot size (s3 charge)
 * ssd: 
    * IOps
    * transactional workload
    * low latency
    * optimized for small size of random IO
    * frequent read/write ops
    * 2 types: general purpose (gp2) & provisioned iops (io1)
    * use case: relational DB, NoSQL
    * which volume ? 
        * IOps or low latency => ec2 instance store
        * performance => io1
        * cost => gp2
 * hdd:
    * large sequential IO, large streaming workload 
    * throughput instead of IO
    * sequential workload (backup, transactional log write, …)
    * 2 types: throuput optimized (st1) & cold hdd (sc1)
    * use case: big data (streaming, analytics workload), file/media
    * which volume ?
        * < 1.7 MB/s: D2 / H1
        * performance: st1
        * cost: sc1
 * uncertain workload: use gp2
 * managing performances:
    * IO characteristics (IO / throughput)
    * EC2 instance
    * volume (EBS) instance
    * initialization: read all the block of the volume before using it to make them accessible
    * increase read-ahead throughput to 1MB for read heavey workload (HDD volumes) (accessing data through OS page cache)
    * RAID for better IO performance (not for replication !)
 * ebs optimized instance: dedicated network bandwith (instead of non ec2-optimized which use a shared network)
 * Data lifecycle Manager (DLM): schedule and create the lifecycle (creation & deletion) of the snapshots; no additional cost (pay for the cost of the snapshot)


 ### NFS
 * One per VPC
 * needs security group
 * creation workflow: region > VPC + (subnet + security group)
 * encryption at rest (specify when mounting)
 * accessible on prem using NFS client
 * 2 storage class
 * bursting (scale as the FS grows) and provisioned throughput mode; 24h delay when changing throughput mode
 * general purpose / max I/O performance mode (can't be changed)
 * security:
    * network => security groups & network ACL
    * files & directory access => linux & file level permissions (POSIX)
    * grant administrative access => IAM 
    * encrypt data at rest => eg. KMS
    * encrypt in transit => TLS on mount option
    * restrict NFS client access => IAM for NFS client
    * manage application specifi access => Access points
 * allow in/out bound traffic on port 2049

### S3

### MACIE

### Cloudfront

### DataSync

### Storage Gateway

### AWS Organizations

### EFS
* NFS
* read after write consistency
* pay as you go
* FSx for windows => SMB server
* FSx Lustre => HPC (high speed, high capacity distributed storage, ML, video encoding, big data); can store data directly on S3


---
[Back](/solution-architect)
