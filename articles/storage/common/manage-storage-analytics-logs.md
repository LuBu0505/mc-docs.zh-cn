---
title: 启用和管理 Azure 存储分析日志（经典）| Microsoft Docs
description: 了解如何使用 Azure 存储分析在 Azure 中监视存储帐户。
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 01/29/2021
ms.date: 03/08/2021
ms.author: v-jay
ms.reviewer: fryu
ms.subservice: common
ms.custom: monitoring
ms.openlocfilehash: 7465461dfa651f0ac9973972e0a6060b820b4087
ms.sourcegitcommit: 0b49bd1b3b05955371d1154552f4730182c7f0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196410"
---
# <a name="enable-and-manage-azure-storage-analytics-logs-classic"></a>启用和管理 Azure 存储分析日志（经典）

[Azure 存储分析](storage-analytics.md)提供 Blob、队列和表的日志。 可以使用 [Azure 门户](https://portal.azure.cn)来配置要为帐户记录的日志。 本文介绍了如何启用和管理日志。 若要了解如何启用指标，请参阅[启用和管理 Azure 存储分析指标（经典）](storage-monitor-storage-account.md)。  在 Azure 门户中检查和存储监视数据会产生相关的费用。 有关详细信息，请参阅[存储分析](storage-analytics.md)。

有关使用存储分析及其他工具来识别、诊断和排查 Azure 存储相关问题的深入指导，请参阅[监视、诊断和排查 Microsoft Azure 存储问题](storage-monitoring-diagnosing-troubleshooting.md)。

<a id="configure-logging"></a>

## <a name="enable-logs"></a>启用日志

可以指示 Azure 存储保存针对 Blob、表和队列服务发出的读取、写入和删除请求的诊断日志。 设置的数据保留策略也适用于这些日志。

> [!NOTE]
> Azure 文件存储目前支持存储分析指标，但尚不支持存储分析日志记录。

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. 在 [Azure 门户](https://portal.azure.cn)中选择“存储帐户”，然后单击存储帐户的名称打开存储帐户边栏选项卡。

2. 在菜单边栏选项卡的“监视(经典)”部分中选择“诊断设置(经典)”。

    ![Azure 门户中“监视”下面的诊断菜单项。](./media/manage-storage-analytics-logs/storage-enable-metrics-00.png)

3. 确保“状态”设置为“打开”，选择要为其启用日志记录的服务。

   > [!div class="mx-imgBorder"]
   > ![在 Azure 门户中配置日志记录。](./media/manage-storage-analytics-logs/enable-diagnostics.png)    


4. 确保选中“删除数据”复选框。  然后，通过移动复选框下方的滑块控件或直接修改显示在滑块控件旁边的文本框中的值，来设置要将日志数据保留的天数。 新存储帐户的默认保留期为 7 天。 如果不需要设置保留策略，请输入零。 如果没有保留策略，则由用户自行决定是否删除日志数据。

   > [!WARNING]
   > 日志作为数据存储在你的帐户中。 日志数据会随着时间的推移在你的帐户中累积，这可能会增加存储成本。 如果只需要一小段时间的日志数据，则可以通过修改数据保留策略来降低成本。 陈旧的日志数据（超出保留策略的数据）将被系统删除。 建议根据要将帐户的日志数据保留多长时间来设置保留策略。 有关详细信息，请参阅[按存储指标计费](storage-analytics-metrics.md#billing-on-storage-metrics)。

4. 单击“ **保存**”。

   诊断日志保存在存储帐户下名为 $logs 的 Blob 容器中。 可以使用 [Microsoft Azure 存储资源管理器](https://storageexplorer.com)等存储资源管理器来查看日志数据，也可以使用存储客户端库或 PowerShell 以编程方式这样做。

   有关如何访问 $logs 容器的信息，请参阅[存储分析日志记录](storage-analytics-logging.md)。

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. 打开 Windows PowerShell 命令窗口。

2. 运行 `Connect-AzAccount` 命令以登录 Azure 订阅，并按照屏幕上的说明操作。

   ```powershell
   Connect-AzAccount -Environment AzureChinaCloud
   ```

3. 如果你的标识关联到多个订阅，请设置你的活动订阅。

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   将 `<subscription-id>` 占位符值替换为你的订阅 ID。

5. 获取定义了要使用的存储帐户的存储帐户上下文。

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * 将 `<resource-group-name>` 占位符值替换为资源组的名称。

   * 将 `<storage-account-name>` 占位符值替换为存储帐户的名称。 

6. 使用 **Set-AzStorageServiceLoggingProperty** 更改当前的日志设置。 控制存储日志记录的 cmdlet 使用 LoggingOperations 参数，该参数是一个字符串，包含要记录的请求类型的逗号分隔列表。 三种可能的请求类型是“读取”、“写入”和“删除”  。 要关闭日志记录，请对 LoggingOperations 参数使用值“无” 。  

   以下命令在保留期设为 5 天的情况下，在默认存储帐户中为队列服务中的读取、写入和删除请求打开日志记录：  

   ```powershell
   Set-AzStorageServiceLoggingProperty -ServiceType Queue -LoggingOperations read,write,delete -RetentionDays 5 -Context $ctx
   ```  

   > [!WARNING]
   > 日志作为数据存储在你的帐户中。 日志数据会随着时间的推移在你的帐户中累积，这可能会增加存储成本。 如果只需要一小段时间的日志数据，则可以通过修改数据保留策略来降低成本。 陈旧的日志数据（超出保留策略的数据）将被系统删除。 建议根据要将帐户的日志数据保留多长时间来设置保留策略。 有关详细信息，请参阅[按存储指标计费](storage-analytics-metrics.md#billing-on-storage-metrics)。
   
   以下命令在默认存储帐户中为表服务关闭日志记录：  

   ```powershell
   Set-AzStorageServiceLoggingProperty -ServiceType Table -LoggingOperations none -Context $ctx 
   ```  

   若要了解如何配置 Azure PowerShell cmdlet 来使用 Azure 订阅并了解如何选择要使用的默认存储帐户，请参阅：[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/)。  

### <a name="net-v12"></a>[.NET v12](#tab/dotnet)

```csharp
QueueServiceClient queueServiceClient = new QueueServiceClient(connectionString);

QueueServiceProperties serviceProperties = queueServiceClient.GetProperties().Value;

serviceProperties.Logging.Delete = true;

QueueRetentionPolicy retentionPolicy = new QueueRetentionPolicy();
retentionPolicy.Enabled = true;
retentionPolicy.Days = 2;
serviceProperties.Logging.RetentionPolicy = retentionPolicy;

serviceProperties.HourMetrics = null;
serviceProperties.MinuteMetrics = null;
serviceProperties.Cors = null;

queueServiceClient.SetProperties(serviceProperties);
```

### <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
``` 

---

<a id="modify-retention-policy"></a>

## <a name="modify-log-data-retention-period"></a>修改日志数据保留期

日志数据会随着时间的推移在你的帐户中累积，这可能会增加存储成本。 如果只需要一小段时间的日志数据，则可以通过修改日志数据保留期来降低成本。 例如，如果只需要三天的日志，请将日志数据保留期设置为值 `3`。 这样一来，日志将在 3 天后自动从你的帐户中删除。 本部分介绍了如何查看当前的日志数据保留期，以及需要时如何更新该时间段。

> [!NOTE]
> 这些步骤仅适用于未在其上启用“分层命名空间”设置的帐户。 如果已在你的帐户上启用该设置，则尚不支持“保留天数”设置。 相反，你必须使用任何受支持的工具（例如 Azure 存储资源管理器、REST 或 SDK）手动删除日志。 若要在存储帐户中查找这些日志，请参阅[日志的存储方式](storage-analytics-logging.md#how-logs-are-stored)。

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. 在 [Azure 门户](https://portal.azure.cn)中选择“存储帐户”，然后单击存储帐户的名称打开存储帐户边栏选项卡。
2. 在菜单边栏选项卡的“监视(经典)”部分中选择“诊断设置(经典)”。

    ![Azure 门户中“监视”下面的诊断菜单项](./media/manage-storage-analytics-logs/storage-enable-metrics-00.png)

3. 确保选中“删除数据”复选框。  然后，通过移动复选框下方的滑块控件或直接修改显示在滑块控件旁边的文本框中的值，来设置要将日志数据保留的天数。

   > [!div class="mx-imgBorder"]
   > ![在 Azure 门户中修改保留期](./media/manage-storage-analytics-logs/modify-retention-period.png)

   新存储帐户的默认保留天数为 7 天。 如果不需要设置保留策略，请输入零。 如果没有保留策略，则由用户自行决定是否删除监视数据。
   
4. 单击“ **保存**”。

   诊断日志保存在存储帐户下名为 $logs 的 Blob 容器中。 可以使用 [Microsoft Azure 存储资源管理器](https://storageexplorer.com)等存储资源管理器来查看日志数据，也可以使用存储客户端库或 PowerShell 以编程方式这样做。

   有关如何访问 $logs 容器的信息，请参阅[存储分析日志记录](storage-analytics-logging.md)。

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. 打开 Windows PowerShell 命令窗口。

2. 运行 `Connect-AzAccount` 命令以登录 Azure 订阅，并按照屏幕上的说明操作。

   ```powershell
   Connect-AzAccount -Environment AzureChinaCloud
   ```

3. 如果你的标识关联到多个订阅，请设置你的活动订阅。

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   将 `<subscription-id>` 占位符值替换为你的订阅 ID。

5. 获取定义了存储帐户的存储帐户上下文。

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * 将 `<resource-group-name>` 占位符值替换为资源组的名称。

   * 将 `<storage-account-name>` 占位符值替换为存储帐户的名称。 

6. 使用 [Get-AzStorageServiceLoggingProperty](https://docs.microsoft.com/powershell/module/az.storage/get-azstorageserviceloggingproperty) 查看当前的日志保留策略。 下面的示例将 blob 和队列存储服务的保留期输出到控制台。

   ```powershell
   Get-AzStorageServiceLoggingProperty -ServiceType Blob, Queue -Context $ctx
   ```  

   在控制台输出中，保留期显示在 `RetentionDays` 列标题的下方。

   > [!div class="mx-imgBorder"]
   > ![PowerShell 输出中的保留策略](./media/manage-storage-analytics-logs/retention-period-powershell.png)

7. 使用 [Set-AzStorageServiceLoggingProperty](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageserviceloggingproperty) 更改保留期。 以下示例将保留期更改为 4 天。  

   ```powershell
   Set-AzStorageServiceLoggingProperty -ServiceType Blob, Queue -RetentionDays 4 -Context $ctx
   ```  

   若要了解如何配置 Azure PowerShell cmdlet 来使用 Azure 订阅并了解如何选择要使用的默认存储帐户，请参阅：[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/)。  

### <a name="net-v12"></a>[.NET v12](#tab/dotnet)

下面的示例将 blob 和队列存储服务的保留期输出到控制台。

```csharp
BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);
QueueServiceClient queueServiceClient = new QueueServiceClient(connectionString);

BlobServiceProperties blobServiceProperties = blobServiceClient.GetProperties().Value;
QueueServiceProperties queueServiceProperties = queueServiceClient.GetProperties().Value;

Console.WriteLine("Retention period for logs from the blob service is: " +
    blobServiceProperties.Logging.RetentionPolicy.Days.ToString());

Console.WriteLine("Retention period for logs from the queue service is: " +
    queueServiceProperties.Logging.RetentionPolicy.Days.ToString());
```

以下示例将保留期更改为 4 天。 

```csharp
BlobRetentionPolicy blobRetentionPolicy = new BlobRetentionPolicy();

blobRetentionPolicy.Enabled = true;
blobRetentionPolicy.Days = 4;

QueueRetentionPolicy queueRetentionPolicy = new QueueRetentionPolicy();

queueRetentionPolicy.Enabled = true;
queueRetentionPolicy.Days = 4;

blobServiceProperties.Logging.RetentionPolicy = blobRetentionPolicy;
blobServiceProperties.Cors = null;

queueServiceProperties.Logging.RetentionPolicy = queueRetentionPolicy;
queueServiceProperties.Cors = null;

blobServiceClient.SetProperties(blobServiceProperties);
queueServiceClient.SetProperties(queueServiceProperties);

Console.WriteLine("Retention policy for blobs and queues is updated");
```

### <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

下面的示例将 blob 和队列存储服务的保留期输出到控制台。

```csharp
var storageAccount = CloudStorageAccount.Parse(connectionString);

var blobClient = storageAccount.CreateCloudBlobClient();
var queueClient = storageAccount.CreateCloudQueueClient();

var blobserviceProperties = blobClient.GetServiceProperties();
var queueserviceProperties = queueClient.GetServiceProperties();

Console.WriteLine("Retention period for logs from the blob service is: " +
   blobserviceProperties.Logging.RetentionDays.ToString());

Console.WriteLine("Retention period for logs from the queue service is: " +
   queueserviceProperties.Logging.RetentionDays.ToString());
```

下面的示例将 blob 和队列存储服务的日志保留期更改为 4 天。 

```csharp

blobserviceProperties.Logging.RetentionDays = 4;
queueserviceProperties.Logging.RetentionDays = 4;

blobClient.SetServiceProperties(blobserviceProperties);
queueClient.SetServiceProperties(queueserviceProperties);  
``` 

---

### <a name="verify-that-log-data-is-being-deleted"></a>验证是否正在删除日志数据

可以通过查看存储帐户的 `$logs` 容器的内容来验证是否正在删除日志。 下图显示了 `$logs` 容器中某个文件夹的内容。 该文件夹对应于 2021 年 1 月，并且每个文件夹包含一天的日志。 如果今天的日期为 2021 年 1 月 29 日，并且你的保留策略设置为仅一天，则该文件夹应当仅包含一天的日志。

> [!div class="mx-imgBorder"]
> ![Azure 门户中的日志文件夹列表](./media/manage-storage-analytics-logs/verify-and-delete-logs.png)

<a id="download-storage-logging-log-data"></a>

## <a name="view-log-data"></a>查看日志数据

 要查看和分析日志数据，应该将包含你感兴趣的日志数据的 blob 下载到本地计算机。 你可以使用很多存储浏览工具从存储帐户下载 blob；你还可以使用 Azure 存储团队提供的命令行 Azure 复制工具 ([AzCopy](storage-use-azcopy-v10.md)) 下载日志数据。  
 
>[!NOTE]
> `$logs` 容器未与事件网格集成，因此在写入日志文件时不会收到通知。 

 要确保下载你感兴趣的日志数据，并避免多次下载相同的日志数据，请执行以下操作：  

-   对包含日志数据的 blob 使用日期和时间命名约定，以跟踪已下载用于分析的 blob，从而避免多次重新下载相同的数据。  

-   使用包含日志数据的 blob 中的元数据来确定特定期限，在该期限内，blob 会保留日志数据以标识需要下载的确切 blob。  

要开始使用 AzCopy，请参阅 [AzCopy 入门](storage-use-azcopy-v10.md) 

下面的示例显示如何下载队列服务的日志数据，时间从 2014 年 5 月 20 日上午 9 点、10 点和 11 点开始。

```
azcopy copy 'https://mystorageaccount.blob.core.chinacloudapi.cn/$logs/queue' 'C:\Logs\Storage' --include-path '2014/05/20/09;2014/05/20/10;2014/05/20/11' --recursive
```

若要详细了解如何下载特定文件，请参阅[使用 AzCopy v10 从 Azure Blob 存储下载 blob](./storage-use-azcopy-blobs-download.md?toc=%2fstorage%2fblobs%2ftoc.json)。

下载日志数据后，可以查看文件中的日志条目。 这些日志文件使用带分隔符的文本格式，许多日志读取工具都可以分析此格式（有关详细信息，请参阅[对 Azure 存储进行监视、诊断和故障排除](storage-monitoring-diagnosing-troubleshooting.md)指南）。 不同的工具提供不同的功能用于筛选、排序和搜索日志文件的内容及设置其格式。 有关存储日志记录日志文件格式和内容的详细信息，请参阅[存储分析日志格式](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format)和[存储分析记录的操作和状态消息](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)。

## <a name="next-steps"></a>后续步骤

* 若要详细了解存储分析，请参阅[存储分析](storage-analytics.md)。
* [配置存储分析指标](storage-monitor-storage-account.md)。
* 有关使用 .NET 语言配置存储日志记录的详细信息，请参阅[存储客户端库参考](https://docs.azure.cn/dotnet/api/overview/storage/client?view=azure-dotnet)。 
* 有关使用 REST API 配置存储日志记录的一般信息，请参阅[启用和配置存储分析](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics)。
* 详细了解存储分析日志的格式。 请参阅[存储分析日志格式](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format)。