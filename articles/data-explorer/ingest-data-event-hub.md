---
title: 将数据从事件中心引入到 Azure 数据资源管理器
description: 本文介绍如何将数据从事件中心引入（加载）到 Azure 数据资源管理器中。
author: orspod
ms.author: v-tawe
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: how-to
origin.date: 08/13/2020
ms.date: 01/19/2021
ms.localizationpriority: high
ms.openlocfilehash: 08bd97390f5fc27ee836ed7cfffb20eef00a4637
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611515"
---
# <a name="ingest-data-from-event-hub-into-azure-data-explorer"></a>将数据从事件中心引入到 Azure 数据资源管理器

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-hub.md)
> * [C#](data-connection-event-hub-csharp.md)
> * [Python](data-connection-event-hub-python.md)
> * [Azure Resource Manager 模板](data-connection-event-hub-resource-manager.md)

[!INCLUDE [data-connector-intro](includes/data-connector-intro.md)]

Azure 数据资源管理器可从事件中心引入（加载数据），是一个大数据流式处理平台和事件引入服务。 [事件中心](/event-hubs/event-hubs-about)每秒可以近实时处理数百万个事件。 在本文中，将创建事件中心，从 Azure 数据资源管理器中连接到该事件中心，并查看通过系统的数据流。

有关从事件中心引入到 Azure 数据资源管理器的常规信息，请参阅[连接到事件中心](ingest-data-event-hub-overview.md)。

## <a name="prerequisites"></a>先决条件

* 如果没有 Azure 订阅，请在开始前创建一个[试用订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
* [一个测试群集和数据库](create-cluster-database-portal.md)。
* 生成数据并将其发送到事件中心的[示例应用](https://github.com/Azure-Samples/event-hubs-dotnet-ingest)。 将示例应用下载到系统。
* 用于运行示例应用的 [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.cn/)。

## <a name="create-an-event-hub"></a>创建事件中心

在本文中，将生成示例数据并将其发送到事件中心。 第一步是创建事件中心。 通过使用 Azure 资源管理器模板在 Azure 门户中执行此操作。

1. 若要创建事件中心，请使用以下按钮开始部署。 右键单击并选择“在新窗口中打开”，以便按本文中的剩余步骤操作。

    [![“部署到 Azure”按钮](media/ingest-data-event-hub/deploybutton.png)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

    “部署到 Azure”按钮将转到 Azure 门户以填写部署窗体。

    ![“创建事件中心”窗体](media/ingest-data-event-hub/deploy-to-azure.png)

1. 选择要在其中创建事件中心的订阅，并创建名为 test-hub-rg 的资源组。

    ![创建资源组](media/ingest-data-event-hub/create-resource-group.png)

1. 使用以下信息填写窗体。

    ![部署窗体](media/ingest-data-event-hub/deployment-form.png)

    对下表中未列出的任何设置使用默认值。

    **设置** | **建议的值** | **字段说明**
    |---|---|---|
    | 订阅 | 订阅 | 选择要用于事件中心的 Azure 订阅。|
    | 资源组 | test-hub-rg | 创建新的资源组。 |
    | 位置 | *中国东部 2* | 对于本文，选择“中国东部 2”。 对于生产系统，请选择最能满足你需求的区域。 在与 Kusto 群集相同的位置创建事件中心命名空间以获得最佳性能（对于具有高吞吐量的事件中心命名空间来说最重要）。
    | 命名空间名称 | 唯一的命名空间名称 | 选择用于标识命名空间的唯一名称。 例如，mytestnamespace。 域名 *servicebus.chinacloudapi.cn* 将追加到所提供的名称。 字段只能包含字母、数字和连字符。 名称必须以字母开头，并且必须以字母或数字结尾。 值长度必须介于 6 到 50 个字符之间。
    | 事件中心名称 | *test-hub* | 事件中心位于命名空间下，该命名空间提供唯一的范围容器。 事件中心名称在命名空间中必须唯一。 |
    | 使用者组名称 | *test-group* | 使用者组允许多个使用应用程序各自具有事件流的单独视图。 |
    | | |

1. 选择“购买”，确认你要在订阅中创建资源。

1. 在工具栏上选择“通知”以监视预配过程。 部署成功可能需要几分钟时间，但现在可以继续执行下一步。

    ![通知图标](media/ingest-data-event-hub/notifications.png)

## <a name="create-a-target-table-in-azure-data-explorer"></a>在 Azure 数据资源管理器中创建目标表

现在，在 Azure 数据资源管理器中创建一个表，事件中心会向该表发送数据。 在“先决条件”中预配的群集和数据库中创建表。

1. 在 Azure 门户中导航到群集，然后选择“查询”。

    ![查询应用程序链接](media/ingest-data-event-hub/query-explorer-link.png)

1. 将以下命令复制到窗口中，然后选择“运行”以创建将接收引入数据的表 (TestTable)。

    ```Kusto
    .create table TestTable (TimeStamp: datetime, Name: string, Metric: int, Source:string)
    ```

    ![运行创建查询](media/ingest-data-event-hub/run-create-query.png)

1. 将以下命令复制到窗口中，然后选择“运行”将传入的 JSON 数据映射到表 (TestTable) 的列名和数据类型。

    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp", "Properties": {"Path": "$.timeStamp"}},{"column":"Name", "Properties": {"Path":"$.name"}} ,{"column":"Metric", "Properties": {"Path":"$.metric"}}, {"column":"Source", "Properties": {"Path":"$.source"}}]'
    ```

## <a name="connect-to-the-event-hub"></a>连接到事件中心

现在，请通过 Azure 数据资源管理器连接到事件中心。 当此连接建立好以后，流入事件中心的数据会流式传输到此前在本文中创建的测试表。

1. 在工具栏上选择“通知”，以验证事件中心部署是否成功。

1. 在创建的群集下，选择“数据库”，然后选择“TestDatabase”。

    ![选择测试数据库](media/ingest-data-event-hub/select-test-database.png)

1. 选择“数据引入”，然后选择“添加数据连接”。 

    :::image type="content" source="media/ingest-data-event-hub/event-hub-connection.png" alt-text="在数据中心选择“数据引入”和“添加数据连接”- Azure 数据资源管理器":::

### <a name="create-a-data-connection"></a>创建数据连接

1. 使用以下信息填写窗体：

    :::image type="content" source="media/ingest-data-event-hub/data-connection-pane.png" alt-text="“数据连接”窗格 - 事件中心 - Azure 数据资源管理器":::

    **设置** | **建议的值** | **字段说明**
    |---|---|---|
    | 数据连接名称 | *test-hub-connection* | 要在 Azure 数据资源管理器中创建的连接的名称。|
    | 订阅 |      | 事件中心资源所在的订阅 ID。  |
    | 事件中心命名空间 | 唯一的命名空间名称 | 先前选择的用于标识命名空间的名称。 |
    | 事件中心 | *test-hub* | 你创建的事件中心。 |
    | 使用者组 | *test-group* | 在创建的事件中心定义的使用者组。 |
    | 事件系统属性 | 选择相关属性 | [事件中心系统属性](/service-bus-messaging/service-bus-amqp-protocol-guide#message-annotations)。 添加系统属性时，[创建](kusto/management/create-table-command.md)或[更新](kusto/management/alter-table-command.md)表架构和[映射](kusto/management/mappings.md)以包括所选属性。 有关系统属性限制的详细信息，请参阅[事件系统属性映射](#event-system-properties-mapping)。 |
    | 压缩 | *无* | 事件中心消息有效负载的压缩类型。 支持的压缩类型：None、GZip。|
    
#### <a name="target-table"></a>目标表

路由引入数据有两个选项：静态和动态。 本文将使用静态路由，需在其中将表名、数据格式和映射指定为默认值。 如果事件中心消息包含数据路由信息，则此路由信息将替代默认设置。

1. 填写以下路由设置：
  
   :::image type="content" source="media/ingest-data-event-hub/default-routing-settings.png" alt-text="用于将数据引入到事件中心的默认路由设置 - Azure 数据资源管理器":::
        
   |**设置** | **建议的值** | **字段说明**
   |---|---|---|
   | 表名 | *TestTable* | 在“TestDatabase”中创建的表。 |
   | 数据格式 | *JSON* | 支持的格式为 Avro、CSV、JSON、MULTILINE JSON、ORC、PARQUET、PSV、SCSV、SOHSV、TSV、TXT、TSVE、APACHEAVRO 和 W3CLOG。 |
   | 映射 | TestMapping | 在“TestDatabase”中创建的[映射](kusto/management/mappings.md)，它将传入的数据映射到“TestTable”的列名称和数据类型 。 对于 JSON、多行 JSON 和 AVRO 是必需的，对于其他格式是可选的。|
    
   > [!NOTE]
   > * 无需指定所有默认路由设置。 部分设置也是接受的。
   > * 只有创建数据连接后进入队列的事件才会被引入。

1. 选择“创建”  。 

### <a name="event-system-properties-mapping"></a>事件系统属性映射

[!INCLUDE [event-hub-system-mapping](includes/event-hub-system-mapping.md)]

如果在表的“数据源”部分选择了“事件系统属性”，则必须在表架构和映射中包含[系统属性](ingest-data-event-hub-overview.md#system-properties)。

## <a name="copy-the-connection-string"></a>复制连接字符串

运行在先决条件中列出的[示例应用](https://github.com/Azure-Samples/event-hubs-dotnet-ingest)时，需要事件中心命名空间的连接字符串。

1. 在创建的事件中心命名空间下，选择“共享访问策略”，然后选择“RootManageSharedAccessKey”。

    ![共享访问策略](media/ingest-data-event-hub/shared-access-policies.png)

1. 复制“连接字符串 - 主键”。 请将其粘贴到下一节。

    ![连接字符串](media/ingest-data-event-hub/connection-string.png)

## <a name="generate-sample-data"></a>生成示例数据

使用下载的[示例应用](https://github.com/Azure-Samples/event-hubs-dotnet-ingest)生成数据。

1. 在 Visual Studio 中打开示例应用解决方案。

1. 在 program.cs 文件中，将 `eventHubName` 常量更新为事件中心的名称，并将 `connectionString` 常量更新为从事件中心命名空间复制的连接字符串。

    ```csharp
    const string eventHubName = "test-hub";
    // Copy the connection string ("Connection string-primary key") from your Event Hub namespace.
    const string connectionString = @"<YourConnectionString>";
    ```

1. 构建并运行应用。 应用将消息发送到事件中心，并且每十秒显示一次状态。

1. 应用发送一些消息后，继续执行下一步：查看到事件中心和测试表的数据流。

## <a name="review-the-data-flow"></a>查看数据流

应用生成数据以后，现在可以看到该数据从事件中心流到群集中的表。

1. 在 Azure 门户中的事件中心下，可以看到应用运行时活动的峰值。

    ![事件中心图](media/ingest-data-event-hub/event-hub-graph.png)

1. 若要检查到目前为止已向数据库发送的消息数，请在测试数据库中运行以下查询。

    ```Kusto
    TestTable
    | count
    ```

1. 若要查看消息的内容，请运行以下查询：

    ```Kusto
    TestTable
    ```

    结果集应如下所示：

    ![消息结果集](media/ingest-data-event-hub/message-result-set.png)

    > [!NOTE]
    > * Azure 数据资源管理器具有用于数据引入的聚合（批处理）策略，旨在优化引入过程。 默认批处理策略配置为在批满足以下条件之一时封装批：最大延迟时间为 5 分钟、总大小为 1G 或 1000 个 blob。 因此，你可能会遇到延迟。 有关详细信息，请参阅[批处理策略](kusto/management/batchingpolicy.md)。 
    > * 事件中心引入包括 10 秒或 1 MB 的事件中心响应时间。 
    > * 配置表以支持流式处理并消除响应时间延迟。 请参阅[流式处理策略](kusto/management/streamingingestionpolicy.md)。 

## <a name="clean-up-resources"></a>清理资源

如果不打算再次使用事件中心，请清理 test-hub-rg，以避免产生费用。

1. 在 Azure 门户的最左侧选择“资源组”，，然后选择创建的资源组。  

    如果左侧菜单处于折叠状态，请选择 ![“展开”按钮](media/ingest-data-event-hub/expand.png) 将其展开。

   ![选择要删除的资源组](media/ingest-data-event-hub/delete-resources-select.png)

1. 在“test-resource-group”下，选择“删除资源组”。

1. 在新窗口中，键入要删除的资源组的名称 (test-hub-rg)，然后选择“删除”。

## <a name="next-steps"></a>后续步骤

* [在 Azure 数据资源管理器中查询数据](web-query-data.md)
