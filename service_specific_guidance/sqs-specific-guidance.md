
# Service-specific guidance: Amazon Simple Queue Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Queue Service (SQS). 

Amazon SQS is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS offers two types of message queues: standard queues for maximum throughput and at-least-once delivery, and FIFO queues for exactly-once processing and strict message ordering. It allows you to send, store, and receive messages between software components without losing messages or requiring other services to be available.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**List of service APIs reviewed against data perimeter control objectives**

* AddPermission
* ChangeMessageVisibility
* ChangeMessageVisibilityBatch
* CreateQueue
* DeleteMessage
* DeleteMessageBatch
* DeleteQueue
* GetQueueAttributes
* GetQueueUrl
* ListDeadLetterSourceQueues
* ListMessageMoveTasks
* ListQueueTags
* ListQueues
* PurgeQueue
* ReceiveMessage
* RemovePermission
* SendMessage
* SendMessageBatch
* SetQueueAttributes
* StartMessageMoveTask
* TagQueue
* UntagQueue
