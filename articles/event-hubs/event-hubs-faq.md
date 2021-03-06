---
title: 常见问题 - Azure 事件中心 | Microsoft Docs
description: 本文提供了有关 Azure 事件中心的常见问题 (FAQ) 和解答的列表。
ms.topic: article
origin.date: 10/27/2020
ms.date: 12/02/2020
ms.author: v-tawe
ms.openlocfilehash: cb2c34bcb2b4b441564ed567ad0a3540ed92ee13
ms.sourcegitcommit: f436acd1e2a0108918a6d2ee9a1aac88827d6e37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509081"
---
# <a name="event-hubs-frequently-asked-questions"></a>事件中心常见问题

## <a name="general"></a>常规

### <a name="what-is-an-event-hubs-namespace"></a>什么是事件中心命名空间？
命名空间是事件中心/Kafka 主题的范围容器。 它提供唯一的 FQDN。 命名空间充当容装多个事件中心/Kafka 主题的应用程序容器。 

### <a name="when-do-i-create-a-new-namespace-vs-use-an-existing-namespace"></a>何时创建新的命名空间与使用现有命名空间？
容量分配（[吞吐量单位 (TU)](#throughput-units)）在命名空间级别进行计费。 命名空间也与区域相关联。

在以下任一情况下，你可能希望创建新的命名空间，而不是使用现有的命名空间： 

- 需要与新区域关联的事件中心。
- 需要与其他订阅关联的事件中心。
- 需要一个具有不同容量分配的事件中心（即，具有添加的事件中心的命名空间的容量需求将超过 40 TU 阈值，并且你不希望使用专用群集）  

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>事件中心基本和标准这两种服务层有什么不同？

Azure 事件中心标准层提供的功能超出了基本层中提供的功能。 以下功能是标准层附带的：

* 更长的事件保留期
* 其他中转连接，对于超出包含的数量的部分收取超额费用
* 多于单个[使用者组](event-hubs-features.md#consumer-groups)
* [捕获](event-hubs-capture-overview.md)
* [Kafka 集成](event-hubs-for-kafka-ecosystem-overview.md)

有关定价层的详细信息（包括专用事件中心），请参阅[事件中心定价详细信息](https://www.azure.cn/pricing/details/event-hubs/)。

### <a name="where-is-azure-event-hubs-available"></a>Azure 事件中心在哪些区域可用？

在所有支持的 Azure 区域中都可使用 Azure 事件中心。 有关列表，请访问 [Azure 区域](https://www.azure.cn/home/features/products-by-region)页。  

### <a name="can-i-use-a-single-advanced-message-queuing-protocol-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>是否可以使用单个高级消息队列协议 (AMQP) 连接来与多个事件中心相互收发数据？

可以，但前提是所有事件中心都在同一个命名空间中。

### <a name="what-is-the-maximum-retention-period-for-events"></a>事件的最长保留期有多久？

事件中心标准版目前支持的最长保留期为七天。 事件中心不应作为永久性的数据存储。 大于 24 小时的保留期适用于将事件流重播到相同系统中的情形。 例如，基于现有数据训练或验证新机器学习模型。 如果需要将消息保留七天以上，请启用事件中心的[事件中心捕获](event-hubs-capture-overview.md)功能，将数据从事件中心提取到所选的存储帐户或 Azure Data Lake 服务帐户。 启用捕获功能需要支付费用，具体因购买的吞吐量单位而异。

可以在存储帐户上配置已捕获数据的保留期。 Azure 存储的“生命周期管理”功能为常规用途 v2 和 blob 存储帐户提供了基于规则的丰富策略。 可使用该策略将数据转移到适当的访问层，或在数据的生命周期结束时使数据过期。 有关详细信息，请参阅[管理 Azure Blob 存储生命周期](../storage/blobs/storage-lifecycle-management-concepts.md)。 

### <a name="how-do-i-monitor-my-event-hubs"></a>如何监视事件中心？
事件中心向 [Azure Monitor](../azure-monitor/overview.md) 发出详尽指标用于提供资源的状态。 此外，参考指标不仅可以在命名空间级别，而且还能在实体级别评估事件中心服务的总体运行状况。 了解 [Azure 事件中心](event-hubs-metrics-azure-monitor.md)提供哪些监视功能。

### <a name="where-does-azure-event-hubs-store-customer-data"></a><a name="in-region-data-residency"></a>Azure 事件中心将客户数据存储在何处？
Azure 事件中心将存储客户数据。 事件中心会自动将此数据存储在单个区域中，因此此服务会自动满足区域内数据驻留要求，包括[信任中心](https://azuredatacentermap.azurewebsites.net/)内指定的要求。

[!INCLUDE [event-hubs-connectivity](../../includes/event-hubs-connectivity.md)]

## <a name="apache-kafka-integration"></a>Apache Kafka 集成

### <a name="how-do-i-integrate-my-existing-kafka-application-with-event-hubs"></a>如何将现有的 Kafka 应用程序与事件中心集成？
事件中心提供可由基于 Apache Kafka 的现有应用程序使用的 Kafka 终结点。 只需完成一项配置更改，即可获得 PaaS Kafka 体验。 使用该体验就如同运行自己的 Kafka 群集。 事件中心支持 Apache Kafka 1.0 和更高版本的客户端，并且适用于现有的 Kafka 应用程序、工具和框架。 有关详细信息，请参阅[适用于 Kafka 的事件中心存储库](https://github.com/Azure/azure-event-hubs-for-kafka)。

### <a name="what-configuration-changes-need-to-be-done-for-my-existing-application-to-talk-to-event-hubs"></a>要使现有的应用程序与事件中心通信，需要完成哪些配置更改？
要连接到事件中心，需要更新 Kafka 客户端配置。 为此，可以创建事件中心命名空间并获取[连接字符串](event-hubs-get-connection-string.md)。 更改 bootstrap.servers，以将事件中心 FQDN 和端口指向 9093。 更新 sasl.jaas.config，以使用正确的身份验证将 Kafka 客户端定向到事件中心终结点（已获取的连接字符串），如下所示：

```properties
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

例如：

```properties
bootstrap.servers=dummynamespace.servicebus.chinacloudapi.cn:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://dummynamespace.servicebus.chinacloudapi.cn/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXX";
```
注意：如果 sasl.jaas.config 在框架中不是受支持的配置，请查找用于设置 SASL 用户名和密码的配置，改为使用这些配置。 将用户名设置为 $ConnectionString，将密码设置为事件中心连接字符串。

### <a name="what-is-the-messageevent-size-for-event-hubs"></a>事件中心的消息/事件大小是多少？
事件中心允许的最大消息大小为 1 MB。

## <a name="throughput-units"></a>吞吐量单位

### <a name="what-are-event-hubs-throughput-units"></a>什么是事件中心吞吐量单元？
事件中心内的吞吐量定义通过事件中心传入和传出的数据量，以 MB 或者 1-KB 事件的数目（以千计）为单位。 此吞吐量以吞吐量单位 (TU) 来计量。 在开始使用事件中心服务之前，需购买 TU。 可以使用门户或事件中心资源管理器模板显式选择事件中心 TU。 


### <a name="do-throughput-units-apply-to-all-event-hubs-in-a-namespace"></a>吞吐量单位是否适用于命名空间中的所有事件中心？
是的，吞吐量单位 (TU) 适用于事件中心命名空间中的所有事件中心。 这意味着，TU 是在命名空间级别购买的，并在该命名空间下的事件中心之间共享。 每个 TU 为命名空间赋予以下功能：

- 入口事件（发送到事件中心的事件）最多为每秒 1 MB，但每秒不超过 1000 个入口事件、管理操作或控制 API 调用。
- 出口事件（从事件中心使用的事件）最多达每秒 2 MB，但不超过 4096 个。
- 事件存储空间最多为 84 GB（对于默认的 24 小时保留期而言已足够）。

### <a name="how-are-throughput-units-billed"></a>吞吐量单位如何计费？
吞吐量单位 (TU) 按小时计费。 计费基于在给定的小时选择的最大单位数目。 

### <a name="how-can-i-optimize-the-usage-on-my-throughput-units"></a>如何优化吞吐量单位的使用？
可以从最少的一个吞吐量单位 (TU) 着手，并启用[自动扩充](event-hubs-auto-inflate.md)。 自动扩充功能可让你在流量/有效负载增大时扩展 TU。 还可以针对 TU 数量设置上限。

### <a name="how-does-auto-inflate-feature-of-event-hubs-work"></a>事件中心自动扩充功能的工作原理是什么？
使用自动扩充功能可以纵向扩展吞吐量单位 (TU)。 这意味着，一开始可以购买较低的 TU，自动扩充会在入口流量增大时纵向扩展 TU。 自动扩充是一种经济高效的选项，可让你完全控制 TU 数量。 这是一个 **仅限纵向扩展** 的功能，可以通过更新来完全控制 TU 数量的缩减。 

可以从较低的吞吐量单位 (TU) 着手，例如 2 个 TU。 如果预测流量可能会增大到 15 TU，请对命名空间启用自动扩充功能，并将最大限制设置为 15 TU。 现在，当流量增大时可以自动扩展 TU。

### <a name="is-there-a-cost-associated-when-i-turn-on-the-auto-inflate-feature"></a>启用自动扩充功能是否会产生相关的费用？
此功能 **不会** 产生相关的费用。 

### <a name="how-are-throughput-limits-enforced"></a>如何强制实施吞吐量限制？
如果某个命名空间中所有事件中心间的总入口吞吐量或总入口事件率超过了聚合吞吐量单位限额，那么发送方会受到限制，并会收到指明已超出入口配额的错误信息。

如果某个命名空间中所有事件中心间的总出口吞吐量或总出口事件率超过了聚合吞吐量单位限额，那么接收方会受到限制，但不会生成任何限制错误。 

入口和出口配额是分开强制实施的，因此，任何发送方都不会使事件耗用速度减慢，并且接收方也无法阻止事件发送到事件中心。

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-reservedselected"></a>可预留/选择的吞吐量单位数量是否有限制？

在 Azure 门户中创建基本层或标准层命名空间时，最多可以为命名空间选择 20 个 TU。 若要将 TU 提升到正好 40 个，请提交一个[支持请求](http://support.azure.cn)。  

1. 在“事件总线命名空间”页面上，选择左侧菜单上的“新建支持请求” 。 
1. 在“新建支持请求”页面上，按照以下步骤操作：
    1. 对于“摘要”，用几句话描述这个问题。 
    1. 对于“问题类型”，请选择“配额”。 
    1. 对于“问题子类型”，选择“吞吐量单位增加或减少请求” 。 
    
        :::image type="content" source="./media/event-hubs-faq/support-request-throughput-units.png" alt-text="“支持请求”页面":::

如果超出 40 TU，事件中心可提供名为“事件中心专用群集”的基于资源/容量的模型。 专用群集按容量单位 (CU) 销售。
<!-- ## Dedicated clusters

### What are Event Hubs Dedicated clusters?
Event Hubs Dedicated clusters offer single-tenant deployments for customers with most demanding requirements. This offering builds a capacity-based cluster that is not bound by throughput units. It means that you could use the cluster to ingest and stream your data as dictated by the CPU and memory usage of the cluster. For more information, see [Event Hubs Dedicated clusters](event-hubs-dedicated-overview.md).

### How do I create an Event Hubs Dedicated cluster?
For step-by-step instructions and more information on setting up an Event Hubs dedicated cluster, see the [Quickstart: Create a dedicated Event Hubs cluster using Azure portal](event-hubs-dedicated-cluster-create-portal.md). 


[!INCLUDE [event-hubs-dedicated-clusters-faq](../../includes/event-hubs-dedicated-clusters-faq.md)] -->


## <a name="partitions"></a>分区

### <a name="how-many-partitions-do-i-need"></a>需要多少分区？
分区数在创建时指定，并且必须介于 1 和 32 之间。 分区计数不可更改，因此在设置分区计数时应考虑长期规模。 分区是一种数据组织机制，与使用方应用程序中所需的下游并行度相关。 事件中心的分区数与预期会有的并发读取者数直接相关。 有关分区的详细信息，请参阅[分区](event-hubs-features.md#partitions)。

你可能希望在创建时将其设置为最高可能值，即 32。 请记住，拥有多个分区将导致事件发送到多个分区而不保留顺序，除非你将发送方配置为仅发送到 32 个分区中的一个分区，剩下的 31 个分区是冗余分区。 在前一种情况下，必须跨所有 32 个分区读取事件。 在后一种情况下，除了必须在事件处理器主机上进行额外配置外，没有明显的额外成本。

事件中心设计用于允许每个用户组使用单个分区读取器。 在大多数用例中，四个分区的默认设置就足够了。 如果希望扩展事件处理，则可以考虑添加其他分区。 对分区没有特定的吞吐量限制，但是命名空间中的聚合吞吐量受吞吐量单位数限制。 增加命名空间中吞吐量单位的数量时，可能需要添加额外分区来允许并发读取器实现其自身的最大吞吐量。

但是，如果有一个模型，其中应用程序具有到特定分区的关联性，则增加分区数可能对你没有任何益处。 有关详细信息，请参阅[可用性和一致性](event-hubs-availability-and-consistency.md)。

### <a name="increase-partitions"></a>增加分区数
通过提交支持请求，你可以请求将分区计数增加到 40（精确）。 

1. 在“事件总线命名空间”页面上，选择左侧菜单上的“新建支持请求” 。 
1. 在“新建支持请求”页面上，按照以下步骤操作：
    1. 对于“摘要”，用几句话描述这个问题。 
    1. 对于“问题类型”，请选择“配额”。 
    1. 对于“问题子类型”，选择“分区更改请求” 。 
    
        :::image type="content" source="./media/event-hubs-faq/support-request-increase-partitions.png" alt-text="增加分区计数":::

分区计数可以增加到正好 40 个。 在这种情况下，TU 的数量也必须增加到 40 个。 如果你稍后决定将 TU 限制降低到 <= 20 个，那么最大分区限制也将减少到 32 个。 

减少分区数不会影响现有的事件中心，因为分区数只在事件中心级别应用，在创建该中心之后是不可变的。 

## <a name="pricing"></a>定价

### <a name="where-can-i-find-more-pricing-information"></a>我可以在哪里找到更多定价信息？

有关事件中心定价的完整信息，请参阅[事件中心定价详细信息](https://www.azure.cn/pricing/details/event-hubs/)。

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>保留事件中心事件超过 24 小时的话，要收取费用吗？

事件中心标准层允许消息保留期长于 24 小时，最长以 7 天为限。 如果所存储事件总量的大小超过所选吞吐量单位数量的存储限制（每个吞吐量单位 84 GB），超出限制的部分会按公布的 Azure Blob 存储区费率收费。 每个吞吐量单元的存储限制包括 24 小时（默认值）的保留期期间的所有存储成本，即使吞吐量单元已经用到了最大入口限制。

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>怎样对事件中心存储区大小进行计算和收费？

所有存储的事件的总大小（包括所有事件中心内事件标头或磁盘存储器结构上的所有内部开销）按整天计量。 在一天结束时，计算存储区大小峰值。 每天的存储限制根据在当天所选择的吞吐量单元的最小数量（每个吞吐量单元提供 84 GB 的限制）来计算。 如果总大小超过计算出的每日存储限额，超出的存储量会采用 Azure Blob 存储费率（按 **本地冗余存储** 费率）来计费。

### <a name="how-are-event-hubs-ingress-events-calculated"></a>事件中心入口事件是怎样计算的？

发送到事件中心的每个事件均计为一条可计费消息。 *入口事件* 定义为小于等于 64 KB 的数据单位。 任何小于等于 64 KB 的事件均被视为一个计费事件。 如果该事件大于 64 KB，则根据事件大小按 64 KB 的倍数来计算计费事件的数量。 例如，发送到事件中心的 8-KB 事件按一个事件计费，而发送到事件中心的 96-KB 的消息则按两个事件计费。

从事件中心耗用的事件以及管理操作和控制调用（例如检查点）不计为计费入口事件，但会累计，其上限为吞吐量单位限额。

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>中转连接费用是否适用于事件中心？

连接费用只在使用 AMQP 协议时适用。 使用 HTTP 发送事件没有连接费用，无论发送系统或设备的数量是多少。 如果计划使用 AMQP（例如，为了实现更高效的事件流式传输，或者为了对 IoT 命令和控制方案启用双向通信），请参阅[事件中心定价信息](https://www.azure.cn/pricing/details/event-hubs/)页，了解有关每个服务层级中包括多少连接的详细信息。

### <a name="how-is-event-hubs-capture-billed"></a>事件中心捕获如何计费？

命名空间中的任何事件中心启用了捕获选项时，即可启用捕获功能。 事件中心捕获按所购买的吞吐量单位以小时计费。 当吞吐量单位计数增加或减少时，事件中心捕获计费将在整个小时增量中反映这些变化。 有关事件中心捕获计费的详细信息，请参阅[事件中心定价信息](https://www.azure.cn/pricing/details/event-hubs/)。

### <a name="do-i-get-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>为事件中心捕获选择的存储帐户是否要付费？

捕获使用在事件中心启用捕获时提供的存储帐户。 因为这是你的存储帐户，任何针对此配置的费用更改都将计入你的 Azure 订阅。

## <a name="quotas"></a>配额

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>是否有与事件中心关联的配额？

如需所有事件中心配额的列表，请参阅[配额](event-hubs-quotas.md)。

## <a name="troubleshooting"></a>疑难解答

### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>为什么在从其他订阅中删除命名空间后无法创建该命名空间？ 
从订阅中删除命名空间时，请等待 4 个小时，然后才能在另一个订阅中使用相同的名称重新创建它。 否则，可能会收到以下错误消息：`Namespace already exists`。 

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>事件中心生成的异常有哪些，建议采取什么操作？

有关可能的事件中心异常的列表，请参阅[异常概述](event-hubs-messaging-exceptions.md)。

<!-- Not Full Available  ### Diagnostic logs -->

### <a name="support-and-sla"></a>支持和 SLA

事件中心的技术支持可通过 [Azure 服务总线的 Microsoft 问答页面](https://www.azure.cn/support/contact/)获取。 计费和订阅管理支持免费提供。

若要详细了解我们的 SLA，请参阅[服务级别协议](https://www.azure.cn/support/legal/sla/)页面。

## <a name="azure-stack-hub"></a>Azure Stack Hub

### <a name="how-can-i-target-a-specific-version-of-azure-storage-sdk-when-using-azure-blob-storage-as-a-checkpoint-store"></a>当使用 Azure Blob 存储作为检查点存储时，我如何以特定版本的 Azure Storage SDK 为目标？
如果在 Azure Stack Hub 上运行此代码，除非将特定的存储 API 版本作为目标，否则会遇到运行时错误。 这是因为事件中心 SDK 使用 Azure 中提供的最新 Azure 存储 API，而此 API 可能在 Azure Stack Hub 平台上不可用。 Azure Stack Hub 支持的存储 Blob SDK 版本可能与 Azure 上通常提供的版本不同。 如果你是将 Azure Blob 存储用作检查点存储，请检查[支持用于 Azure Stack Hub 内部版本的 Azure 存储 API 版本](/azure-stack/user/azure-stack-acs-differences?#api-version)，并在代码中将该版本作为目标。 

例如，如果在 Azure Stack Hub 版本 2005 上运行，则存储服务的最高可用版本为版本 2019-02-02。 默认情况下，事件中心 SDK 客户端库使用 Azure 上的最高可用版本（在 SDK 发布时为 2019-07-07）。 在这种情况下，除了执行本部分中的步骤以外，还需要添加相关代码，将存储服务 API 版本 2019-02-02 作为目标。 如需通过示例来了解如何以特定的存储 API 版本为目标，请参阅以下 C#、Java、Python 和 JavaScript/TypeScript 示例。  

如需通过示例来了解如何从代码中以特定存储 API 版本为目标，请参阅 GitHub 上的以下示例： 

- [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/)
- [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java)
- Python - [同步](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/receive_events_using_checkpoint_store_storage_api_version.py)、[异步](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/receive_events_using_checkpoint_store_storage_api_version_async.py)
- [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript/receiveEventsWithApiSpecificStorage.js) 和 [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript/src/receiveEventsWithApiSpecificStorage.ts)

## <a name="next-steps"></a>后续步骤

访问以下链接可以了解有关事件中心的详细信息：

* [事件中心概述](./event-hubs-about.md)
* [创建事件中心](event-hubs-create.md)
* [事件中心自动膨胀](event-hubs-auto-inflate.md)
