---
title: 对 Azure 数据工厂中的管道业务流程和触发器进行故障排除
description: 使用不同方法排查 Azure 数据工厂中的管道触发器问题。
author: WenJason
ms.service: data-factory
origin.date: 12/15/2020
ms.date: 02/01/2021
ms.topic: troubleshooting
ms.author: v-jay
ms.reviewer: susabat
ms.openlocfilehash: 6a74eb293d28d789a86fbe13bae39504e4f2961e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059812"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-azure-data-factory"></a>对 Azure 数据工厂中的管道业务流程和触发器进行故障排除

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure 数据工厂中的“管道运行”用于定义管道执行实例。 例如，假设你有一个管道，它在上午 8:00、9:00 和 10:00 点运行。 在这种情况下，将有三次独立的管道运行。 每次管道运行都有唯一的管道运行 ID。 运行 ID 是标识该特定管道运行的全局唯一标识符 (GUID)。

管道运行通常通过将自变量传递给管道中定义的参数进行实例化。 你可以手动运行管道，也可以使用触发器来这样做。 有关详细信息，请参阅 [Azure 数据工厂中的管道执行和触发器](concepts-pipeline-execution-triggers.md)。

## <a name="common-issues-causes-and-solutions"></a>常见问题、原因和解决方案

### <a name="a-pipeline-run-is-canceled-but-the-monitor-still-shows-progress-status"></a>管道运行已取消，但监视器仍显示进度状态

很多情况下，取消管道运行后，管道监视仍然显示进度状态。 此问题是由浏览器缓存问题导致的。 你也可能没有合适的监视筛选器。

**解决方法**：刷新浏览器，并应用合适的监视筛选器。
 
### <a name="you-see-a-delimitedtextmorecolumnsthandefined-error-when-copying-a-pipeline"></a>复制管道时出现“DelimitedTextMoreColumnsThanDefined”错误
 
如果你要复制的文件夹包含的文件具有不同架构（例如可变列数、不同分隔符、引号字符设置或某个数据问题），则数据工厂管道可能会引发以下错误：

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

**解决方法**：创建复制活动时，选择“二进制复制”选项。 这样一来，若要将数据从一个数据湖批量复制或迁移到另一个数据湖，数据工厂不需打开文件来读取架构。 数据工厂会将每个文件视为二进制文件并将其复制到其他位置。

### <a name="a-pipeline-run-fails-when-you-reach-the-capacity-limit-of-the-integration-runtime"></a>当你达到集成运行时的容量限制时，管道运行会失败

错误消息：

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

**原因：** 已达到集成运行时的容量限制。 你可能在使用同一个集成运行时同时运行大量数据流。 有关详细信息，请参阅 [Azure 订阅和服务的限制、配额与约束](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#version-2)。

**解决方法**：
 
- 在不同的触发时间运行你的管道。
- 创建一个新的集成运行时，并将你的管道拆分到多个集成运行时中。

### <a name="you-have-activity-level-errors-and-failures-in-pipelines"></a>你在管道中遇到活动级错误和故障

Azure 数据工厂业务流程允许条件逻辑，使用户能够根据前面的活动的结果采用不同的路径。 它允许四个条件路径：“成功时（默认通过）”、“失败时”、“完成时”和“跳过时”。    

Azure 数据工厂会评估所有叶级活动的结果。 仅当所有叶都成功时，管道结果才是成功的。 如果跳过了某个叶活动，则会改为评估其父活动。 

**分辨率**

1. 按照[如何处理管道故障和错误](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459)一文的说明实施活动级检查。
1. 按照[通过工厂进行查询](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory)一文的说明使用 Azure 逻辑应用来定期监视管道。

## <a name="monitor-pipeline-failures-in-regular-intervals"></a>按固定时间间隔监视管道故障

你可能需要按特定时间间隔（例如 5 分钟）监视数据工厂管道。 可以使用终结点从数据工厂查询和筛选管道运行。 

将 Azure 逻辑应用设置为每隔 5 分钟查询一次所有故障管道，如[通过工厂进行查询](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory)一文所述。 然后，你可以将事件报告给我们的票证系统。

有关详细信息，请参阅[从数据工厂发送通知 - 第 2 部分](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/)。
