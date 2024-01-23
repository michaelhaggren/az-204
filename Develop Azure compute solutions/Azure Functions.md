
- [Azure Functions Documentation](https://docs.microsoft.com/en-us/azure/azure-functions/)
- [Azure App Service Documentation](https://docs.microsoft.com/en-us/azure/app-service/)
- [Azure Container Instances Documentation](https://docs.microsoft.com/en-us/azure/container-instances/)

### Azure functions

#### What are Azure Functions?
Azure Functions is a serverless computing service offered by Microsoft Azure that enables you to run code on-demand without having to manage the underlying infrastructure. Azure Functions allows you to run code in response to events, such as a new image being added to a storage account, run at a specific time or an HTTP request to a REST API.

#### Azure Function features
1. `Custom Handler`: Enables running Azure Functions in any language or runtime not natively supported by Azure, current supported languages is Rust or Go.
2. `Extension Bundle`: A prepackaged set of Azure Function extensions for easy version management and deployment.
3. `Trigger`: The event source initiating the execution of an Azure Function.
4. `Runtime`: The execution environment's language version for your Azure Function.
5. `Policy`: Set of configurations for controlling Azure Function behavior and security.
6. `Hosting Plan`: The pricing and scaling model that determines the resource allocation for Azure Functions.

| Service | Binding Type | Common Trigger/Usage Method |
| ---- | ---- | ---- |
| Blob storage | Trigger, Input, Output | Triggered by changes in Blob Storage; reads/writes blobs |
| Azure Cosmos DB | Trigger, Input, Output | Triggered by changes in Cosmos DB; reads/writes documents |
| Azure Data Explorer | Input, Output | Processes data from/to Azure Data Explorer |
| Azure SQL | Trigger, Input, Output | Triggered by changes in SQL database; executes SQL queries |
| Event Grid | Trigger, Output | Triggered by events sent to Event Grid |
| Event Hubs | Trigger, Output | Triggered by events sent to Event Hubs |
| IoT Hub | Trigger, Output | Triggered by messages from IoT devices |
| HTTP | Trigger | Triggered by HTTP requests |
| Queue storage | Trigger, Output | Triggered by new messages in Azure Queue Storage |
| RabbitMQ | Trigger, Output | Triggered by messages in RabbitMQ queues |
| SendGrid | Output | Sends emails via SendGrid (typically used with another trigger like HTTP) |
| Service Bus | Trigger, Output | Triggered by messages in Azure Service Bus queues/topics |
| SignalR | Trigger, Input, Output | Handles real-time communication with SignalR |
| Table storage | Input, Output | Reads/writes data to Azure Table Storage |
| Timer | Trigger | Triggered based on a schedule (CRON expression) |
| Twilio | Output | Sends SMS or makes calls via Twilio (used with another trigger) |
| WebPubSub | Trigger | Handles real-time web-based pub/sub messaging |
function.json file for a WebPubSubTrigger function
```json
{
  "bindings": [
    {
      "type": "webPubSubTrigger",
      "direction": "in",
      "name": "message",
      "hub": "your_hub_name",
      "eventName": "your_event_name",
      "eventType": "system" // or "user"
    }
  ]
}
```
function.json for a EventGrid Function
```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "direction": "in",
      "name": "eventGridEvent"
    }
  ]
}

```

- **Trigger Bindings**: These are the points of entry for an Azure Function. A trigger binding defines how a function is invoked and what source of events it responds to. For example:
    - **HTTPTrigger**: The function is triggered by an HTTP request.
    - **TimerTrigger**: The function is triggered on a specified schedule.
    - **BlobTrigger**: The function is triggered when a blob is added or updated in Azure Blob Storage.
- **Input/Output Bindings**: Besides trigger bindings, there are input and output bindings, which don't trigger the function but are used to read data from or write data to various services. For example:
    - **Input Binding**: Used to read data from a service or resource. The function can process this data when it runs.
    - **Output Binding**: Used to write or output data to a service or resource after the function completes its execution.


_Example of a timer triggered Azure Function in C#_
```csharp
using System; 
using Microsoft.Azure.WebJobs; 
using Microsoft.Extensions.Logging; 

public static class TimerExample 
{ 
	[FunctionName("TimerExample")] 
	public static void Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log) 
	{ 
		log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}"); 				
	} 
}
```
_This function will run every 5 minutes. The `[TimerTrigger("0 */5 * * * *")]` attribute specifies the schedule on which the function will run. The schedule is specified in the CRON expression format._


Example of a Azure Function with both input/output binding.

```csharp
using System.IO;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

public static class ProcessBlobAndQueueOutput
{
    [FunctionName("ProcessBlobAndQueueOutput")]
    public static void Run(
        [BlobTrigger("sample-container/{name}")] Stream blobInput,
        [Queue("output-queue")] out string queueMessage,
        string name,
        ILogger log)
    {
        // Read the blob data
        using (StreamReader reader = new StreamReader(blobInput))
        {
            string blobContent = reader.ReadToEnd();

            // Log the blob content
            log.LogInformation($"Blob name: {name}, Content: {blobContent}");

            // Prepare a message for the queue
            queueMessage = $"Processed blob: {name}";
        }

        // The message is automatically sent to the specified Azure Queue
    }
}

```

#### Azure function trigger bindings

**Trigger binding** 
`WebPubSubStrigger` is used when you need to handle requests from the service side.
```json
{ 
"disabled": false, 
 "bindings": [
  { 
	  "type": "webPubSubTrigger", 
	  "direction": "in",
	  "name": "data",
	  "hub": "<hub>",
      "eventName": "message",
      "eventType": "user" } 
      ]
 }
```


#### Monitoring Azure Functions

**Scale controller logs**
`SCALE_CONTROLLER_LOGGING_ENABLED``: 


### Durable Functions
Durable Functions is an extension of Azure Functions that adds support for stateful, long-running, and reliable execution of functions. Durable Functions allows you to write functions that can be called, monitored, and managed as a single unit, even if the functions are composed of multiple steps or are long-running.

##### Query Durable Function Instances
These are properties you can use and reach for Durable Functions to get important information that is needed in the code logic, such as ``RuntimeStatus``, allowing for getting information about the state of your durable functions
- **Name**: The name of the orchestrator function.
- **InstanceId**: The instance ID of the orchestration (should be the same as the `instanceId` input).
- **CreatedTime**: The time at which the orchestrator function started running.
- **LastUpdatedTime**: The time at which the orchestration last checkpointed.
- **Input**: The input of the function as a JSON value. This field isn't populated if `showInput` is false.
- **CustomStatus**: Custom orchestration status in JSON format.
- **Output**: The output of the function as a JSON value (if the function has completed). If the orchestrator function failed, this property includes the failure details. If the orchestrator function was suspended or terminated, this property includes the reason for the suspension or termination (if any).
- **RuntimeStatus**: One of the following values:
    - **Pending**: The instance has been scheduled but has not yet started running.
    - **Running**: The instance has started running.
    - **Completed**: The instance has completed normally.
    - **ContinuedAsNew**: The instance has restarted itself with a new history. This state is a transient state.
    - **Failed**: The instance failed with an error.
    - **Terminated**: The instance was stopped abruptly.
    - **Suspended**: The instance was suspended and may be resumed at a later point in time.
- **History**: The execution history of the orchestration. This field is only populated if `showHistory` is set to `true`.

After starting new orchestration instances, you'll most likely need to query their runtime status to learn whether they are running, have completed, or have failed.

##### What is the main difference between a standard function and a durable function?
The main difference between a  _Standard Function_ and a *Durable Function* is that a _Durable Function_ is a specific type of Azure Function that is designed to be long-running, stateful, and scalable. _Standard Functions_ are meant to be short-lived and stateless, and are typically used to perform a specific task or operation in response to a specific trigger or event.

##### Durable Function Application Patterns
| Pattern           | Explanation                                                                                                                                                                                                                                                                         |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Function Chaining | Like following a recipe. You have a list of steps that you need to complete in a certain order to make a dish. In Durable Functions, you use this pattern to run a series of related functions that depend on each other.                                                           |
| Fan-out/Fan-in    | Like sending a message to a group of friends, and then they all respond back to you. In Durable Functions, you use this pattern to trigger multiple functions at the same time and then combine their results into a single response.                                               |
| Async HTTP API    | Like ordering food at a restaurant. You place your order with the waiter, and then you wait for the food to arrive. In Durable Functions, you use this pattern to create an HTTP API that allows you to trigger a long-running function and then receive a response when it's done. |
| Monitor           | Like watching a pot of water boil. You keep an eye on it, and when it starts boiling, you know it's ready. In Durable Functions, you use this pattern to monitor the progress of a long-running function and then take action when it's done.                                       |
| Human Interaction | Like getting help from a friend. You ask them for their opinion, and then you make a decision based on their input. In Durable Functions, you use this pattern to create a workflow that includes human interaction.                                                                |
#### Durable Function types

| Function Type | Description | Real-World Scenario Example |
| ---- | ---- | ---- |
| Orchestrator | Coordinates complex workflows and may call HTTP endpoints, activity, and entity functions. | Managing a user registration process that involves multiple steps and external API calls. |
| Activity | Performs tasks in a workflow, potentially calling external HTTP APIs if an HTTP response is not awaited. | Executing a database transaction or processing an image. |
| Entity | Manages state within orchestrations, allowing for operations on mutable state with concurrency controls. | Implementing a counter for the number of times a resource is accessed. |
| Client | Provides functionality to start, signal, query, and terminate orchestrations from external clients. | Triggering a data processing workflow from an external web application. |


Understanding the difference between Calling an entity vs Signaling. Basically calling an entity means you expect some sort of response, i.ea a two-way communication. But signaling an entity, is a one-way communication, indicating to update/change something, in this case signaling to add 1 if current value is less than 10.
```csharp
[FunctionName("CounterOrchestration")] public static async Task Run( [OrchestrationTrigger] IDurableOrchestrationContext context) { var entityId = new EntityId(nameof(Counter), "myCounter"); 
// Two-way call to the entity which returns a value - awaits the response 
int currentValue = await context.CallEntityAsync<int>(entityId, "Get"); 
   if (currentValue < 10) 
{ 
// One-way signal to the entity which updates the value - does not await a response 
context.SignalEntity(entityId, "Add", 1);
} 
																																  }
```

