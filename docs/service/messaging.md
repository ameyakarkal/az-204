# Azure based Messaging
## Azure Storage Queue
    - queue under storage account
    - number of messages per queue depends on size of storage account
    - per message size of 64 KiB
    - expiration / TTL (time to live) : configurable per message (default 7 days)
    - data redundancy : same as storage account (LRS|ZRS|GRS|GZRS|GA-GRS|GA-GZRS)
    - data encryption : encrypted by default
    - authentication :
        - shared key (primary | secondary)
        - SAS Token (generated off shared key)
        - Azure AD
    - visibility timeout
        - messages are consumed by the consumer
        - if the consumer does not say that it is DONE with the task the message gets AVAILABLE/VISIBLE again after visibility timeout has passed
        - configurable for a queue
    - limits
        - single accoount 20k messages per second
        - single queue 
            - message limit 2k messages per second
            - cannot exceed 500 TiB
        - single queue message 
            - cannot exceed 64 Kib

### when to choose storage queue 
- when size of the queue needs to be greater than 80GB
- log transactions 
- track progress of message


## Azure Service Bus Queue

- integrations
    - multiple modes
    - commons messaging service (JMS)
- structure 
    - namespace
        - queue
        - topic
            - multiple consumer
            - filter on consumer
- protocol
    - http/https/AMQP
- messaging modes
    - ordering
    - batching
    - dead letter queue
    - duplicate detection
- tiers
    - standard
        - pricing: pay as you go
        - scaling: auto scaling
        - resource: shared (underlying resources for the service are shared)
    - premium
        - pricing: fixed. determines scaling and throughput
        - scaling: rule based
        - resource: dedicated

### messaging properties
- set maximum queue size
- set batch size
- set default expiration of message
- set lock duration. similar to visibility time after which the message is visible on the queue if the consumer does not proess it
- set partitioning
- set session
- set dead letter queue

- FIFO
    - needs session feature to be enabled on queue on topic
- Partitioning
    - this is required for scaling Standard tier. Premium tier is handled differently.
    - each partition can have a separate data store and message broker
    - message needs to define partition key. in absence round robin method is used to choose a partition

### when to choose service bus queue
- receive messages without polling AMQP
- ordering of messages
- duplicate detection
- batching
- one produce with multiple consumers


:::mermaid
    flowchart LR;
    producer
    subgraph service
        subgraph namespace
            queue
            topic
        end
    end
    consumer
    producer-->queue
    queue--->consumer

    producer--->topic
    topic--->consumer_01
    topic--filter_01-->consumer_02
    topic--filter_02-->consumer_03
:::