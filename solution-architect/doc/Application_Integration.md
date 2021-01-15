Application Integration
==============

### SNS

* topics
    * Standard Topics
    * FIFO Topics ( => FIFO subscriber/consumer)
* multiple destinations
    * filter & fanout events: SQS, Lambda, Event Fork Pipeline, Webhook 
* durability
    * cross AZ storage (replication)
    * Dead Letter Queue when retry fails
* encryption
    * (optional) encrypted topic with 256-bit AES-GCM algorithm with a customer master key (CMK) from KMS.
    * messages are stored stored in encrypted form and are decrypted when delivered
* privacy
    * use VPC endpoints and Privalink


---
[Back](/solution-architect)
