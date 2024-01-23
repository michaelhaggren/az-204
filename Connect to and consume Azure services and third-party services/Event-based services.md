
- [Azure API Management Documentation](https://docs.microsoft.com/en-us/azure/api-management/)
- [Azure Logic Apps Documentation](https://docs.microsoft.com/en-us/azure/logic-apps/)
- [Azure Event Grid Documentation](https://docs.microsoft.com/en-us/azure/event-grid/)
- [Azure Service Bus Documentation](https://docs.microsoft.com/en-us/azure/service-bus-messaging/)
## Event-based solutions

##### Azure Event Grid

**Purpose**: Event Grid is designed for event-driven architectures, focusing on reactive programming. It's about responding to events from various sources.
**Event Source**: Represents where the event is coming from. In Event Grid, these are referred to as "topics."
**Event Handler**: Represents where the event is being sent to for a reaction. In Event Grid, subscribers to topics are essentially the handlers.
**Event Receiver**: While not explicitly a term in Event Grid's documentation, any entity receiving events from Event Grid can be thought of as an event receiver. This might be an Azure Function, Logic App, or other service.

##### Azure Event Hubs

**Purpose**: Event Hubs is a big data streaming platform and event ingestion service. It can collect millions of events per second.
**Event Source**: The applications and services producing the events.
**Event Receiver**: In the context of Event Hubs, receivers actively pull events from the hub. These could be Azure Stream Analytics jobs, Azure Functions, or other services that process the incoming events.
**Event Handler**: Not a standard term in the context of Event Hubs. However, any processing or action done after receiving the event could be considered "handling" it.

##### Azure Service Bus

**Purpose**: Service Bus is a more general message broker service, offering message queues and publish-subscribe capabilities for complex communication needs.
**Event Source**: Not typically used in this context. Instead, we'd talk about message senders.
**Event Receiver**: Corresponds to the entities retrieving messages from the queue or subscription. In Service Bus, this is more about direct message processing rather than event-driven reactivity.
**Event Handler**: Again, not a standard term for Service Bus, but any processing or action after receiving the message could be considered "handling" it.

##### The difference between an Event and a Message

- An `event` is a lightweight notification that simply signals that something has happened. It's like saying, "Hey, this just occurred!" Those who are interested can react to the event, but there's no strict obligation to do so.

- A `message`, on the other hand, carries more detailed information and usually expects a specific action or response. It's like receiving a letter with instructions that you need to follow or tasks you need to complete.

So, in simpler terms:

- **Event**: "Something happened!"
- **Message**: "Here's information about something, and here's what you should do about it."

### Azure Event Hub

**RBAC Roles**

| Roles |  |
| ---- | ---- |
| Event Hub Data Owner | Use this role to give full access to Event Hubs resources. |
| Event Hub Data Sender | Use this role to give the send access to Event Hubs resources. |
| Event Hub Data Receiver | Use this role to give the consuming/receiving access to Event Hubs resources. |
| --- |  |
| **Other authorize access types** |  |
| Managed Identities |  |
| MSAL |  |
| Event hub publishers access with SAS |  |
| Event hub consumers access with SAS |  |

##### Scaling
**Auto-Inflate**: Can be enabled and basically handles the throughput-units automatically based on traffic demand for your Azure Event Hub.
*is not available for the Basic tier*

**Partition count and message retention configuration**
- Message retention lets you specify the time-to-live for your events. If they pass the set retention, after x days they will disappear, 1-7 days.
- Partition Count is a data organization mechanism and handles parallelism consuming of your events. Yo usually set this to a value that represents the amount of concurrent readers you expect to have.
- A Event-hub in the namespace has can be set to have minimum of 2 partitions and a maximum of  32.


##### Good-to-knows

- A Event-hub in the namespace has can be set to have minimum of 2 partitions and a maximum of  32.
- A Consumer group in a event hub instance can only be created in standard/premium tier.
#### Core Components of Azure Event Hubs:

1. **Event Producers**: This is the the source of the events. A producer send/publish events to the Event Hub. Examples include IoT devices, user activity on a website, or telemetry from software applications.
2. **Event Hub Namespace**: A container for one or more event hubs. It provides a unique FQDN (Fully Qualified Domain Name) for the event hubs inside it.
3. **Event Hub Consumer**: This is where the events are sent. It's a unique stream within the namespace.
5. **Partitions**: Each Event Hub contains one or more partitions. Events sent to the Event Hub are automatically distributed across the partitions.
6. **Consumer Groups**: For each partition, you can have multiple consumer groups. A consumer group is a view (state, position, or offset) of an entire event hub. Consumers read events from an event hub via a consumer group.
7. **Event Receivers**: These are the applications or services that process or handle the events. They read from a specific partition and consumer group.

![[Pasted image 20230820142429.png]]

### Azure Event Grid

**Built-in roles**

| Role                                | Description                                      |
| ----------------------------------- | ------------------------------------------------ |
| Event Grid Subscription Reader      | Authorized to read event subscriptions           |
| Event Grid Subscription Contributor | Allowed to manage event subscription operations. |
| Event Grid Contributor              | Can create & manage Event Grid resources                                                 |
| Event Grid Data Sender              |                                         Lets you send events to Event Grid topics         |

Azure Event Grid is marketed as a Pub-Sub message service. Its main job is to let different parts of Azure talk to each other by sending and receiving notifications about events. These events could be anything: a file got uploaded, a new user signed up, an error occurred, etc.

### Event Grid messaging Protocols

##### MQTT vs HTTP

**MQTT:**
Imagine you and your friends have walkie-talkies. You all agree that if someone says a special word, like "horror," everyone who wants to hear stories about horror will listen.

1. **Walkie-Talkie Channel**: This is like an MQTT "topic." If you want to hear horror, you tune into the horror channel.
2. **Telling Stories**: When someone tells a horror story on that channel, it's like "publishing" a message.
3. **Listening**: If you're tuned into the horror channel, you're "subscribed" and can hear the story.
4. **Main Friend**: There's one friend who makes sure the stories get to everyone. This friend is like the "broker" in MQTT.

**HTTP:**
Imagine you have a toy mailbox. Every day, you put a letter inside asking for a toy. The next day, you open the mailbox to see if there's a toy inside.

1. **Asking for a Toy**: When you put a letter in the mailbox, it's like sending an "HTTP request."
2. **Getting the Toy**: When you find a toy in the mailbox the next day, that's the "HTTP response."
3. **Toy Mailbox**: It doesn't remember if you asked for the same toy yesterday or last week. Each day is a new day. This is like HTTP being "stateless."

### How it relates to Azure Event Grid:

Azure Event Grid is like a magical toy that can use both the walkie-talkie and the toy mailbox. It can tell stories to your friends using the walkie-talkie (MQTT) and can also send and receive toys using the mailbox (HTTP). So, no matter how your friends like to play, Azure Event Grid can join in the fun!

### Core Concepts in Azure Event Grid:

1. **Event Sources**: These are the services or applications that originate or "produce" the events. It's like someone who would pin a notice onto our central bulletin board. In Azure, typical event sources include services like Azure Blob Storage (for file uploads) or Azure Resource Groups (for resource management tasks).
2. **Topics**: A topic is like a specific category on the bulletin board. When an event source has some information to share, it sends it to a specific topic. For example, all events related to file uploads in a storage account might go to a "File Uploads" topic.
3. **Event Subscriptions**: Once you have topics, you need a way for services or applications to express their interest in these topics. That's where subscriptions come in. A service "subscribes" to a topic to say, "Hey, I want to know when something gets posted here."
4. **Event Handlers**: After subscribing to a topic, you need to specify what should happen when an event arrives. This action is handled by an "event handler". For example, if the event is about a new file upload, the handler might be a function that processes that file.
5. **Filters**: Sometimes, a subscriber might not want to know about every single event in a topic, but only specific ones. Filters allow subscribers to specify criteria to receive only the events they're interested in. For instance, you might only want events about files larger than a certain size.

### Azure Service Bus

Azure Service Bus is a fully managed message broker that enables secure and reliable communication between systems and applications. It uses ==topics== to collect each message and allows services(typically microservice applications) to subscribe to these topics so that each time a new message ends up in a ==topic== it will automatically collect and process that message. This allows for automated handling for whenever a new message appears in the topic.

**Dead-letter queue** is an important feature that the Service Bus. The DLQ (Dead-letter Queue) will catch and save any message that could not be processed from the topic after x amount of tries (which can be configured) and you can then either try resending the message or see what the message consists of (i.e. what data etc. did this message consist of) to then analyze/debug why this message could not be sent.

**Message sessions**
Think of a message session like a chatroom where messages must be read in the order they came in. Only one person can read the messages at a time, ensuring nothing gets mixed up.

*Patterns*:
- `FIFO-pattern`: Handle messages in the first-in-first-out sequence.
- `Request-response pattern` Checks the header in the request to know who/where to send the response after the message has been handled.

#### Service Bus Queue Filters

| Filter Type | Description | Real-World Scenario | Behavior if Conditions Not Met |
| ---- | ---- | ---- | ---- |
| `SQL Filters` + `Boolean Filters` | Uses SQL-like syntax for complex conditions. Can perform arithmetic, logical, and relational operations. | Filtering messages in a logistics app by region and priority, e.g., `region = 'North' AND priority > 1`. Boolean filters for true/false conditions, like `isActive = true`. | Messages that don't meet the filter criteria are ignored by the subscription. They remain unprocessed in this context. |
| `Correlation Filters` | Matches specific properties in messages. Efficient for equality-based filtering without complex expressions. | In event-driven architecture, correlating messages using IDs for related events, or filtering messages based on specific labels or application-specific properties. | Similar to SQL filters, messages that don't match are ignored by the subscription and remain unprocessed. |
#### Event Grid vs Service Bus

##### Azure Event Grid:

1. **Purpose**: Azure Event Grid is designed for event-driven architectures. It's all about reacting to status changes.
2. **Event Model**: It deals with discrete event notifications. These events notify the system of state changes.
3. **Push Model**: Event Grid uses a push model, meaning it pushes the event to subscribers.
4. **Scalability**: Built for high-speed and dynamic event routing with filtering capabilities.
5. **Latency**: Typically offers low-latency delivery.
6. **Integration**: Deep integration with Azure services to generate or handle events, such as Azure Functions, Logic Apps, and more.
7. **Fan-out**: Supports multiple subscribers to a single event, allowing for broad dissemination of an event.

##### Azure Service Bus:

1. **Purpose**: Azure Service Bus is a general-purpose message broker. It's designed for high-value enterprise messaging and can act as a decoupler between different parts of a distributed application.
2. **Message Model**: Deals with messages, which can be simple data transfers or more complex with commands, events, and requests.
3. **Pull Model**: Service Bus typically uses a pull model, where receivers request messages from the Service Bus.
4. **Durability**: Messages can be stored durably and can be consumed in a deferred manner. This is useful for scenarios where order or guaranteed delivery is essential.
5. **Transactions**: Supports transactions, sessions, duplicate detection, and more.
6. **Patterns**: Supports a variety of communication patterns like point-to-point, publish-subscribe, and request-reply.
7. **Dead-lettering**: Provides a dead-letter queue for messages that can't be processed.

##### Why Both Exist:

While there's some overlap, the two services are optimized for different scenarios:

- **Event Grid** is ideal for scenarios where you want to react to status changes in real-time. For example, if a new file is uploaded to Azure Blob Storage, an event can trigger a function to process that file.
- **Service Bus** is more suitable for situations where you need reliable message delivery, order guarantees, or you're dealing with high-value messages. For instance, processing orders in an e-commerce application where you need to ensure every order is processed exactly once.

### Code examples

#### Methods and syntax for messages/queues/topics

**Allows for peeking a message without it being locked.**

```c#
var peekMessage = queue.PeekMessage(asd, aasda);
```

**Scheduling a message enables for it being enqueued at a later time.**

```c#
long seq = await send.ScheduleMessageAsync(message, DateTimeOffset.Now.AddDays(1));
```

```js
QueueClient queueClient = new QueueClient("serviceBusConnectionString", "queueName");

// To peek at the next message in the queue without removing it, use the PeekAsync method:
Message message = await queueClient.PeekAsync();

// You can then access the contents of the message by calling message.Body.
// For example, if the message body is a string, you can use:
string messageBody = Encoding.UTF8.GetString(message.Body);

// To receive and remove the next message in the queue, use the ReceiveAsync method:
Message message = await queueClient.ReceiveAsync();
// As with PeekAsync, you can access the contents of the message using message.Body.
```

