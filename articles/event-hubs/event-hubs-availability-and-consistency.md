---
title: 可用性和一致性 - Azure 事件中心 | Microsoft Docs
description: 如何使用分区为 Azure 事件中心提供最大程度的可用性和一致性。
ms.topic: article
ms.date: 02/24/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 4ac4a9b19659fabbb8d63e2787732839c3fc62eb
ms.sourcegitcommit: 136164cd330eb9323fe21fd1856d5671b2f001de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196747"
---
# <a name="availability-and-consistency-in-event-hubs"></a>事件中心内的可用性和一致性
本文提供了有关 Azure 事件中心支持的可用性和一致性的信息。 

## <a name="availability"></a>可用性
Azure 事件中心分散了个别计算机发生灾难性故障的风险，甚至分散了跨数据中心内多个故障域的群集中整个机架发生灾难性故障的风险。 它实现了透明的故障检测和故障转移机制，使服务能够继续在有保证的服务级别运行，并且发生此类故障时，通常不会出现明显的中断。

当客户端应用程序将事件发送到事件中心时，事件会自动分布到事件中心内的各个分区中。 如果某个分区由于某种原因而不可用，则会将事件分布到其余分区中。 此行为可实现最大运行时间量。 对于要求保障最大正常运行时间的用例，此模型是首选模型，而不是将事件发送到特定分区。 有关详细信息，请参阅[分区](event-hubs-scalability.md#partitions)。

## <a name="consistency"></a>一致性
在某些方案中，事件的排序可能十分重要。 例如，可能希望后端系统先处理更新命令，再处理删除命令。 在此情况下，客户端应用程序将事件发送到某个特定分区，以便保留排序。 当使用者应用程序从该分区使用这些事件时，将按顺序读取它们。 

使用此配置，请记住，如果发送到的特定分区不可用，则会收到错误响应。 作为对比，如果未与单个分区关联，则事件中心服务会将事件发送到下一个可用分区。

确保排序的一个可能解决方法（同时还最大限度地延长运行时间）是将事件作为事件处理应用程序的一部分进行聚合。 实现此目的的最简单方法是使用自定义序号属性来标记事件。

在此情况下，生成者客户端将事件发送到事件中心内的一个可用分区，并从应用程序设置对应序号。 此解决方案要求处理应用程序保持状态，不过会为发送者提供更可能可用的终结点。

## <a name="appendix"></a>附录

### <a name="net-examples"></a>.NET 示例

#### <a name="send-events-to-a-specific-partition"></a>将事件发送到特定分区
在事件上设置分区键，或者使用 `PartitionSender` 对象（如果使用的是旧 Microsoft.Azure.Messaging 库）仅将事件发送到某个分区。 这样做可确保从分区读取这些事件时，按顺序读取它们。 

如果使用的是较新的 Azure.Messaging.EventHubs 库，请参阅[将代码从 PartitionSender 迁移到 EventHubProducerClient，以便将事件发布到分区](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md#migrating-code-from-partitionsender-to-eventhubproducerclient-for-publishing-events-to-a-partition)。

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs（5.0.0 或更高版本）](#tab/latest)

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    var batchOptions = new CreateBatchOptions() { PartitionId = "my-partition-id" };
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync(batchOptions);
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));
    
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs（4.1.0 或更早版本）](#tab/old)

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

var connectionStringBuilder = new EventHubsConnectionStringBuilder(connectionString){ EntityPath = eventHubName }; 
var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
PartitionSender partitionSender = eventHubClient.CreatePartitionSender("my-partition-id");
try
{
    EventDataBatch eventBatch = partitionSender.CreateBatch();
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));

    await partitionSender.SendAsync(eventBatch);
}
finally
{
    await partitionSender.CloseAsync();
    await eventHubClient.CloseAsync();
}
```

---

### <a name="set-a-sequence-number"></a>设置序号
下面的示例使用自定义序号属性来标记事件。 

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs（5.0.0 或更高版本）](#tab/latest)

```csharp
// create a producer client that you can use to send events to an event hub
await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    // get the latest sequence number from your application
    var sequenceNumber = GetNextSequenceNumber();

    // create a batch of events 
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync();

    // create a new EventData object by encoding a string as a byte array
    var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

    // set a custom sequence number property
    data.Properties.Add("SequenceNumber", sequenceNumber);

    // add events to the batch. An event is a represented by a collection of bytes and metadata. 
    eventBatch.TryAdd(data);

    // use the producer client to send the batch of events to the event hub
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs（4.1.0 或更早版本）](#tab/old)
```csharp
// Create an Event Hubs client
var client = new EventHubClient(connectionString, eventHubName);

//Create a producer to produce events
EventHubProducer producer = client.CreateProducer();

// Get the latest sequence number from your application 
var sequenceNumber = GetNextSequenceNumber();

// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);

// Send single message async
await producer.SendAsync(data);
```
---

此示例将事件发送到事件中心内的一个可用分区，并从应用程序设置对应序号。 此解决方案要求处理应用程序保持状态，不过会为发送者提供更可能可用的终结点。

## <a name="next-steps"></a>后续步骤
访问以下链接可以了解有关事件中心的详细信息：

* [事件中心服务概述](./event-hubs-about.md)
* [创建事件中心](event-hubs-create.md)
