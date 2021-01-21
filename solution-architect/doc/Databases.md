Databases
=========

## RDS
* not serverless (except Aurora)
* choose EC2 instance
* can enable backup (auto with a retention day period) & snapshot (manual); using it will create a new DB !
* can enable multi AZ for disaster recovery (DR) as a failover solution (DNS are updated to the secondary instance)
* can enable autoscaling
* can enable encrypted at rest with KMS 
* read replica for scaling
    * max 5 read replica for MySQL
    * max 1 read replica for PG
    * could be in another region
    * read replica of a read replica can be managed asynchronously
    * can be promote as a main DB
    * require backup turned on
    * seconds async replication time
    * could be of type Aurora (usefull for service migration !)

### Aurora
* on-demand serverless service
* 5x perf over MySQL and 3x perf over PG
* starting 10GB
* autoscaling can reach 32vCPUs, 244GB memory & 64TB of storage
* 2 copies of the data in each AZ, with a minimum of 3 AZ => 6 copies of the data
* ms async replication time
* global database for multi region usage (???)
* at most 15 read replica
* in region replication (no cross region !)
* automatic backup by default, does not impact performances
* can take and share snapshot
* cost-effective solution for unpredictable workload
* can enable encryption at rest using KMS

## DynamoDB
* NoSQL
* consistent single digit millisecond latency
* document & key-value db
* ssd storage
* spread accross 3 geographically distinct zones
* eventual consistency by default
* can configure strongly consistent read if needed
* transactions
* encryption at rest using KMS
* snapshot & backups (same region as the source table)
* security: can use VPC endpoint
* can enable point in time recovery: restore data to any point in the last 35 days
* DynamoDB streams
    * stored for 24 hours
    * ordered sequence of events
    * global tables
        * multi-master, multi-region replication (for globally distributed applications)
        * multi region replication
        * DR & HA
* charges:
    * pay-per-request pricing
    * storage & backups
* Dax (DynamoDB Accelerator): 
    * 10x improvement performance (in-memory cache)
    * high availabilty: fail over to AZ
    * increase reads & writes

## Redshift
* data warehouse
* single node (160GB) or multi-node in a leader/compute node model
* advanced compression
* columnar format
* MPP
* automatic backup with 1 day retention period (max 35)
* 3 copies of the data (original, replica on the compute node, backup)
* DR handled by copying data to another region (S3)
* charges:
    * compute nodes
    * backup
    * data transfer (within a vpc)
* encrypted transit
* encrypted at rest (KMS, customer own key)
* available in only one AZ

## Elasticache
* web service for in-memory caching (redis & memcached)
* redis is multi AZ and can be backuped

## Database Migration Service
* migrate data between db (cloud & onprem)
* different types:
    * homogenous migration (oracle -> oracle)
    * hererogenous migration (oracle -> aurora) using Schema Conversion Tool (SCT)
* source type could be traditionnal rgbd, RDS, SAP, S3, azure db, mongodb, ...
* target type could be traditionnal rgbd (not aurora), redshift, kinesis data streams, elasticsearch, dynamodb, documentdb


---
[Back](/solution-architect)
