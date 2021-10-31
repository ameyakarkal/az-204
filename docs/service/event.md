## Reference 
[Microsoft Azure Developer : Develop Event-based Solutions](https://app.pluralsight.com/library/courses/microsoft-azure-developer-develop-event-based-solutions/table-of-contents)

Event Grid : for app. discrete . report changes
Event Hub : for app. series based. time series
Notification Hub : for users

# Event Grid
- publisher / subscribers
    - publisher can be azure service or custom
    - multiple subscribers to a publisher
    - filter events on subscribers
    - highly scalable
    - serverless
- provider needs to be enabled at the subscription
- terminology
    - event : event that hapened
    - publisher : creator of event
        - too many to define
        - can create custom event hub publishers
    - topics : collection of event
        - application subscribe to topics
        - filters are applied on subscription
    - handlers : consumers listening to a topic
        - azure function
        - event hubs
        - service bus
        - storage queue
        - webhook
        - applications that listen to these messages can handle the events

### system grid
1. create event grid system topic : handle events of azure service. e.g : blob service    
2. add subscription
    1. pick the types of event you want to subscribe (blob created / updated)
    2. add filters (file extensions)
    3. create azure function

### custom grid
1. create event grid topic
2. create subscription
3. azure function / webhook listens to subcription  

# Event Hub

- used for time series
- telemetry
- data archival
- transaction processing  
- anomoly detection

- structure
    - namespace
        - producers : send data to event hub
        - partitions : buckets of messages
            - multiple partitions
        - consumer group : view of the event hub
            - multiple consumer groups per event hub
            - each consumer group is a view of the same event hub
            - it has a current position associated to it
        - subscribers : application reading events from the event hub
            - can process events of the same event hub at its pace independent of the other consumer groups. allowing multiple applications read the same data at different throughput 
- namespace : scoping container. there can one or more event hubs in a namespace
- throughput set at the namespace level
- partitions 
    - max of 32 partition
    - number of partitions (ideally) matches to number of consumer
- message
    - messages are placed in partition in order of arrival
    - not deleted till the time to live
    - partition key can be used to specify partition
    - size limit of 1mb per batch of messages

### create event hub
```bash
# create event hub namespace
az eventhubs namespace create \
--name 'namespace-name' \
--location 'location' \
--resource-group 'group' \
--sku Standard

# create event hub under namespace
az eventhubs eventhub create \
--name 'eventhub-name' \
--namespace 'namespace-name' \
--resource-group 'group' \
--partition-count 'p' \ # number of partitions 2-32. default=4
--message-retention 'r' # number of days message is retained. max = 7 days
```

### sending data

```c#
/* Producer */
// event batch
var batch = EventDataBatch();

// add to batch
items
.Select(i => new EventData(JsonSerializer.UTF8Bytes(i)))
.ForEach(i => {
    if(!batch.TryAdd(i)) Console.WriteLine(i, 'NOT added');
});

// send with options
await ProducerClient
    .SendAsync(batch, new EventSendOptions { PartitionKey  = "partition_01"});

/* Consumer */

```

### sending and receiving data
- producer
    - uses `EventHubProducerClient` to connect to the event hub using 
        -  connection string
        - event hub name
    - create `EventHubDataBatch`
    - create `EventData` using UTF8Bytes of the object to be sent.
    - adds `EventData` to `EventHubDataBatch` which ensures that 1mb limit is maintained
    - `EventHubProducerClient` sends batch with `EventSendOption` which can be used to specify partition
- consumer
    - uses `EventHubConsumerClient` using 
        - consumer group name
        - connection string to namespace
        - event hub name
    - uses `PartitionInfo` to get the current position of the consumer group
    - uses cancelToken to stop listening to the event hub or else it will listen to the hub

# Notification Hubs

- structure
    - namespace
        - notification hub (usually one per env)
- namespace
    - billing per namespace
    - mutliple hubs
- scalable
- platform notification service  (PNS): 
    - these are the actual notification services that send out the notification
    - ANH communicates with PNS
    - e.g : apple | android
    
- timeline
    - register a device:  
        - setup Installation with (sent via device)
            - InstallationId
            - PushChannel
            - Platform
            - Template : template message notification
    - send message
        - send template notification. with the actual value dictionary