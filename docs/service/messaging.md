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
    - single account 20k messages per second
    - single queue 
        - message limit 2k messages per second
        - cannot exceed 500 TiB
    - single queue message 
        - cannot exceed 64 Kib

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

- protocol
    - http/https/AMQP
- tiers (at the namespace level)
    - basic
        - no topics. just queue
    - standard
        - pricing: pay as you go 
        - throughput : variable
        - message size : 256kb
        - resilency : no geo redundancy or zoning
        - scaling: auto scaling
        - resource: shared (underlying resources for the service are shared)
    - premium
        - pricing: fixed. determines scaling and throughput
        - throughput : based on messaging unit
        - scaling: rule based
        - resource: dedicated
        - resilency : is geo redundant and zoning
- messaging modes
    - ordering
    - batching
    - dead letter queue
    - duplicate detection

### messaging properties
- set maximum queue size
- set batch size
- set default expiration of message
- set lock duration. similar to visibility time after which the message is visible on the queue if the consumer does not proess it
- set partitioning
- set session
- set dead letter queue

### message behavior
- FIFO
    - needs session feature to be enabled on queue or topic
    - additional properies on the message like groupid or messageid
- Partitioning
    - Standard tier: SUPPORTED
    - Premium tier : NOT SUPPORTED | since messages are handled differently on this tier
    - each partition can have a separate data store and message broker
    - message needs to define partition key. in absence round robin method is used to choose a partition
- Dead Letter Queue
    - capture messages that were not processed
    - 
## Azure Storage Queue vs Azure Service Bus Queue
- azure storage queue
    - total message above 80GB 
    - logs against all messages need to be executed
    - track progress of message processing
- azure service bus
    - receive messages without polling AMQP
    - ordering of messages
    - duplicate detection
    - batching producing and consuming messages
    - one produce with multiple consumers
