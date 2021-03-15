---
title: 用于 SQL API 的 Azure Cosmos DB Java SDK v4 的发行说明和资源
description: 了解有关用于 SQL API 的 Azure Cosmos DB Java SDK v4 和 SDK 的所有信息，包括发行日期、停用日期和 Azure Cosmos DB SQL Async Java SDK 各版本之间所做的更改。
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
origin.date: 08/12/2020
author: rockboyfor
ms.date: 03/15/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: devx-track-java
ms.openlocfilehash: f8d00793efa1415c029cb4abb1589e9682c4c4b5
ms.sourcegitcommit: fb2fba1c106406553ed84b8652a915c823d9ab07
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "102996666"
---
<!--Verified successfully-->
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>用于 Core (SQL) API 的 Azure Cosmos DB Java SDK v4：发行说明和资源
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET 更改源 SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK v4](sql-api-sdk-java-v4.md)
> * [Async Java SDK v2](sql-api-sdk-async-java.md)
> * [Sync Java SDK v2](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Spark 连接器](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST 资源提供程序](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](./sql-query-getting-started.md)
> * [批量执行工具 - .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [批量执行程序 - Java](sql-api-sdk-bulk-executor-java.md)

用于 Core (SQL) 的 Azure Cosmos DB Java SDK v4 将异步 API 和同步 API 合并到一个 Maven 项目中。 v4 SDK 基于 Project Reactor 和 [Netty 库](https://netty.io/)提供了增强的性能、新 API 功能和 Async 支持。 通过 Azure Cosmos DB Java SDK v4 与 [Azure Cosmos DB Async Java SDK v2](sql-api-sdk-async-java.md) 和 [Azure Cosmos DB Sync Java SDK v2](sql-api-sdk-java.md)，用户可以提高性能。

> [!IMPORTANT]  
> 这些发行说明仅适用于 Azure Cosmos DB Java SDK v4。 如果你当前使用的是早于 v4 的版本，请参阅[迁移到 Azure Cosmos DB Java SDK v4](migrate-java-v4-sdk.md) 指南，获取升级到 v4 的相关帮助。
>
> 以下是快速操作的三个步骤！
> 1. 安装[最低支持的 Java 运行时版本，JDK 8](https://docs.azure.cn/java/java-supported-jdk-runtime)，以便可以使用 SDK。
> 2. 通过 [Azure Cosmos DB Java SDK v4 快速入门指南](./create-sql-api-java.md)，可以访问 Maven 项目并完成基本的 Azure Cosmos DB 请求。
> 3. 阅读 Azure Cosmos DB Java SDK v4 [性能提示](performance-tips-java-sdk-v4-sql.md)和[疑难解答](troubleshoot-java-sdk-v4-sql.md)指南，以优化应用程序的 SDK。
>
> 此外，[Azure Cosmos DB 研讨会和实验室](https://aka.ms/cosmosworkshop)也是了解如何使用 Azure Cosmos DB Java SDK v4 的绝佳资源！
>

## <a name="helpful-content"></a>帮助性内容

| Content | 链接 |
|---|---|
|**SDK 下载**| [Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos) |
|**API 文档** | [Java API 参考文档](https://docs.microsoft.com/java/api/overview/azure/cosmosdb/client) |
|**参与 SDK** | [GitHub 上用于 Java 的 Azure SDK 中央存储库](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-cosmos) | 
|**入门** | [快速入门：生成 Java 应用以管理 Azure Cosmos DB SQL API 数据](./create-sql-api-java.md) <br /> [具有快速入门代码的 GitHub 存储库](https://github.com/Azure-Samples/azure-cosmos-java-getting-started) | 
|**基本代码示例** | [Azure Cosmos DB：SQL API 的 Java 示例](sql-api-java-sdk-samples.md) <br /> [具有示例代码的 GitHub 存储库](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples)|
|**包含更改源的控制台应用**| [更改源 - Java SDK v4 示例](create-sql-api-java-changefeed.md) <br /> [具有示例代码的 GitHub 存储库](https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example)| 
|**Web 应用示例**| [使用 Java SDK v4 构建 Web 应用](sql-api-java-application.md) <br /> [具有示例代码的 GitHub 存储库](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-todo-app)|
| **性能提示**| [适用于 Java SDK v4 的性能提示](performance-tips-java-sdk-v4-sql.md)| 
| **故障排除** | [排查 Java SDK v4 问题](troubleshoot-java-sdk-v4-sql.md) |
| **从旧 SDK 迁移到 v4** | [迁移到 Java V4 SDK](migrate-java-v4-sdk.md) |
| 受支持的最小运行时|[JDK 8](https://docs.azure.cn/java/java-supported-jdk-runtime) | 
| **Azure Cosmos DB 研讨会和实验室** |[Cosmos DB 研讨会主页](https://aka.ms/cosmosworkshop)

## <a name="release-history"></a>发布历史记录

## <a name="490-beta1-unreleased"></a>4.9.0-beta.1（未发布）


### <a name="480-2020-10-27"></a>4.8.0 (2020-10-27)
#### <a name="new-features"></a>新功能
* 向 `CosmosItemRequestOptions` 添加了 `contentResponseOnWriteEnabled` 功能。
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了从延续令牌恢复时可能会影响查询行为的问题。

### <a name="471-2020-10-21"></a>4.7.1 (2020-10-21)
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 改进了 449 重试策略，以在初始重试时强制退避，并从较短的退避开始。

### <a name="470-2020-10-17-note-we-strongly-recommend-our-customers-to-use-version-470-and-above"></a>4.7.0 (2020-10-17) 注释：强烈建议客户使用 4.7.0 及更高版本
#### <a name="new-features"></a>新功能
* 添加了针对事务性批处理的 Beta 版本。

#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了在以“,”作为浮点分隔符的区域设置中分析查询指标时出现的错误。
* 从网关检索包含无效 json 的响应时，停止了过度的区域故障转移。
* 修复了一个错误，该错误导致网关中不必要地预期某些查询，即使使用直接传输也是如此。
* 通过处理通道关闭时引发的 SSLException 来降低日志记录噪声级别。
* 提高了“404 - ReadSession 不可用”错误的重试逻辑效率。

### <a name="460-2020-09-30"></a>4.6.0 (2020-09-30)
#### <a name="new-features"></a>新功能
* 添加了新的 API，用于在 Cosmos 中支持 AAD 基于角色的访问控制。 这是一项预览功能，需要在帐户设置中启用它。
* 向 `CosmosPagedFlux`/`CosmosPagedIterable` 添加了处理程序 API (beta)，以便对每个响应调用。

### <a name="452-2020-09-29"></a>4.5.2 (2020-09-29)
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 增强了在出现间歇性连接问题时执行查询和获取元数据缓存的可靠性。

### <a name="451-2020-09-25"></a>4.5.1 (2020-09-25)
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 为 ChangeFeedProcessor 添加了预览实现，可让你更详细地查看当前状态。
* 解决了同时运行的多分区监督器任务在 leaseAcquireInterval 小于 leaseRenewInterval 时出现的问题。
* 改进了 Rntbd 连接诊断。
* 已阻止将 onError Dropped 事件泄露到默认反应器挂钩的情况。

### <a name="450-2020-09-16"></a>4.5.0 (2020-09-16)
#### <a name="new-features"></a>新功能
* 增强了 Rntbd 堆栈在出现间歇性连接问题时的可靠性。
* 缩短了在多区域帐户的单个后端副本出现间歇性连接问题时出现的延迟，避免启动不必要的区域故障转移。

### <a name="440-2020-09-12"></a>4.4.0 (2020-09-12)
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了启用 `netty-tcnative-boringssl` 依赖项时引发的 RequestTimeoutException。
* 解决了 `GATEWAY` 模式中 `Delete` 操作的内存泄漏问题。
* 修复了终结点 URI 无效时 `CosmosClient` 实例化中的泄漏情况。
* 改进了 `CPU History` 诊断。

### <a name="440-beta1-2020-08-27"></a>4.4.0-beta.1 (2020-08-27)
#### <a name="new-features"></a>新功能
* 添加了新的 API，用于高效地通过 pk/id 对列表来加载多个文档，或加载与一组 pk 值对应的所有文档。
* 添加了新的 `deleteItem` API。
* 默认情况下启用了查询指标。
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了 `GatewayAddressCache` 中的 NPE。
* 修复了零项响应的查询指标问题。
* 提高了地址分析和主密钥身份验证的性能（降低了 CPU 使用率）。

### <a name="432-beta2-2020-08-17"></a>4.3.2-beta.2 (2020-08-17)
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 与以前版本相比没有变化，因为与 spring 数据模块的兼容性问题而发布。

### <a name="432-beta1-2020-08-14"></a>4.3.2-beta.1 (2020-08-14)
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了 RntbdServiceEndpoint 中的问题，以避免未使用的 TCP 连接提前关闭。

### <a name="431-2020-08-13"></a>4.3.1 (2020-08-13)
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了 `GROUP BY` 查询的问题。在该查询中，它仅返回一页。
* 根据中央 SDK 准则修复了用户代理字符串格式。
* 增强了诊断信息，使之包括查询计划诊断。

### <a name="430-2020-07-29"></a>4.3.0 (2020-07-29)
#### <a name="new-features"></a>新功能
* 已将 reactor-core 库版本更新为 `3.3.8.RELEASE`。 
* 已将 reactor-netty 库版本更新为 `0.9.10.RELEASE`。 
* 已将 netty 库版本更新为 `4.1.51.Final`。 
* 为具有 `partitionKey` 的 `upsertItem` 添加了新的重载 API。 
* 添加了开放式遥测跟踪支持。 
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了在网关模式下取消请求时引发 SSLException 的问题。
* 修复了存储过程执行中的资源限制重试策略问题。
* 修复了 SDK 在日志级别调试模式下挂起的问题。 
* 修复了直接模式下延迟定期出现峰值的问题。 
* 修复了客户端初始化时间过长的问题。 
* 修复了使用直接模式和网关模式自定义客户端时的 http 代理 bug。 
* 修复了用户中的潜在 NPE 传递 null 选项的问题。 
* 将 timeUnit 添加到诊断字符串中的 `requestLatency`。
* 从诊断字符串中删除了重复的 uri 字符串。 
* 将诊断字符串修复为正确的 JSON 格式以用于点操作。
* 修复了导致 reactor 链在出现“未找到”异常时爆发的 `.single()` 运算符问题。 

### <a name="420-2020-07-14"></a>4.2.0 (2020-07-14)
#### <a name="new-features"></a>新功能
* 向 `CosmosStoredProcedureRequestOptions` 添加了启用脚本日志记录的 API。
* 将 `DirectConnectionConfig` 的默认 `idleEndpointTimeout` 更新为 1 小时，并将默认 `connectTimeout` 更新为 5 秒。
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了 `GatewayConnectionConfig` `idleConnectionTimeout` 替代 `DirectConnectionConfig` `idleConnectionTimeout` 的问题。
* 修复了 `CosmosQueryRequestOptions` 中的 `responseContinuationTokenLimitInKb` get 和 set API。
* 修复了使用相同名称重新创建集合时查询和更改源中的问题。
* 修复了引发 ClassCastException 的顶层查询的问题。
* 修复了 order by 查询引发 NullPointerException 的问题。
* 修复了在直接模式下处理已取消的请求导致调用 reactor `onErrorDropped` 的问题。 

### <a name="410-2020-06-25"></a>4.1.0 (2020-06-25)
#### <a name="new-features"></a>新功能
* 添加了对 `GROUP BY` 查询的支持。
* 在 DirectConnectionConfig 中将 maxConnectionsPerEndpoint 的默认值增加到 130。
* 在 DirectConnectionConfig 中将 maxRequestsPerConnection 的默认值增加到 30。
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了使用继续标记恢复时 order by 查询返回重复结果的问题。 
* 修复了 value 查询为嵌套对象返回 null 值的问题。
* 修复了 RntbdClientChannelPool 中请求管理器上的空指针异常问题。

### <a name="401-2020-06-10"></a>4.0.1 (2020-06-10)
#### <a name="new-features"></a>新功能
* 已将 `QueryRequestOptions` 重命名为 `CosmosQueryRequestOptions`。
* 已将 `ChangeFeedProcessorBuilder` 更新为生成器模式。
* 使用新的容器名称和子资源 API 更新了 `CosmosPermissionProperties`。
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了 ConnectionPolicy `toString()` 空指针异常。

### <a name="401-beta4-2020-06-03"></a>4.0.1-beta.4 (2020-06-03)
#### <a name="new-features"></a>新功能
* 为 `CosmosClientBuilder` 添加了更多示例和丰富的文档。 
* 更新了带有吞吐量属性的 `CosmosDatabase` 和 `CosmosContainer` API，以支持自动缩放/Autopilot。 
* 已将 `CosmosClientException` 重命名为 `CosmosException`。 
* 已将 `AccessCondition` 和 `AccessConditionType` API 替换为 `ifMatchETag()` 和 `ifNoneMatchETag()` API。 
* 已将所有 `Cosmos*AsyncResponse` 和 `CosmosResponse` 类型合并为单个 `CosmosResponse` 类型。
* 已将 `CosmosResponseDiagnostics` 重命名为 `CosmosDiagnostics`。  
* 已将 `FeedResponseDiagnostics` 包装到 `CosmosDiagnostics` 中。 
* 从 azure-cosmos 中删除了 `jackson` 依赖项并依赖于 azure-core。 
* 已将 `CosmosKeyCredential` 替换为 `AzureKeyCredential` 类型。 
* 为 `GatewayConnectionConfig` 添加了 `ProxyOptions` API。 
* 更新了 SDK 以使用 `Instant` 类型，而不是使用 `OffsetDateTime`。 
* 添加了新的枚举类型 `OperationKind`。 
* 已将 `FeedOptions` 重命名为 `QueryRequestOptions`。 
* 为 `Cosmos*Properties` 类型添加了 `getETag()` 和 `getTimestamp()` API。 
* 在 `CosmosException` 和 `CosmosDiagnostics`中添加了 `userAgent` 信息。 
* 已将 `Diagnostics` 中的新换行符更新为系统的新换行符。 
* 删除了 `readAll*` API，请改用查询选择所有 API。
* 添加了 `ChangeFeedProcessor` 估算滞后时间 API。   
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了在按查询的值顺序分析查询结果时的问题。 

### <a name="401-beta3-2020-05-15"></a>4.0.1-beta.3 (2020-05-15)
#### <a name="new-features"></a>新功能
* 为 SDK 添加了自动缩放/autopilot 吞吐量预配支持。  
* 已将 `ConnectionPolicy` 替换为新的连接配置。 通过 `CosmosClientBuilder` 为直接模式和网关模式连接配置公开 `DirectConnectionConfig` 和 `GatewayConnectionConfig` API。
* 已将 `JsonSerializable` 和 `Resource` 移到实现包。 
* 向 CosmsClientBuilder 添加了 `contentResponseOnWriteEnabled` API，该 API 禁用写操作时的完整响应内容。
* 对响应类型公开了 `getETag()` API。
* 已将 `CosmosAuthorizationTokenResolver` 移至实现。 
* 已将 `preferredLocations` 和 `multipleWriteLocations` API 重命名为 `preferredRegions` 和 `multipleWriteRegions`。 
* 已将 `reactor-core` 更新为 3.3.5.RELEASE，将 `reactor-netty` 更新为 0.9.7.RELEASE，并将 `netty` 更新为 4.1.49.Final 版本。 
* 在 SDK 中添加了对 `analyticalStoreTimeToLive` 的支持。     
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了直接 TCP 客户端的套接字泄漏问题。
* 修复了 `orderByQuery` 的继续标记 bug。

### <a name="401-beta2-2020-04-21"></a>4.0.1-beta.2 (2020-04-21)
#### <a name="new-features"></a>新功能
* `CosmosClientException` 扩展了 `AzureException`。 
* 从 `FeedOptions` 中删除了 `maxItemCount` 和 `requestContinuationToken` API，改用 `CosmosPagedFlux` 和 `CosmosPagedIterable` 中的 `byPage()` API。
* 在 `Permission` API 的公共图面上引入了 `CosmosPermissionProperties`。
* 删除了 `SqlParameterList` 类型并将其替换为 `List`
* 修复了直接 TCP 客户端中的多个内存泄漏。 
* 添加了对 `DISTINCT` 查询的支持。 
* 删除了 `fasterxml.uuid, guava, commons-io, commons-collection4, commons-text` 的外部依赖项。  
* 已将 `CosmosPagedFlux` 和 `CosmosPagedIterable` 移到 `utils` 包。 
* 已将 netty 更新为“4.1.45.Final”，并已将 project reactor 更新为 3.3.3 版本。
* 已将公共 REST 协定更新为 `Final` 类。
* 添加了对点操作高级诊断的支持。
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了处理分区拆分但未找到分区时的 `ChangeFeedProcessor` bug。
* 修复了跨不同线程同步租约更新时的 `ChangeFeedProcessor` bug。

### <a name="401-beta1-2020-03-10"></a>4.0.1-beta.1 (2020-03-10)
#### <a name="new-features"></a>新功能 
* 已将包更新为 `com.azure.cosmos`
* 为模型/REST 协定添加了 `models` 包
* 为 `CosmosPagedFlux` 和 `CosmosPagedIterable` 类型添加了 `utils` 包。 
* 更新了公共 API 以跨 SDK 使用 `Duration`。
* 已将所有 REST 协定添加到 `models` 包。
* `RetryOptions` 已重命名为 `ThrottlingRetryOptions`。
* 为查询 API 添加了`CosmosPagedFlux` 和 `CosmosPagedIterable` 分页类型。 
* 添加了对在 `CosmosClientBuilder#connectionSharingAcrossClientsEnabled(true)` 中使用新 API 跨多个 CosmosClients 实例共享 TransportClient 的支持
* 通过删除双序列化/反序列化来进行查询优化。 
* 通过删除不必要的来回复制来优化响应标头。 
* 通过删除中间字符串实例化优化了 `ByteBuffer` 序列化/反序列化。
#### <a name="key-bug-fixes"></a>关键 Bug 修复
* 修复了在 StoreReader 中导致 `ArrayIndexOutOfBound` 异常的争用情况

## <a name="faq"></a>常见问题
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)] 

## <a name="next-steps"></a>后续步骤
若要了解有关 Cosmos DB 的详细信息，请参阅 [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) 服务页。

<!--Update_Description: update meta properties, wording update, update link-->