## Reference 
[Microsoft Azure Developer : Develop Event-based Solutions](https://app.pluralsight.com/library/courses/microsoft-azure-developer-develop-event-based-solutions/table-of-contents)

Event Grid : for app. discrete . report changes
Event Hub : for app. series based. time series
Notification Hub : for users

## Event Grid
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