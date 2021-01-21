Application Integration
==============


## SNS

* topics
    * Standard Topics
    * FIFO Topics ( => FIFO subscriber/consumer)
* multiple destinations
    * filter & fanout events: SQS, Lambda, Event Fork Pipeline, Webhook 
    * push based (no polling)
* durability
    * cross AZ storage (replication)
    * Dead Letter Queue when retry fails
* encryption
    * (optional) encrypted topic with 256-bit AES-GCM algorithm with a customer master key (CMK) from KMS.
    * messages are stored stored in encrypted form and are decrypted when delivered
* privacy
    * use VPC endpoints and Privalink


## SQS
* 256KB max (= 1 request)
* stores message for 1mn to 14 days
* default retention period is 4 days
* visibility timeout
    * time allowed to the job who process the message
    * the message is not visible during this time
    * could be increased !
    * 12hours max
* long polling to reduce costs
* standard queues: 
    * unimited number of transaction
    * best effort for orderings and deliveries (at least one)
* FIFO queues
    * preserves ordering
    * exactly once processing
    * 300 op/s
    * message groups
    * pull based


## Lambda
* pricing 
    * number of request
    * duration; depends of the number of memory allocated
* serverless (≠ virtual machine like EC2 or RDS)
* scales out automatically
* lambda fun are independant
* debug with AWS X-ray
* globally service
* which services can trigger lambda ??? (eg. not RDS !)


## Simple Workflow Service (SWF)
* coordinate the work and the tasks (could be a manual process)
* workflow execution can last up 1 year
* task oriented API
* ensures a task is assigned only once
* keeps track of all the tasks and events in an application
* workflow:
    * workflow starters (initiate a wf)
    * deciders: control the flow of activity tasks and decide what to do after a task ends
    * activity workers (program of the application)
* replication (???)

## Elastic Transcoder
* media transcoder in the cloud
* provide transcoding presets for each format


## Api Gateway
* publish and maintain secure API at any scale
* front door for applications to access data
* serverless-ly connect to lambda & dynamoDB
* scale effortlessly
* throttle request to prevent attacks
* API caching
* XSS protection using CORS


## SAM
* cloudformation supports
* package & deploy
* test localy


## ECS
* create clusters to manage fleets of container deployments
* structure:
    * cluster
    * task definition (like a docker compose); can contain multiple container
    * container definition (memory, cpu, port mapping)
    * task / instance of a task: running copy of a task definition
    * service: allow tasks definition scaling by adding tasks
    * registry (eg. ECR)
* fargate:
    * serverless container engine
    * each workload runs in its own kernel
    * isolation and security
* choose ec2 for:
    * require broader customization
    * require GPU
    * has compliance requirements
* EKS: good choice when migrating from onprem to aws
* ECS + ELB
    * supports ALB(layer 7) / NLB/CLB (layer 4)
    * dynamic host port mapping
    * ALB is recommanded
* instance role: adding policy to all tasks running into a EC2 instance
* task role: apply policy per task (according to least privileges principles)

---
[Back](/solution-architect)
