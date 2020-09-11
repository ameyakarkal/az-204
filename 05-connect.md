# 05 : Connect

> Connect and consume azure services and third party services

## 5.1 : Develop service bus systems
- Durable datastore
- Structural Heirarchy
    1. namespace
        1.1 queue
        1.2 topic
            1.2.1 subscription 01
            1.2.2 subscription 02   
- Features
    1. multiple receivers
    2. dead letter queues
    3. message sessions, multiple messages can belong to saga / sessions
    4. request-response messaging : two way async communication
    5. message deferral / message scheduled queuing

### 5.1.1 : sdk
- older sdk uses JSON serialization for message
- newer sdk uses binary serialization, not compatible with sender/reciever of other version

### 5.1.2 : queue

- `QueueClient` used to send messages
- `QueueClient.RegisterMessageHandlers` used to receive messages / alternatively handle exceptions

### 5.1.3 topics

- `TopicClient` used to send messages
- `SubscriptionClient.RegisterMessageHandler` used to receive messages

### 5.2 : messages

message structure
    - header
        - size of 64k
        - message id
        - session id
        - label
        - user properties <key, value> pair
    - body
        - size of 256k / 1024k for premium


## Develop event based systems

- Pluralsight video : [Designing a Microsoft Azure Messaging Architecture](https://app.pluralsight.com/library/courses/microsoft-azure-messaging-architecture-designing/table-of-contents)