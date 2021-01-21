Analytics
==========

## EMR
* hadoop on AWS
* cluster of EC2 instances (nodes)
    * master node: manage the cluster, track tasks status and monitor the cluster health
    * core node: contains software that runs tasks; eventually contains file acting as HDFS
    * task node: software components run tasks
* can store logs to s3 (configure on cluster creation only)


## Kinesis
### Kinesis Streams
* 24 hours retention; increase to 7 days
* data contained in shards
* read 5 transactions/s per shard, for a maximum total data of 2MB
* 1000 records/s to write (1MB)

### Kinesis Firehose
* no persistent storage
* producers -> processing -> output (eg. s3, elasticsearch cluster, ...)

### Kinesis Analytics
* analyse data on the fly inside the service
* store the data on s3, redshift, elasticsearch...

---
[Back](/solution-architect)
