---
title: 清理 Azure 流分析作业
description: 本文演示用于删除 Azure 流分析作业的不同方法。
author: Johnnytechn
ms.author: v-johya
ms.service: stream-analytics
ms.topic: how-to
origin.date: 08/09/2018
ms.date: 01/25/2021
ms.custom: seodec18
ms.openlocfilehash: fbec7a572dcfd7d96a07cc0fa0e1ae1cd87c94b7
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059996"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>停止或删除 Azure 流分析作业

可以通过 Azure 门户、Azure PowerShell、Azure SDK for .Net 或 REST API 轻松地停止或删除 Azure 流分析作业。 流分析作业在删除后无法恢复。

>[!NOTE] 
>当停止流分析作业时，数据只会保留在输入和输出存储中，例如事件中心或 Azure SQL 数据库。 如果需要从 Azure 中删除数据，请务必遵循流分析作业的输入和输出资源删除过程进行操作。

## <a name="stop-a-job-in-azure-portal"></a>停止 Azure 门户中的作业

停止作业时，将取消预配资源并停止处理事件。 与此作业相关的收费也将停止。 但是，你的所有配置都会保留，你可以稍后重启作业 

1. 登录 [Azure 门户](https://portal.azure.cn)。 

2. 找到正在运行的流分析作业并选择它。

3. 在流分析作业页上，选择“停止”  以停止作业。 

   ![停止 Azure 流分析作业](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>删除 Azure 门户中的作业

>[!WARNING] 
>流分析作业在删除后无法恢复。

1. 登录到 Azure 门户。 

2. 找到现有流分析作业并选择它。

3. 在流分析作业页上，选择“删除”  以删除作业。 

   ![删除 Azure 流分析作业](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>使用 PowerShell 停止或删除作业

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

若要使用 PowerShell 停止作业，请使用 [Stop-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) cmdlet。 若要使用 PowerShell 删除作业，请使用 [Remove-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) cmdlet。

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>使用 Azure SDK for .NET 停止或删除作业

若要使用 Azure SDK for .NET 停止作业，请使用 [StreamingJobsOperationsExtensions.BeginStop](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop) 方法。 若要使用 Azure SDK for .NET 删除作业，请使用 [StreamingJobsOperationsExtensions.BeginDelete](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete) 方法。

## <a name="stop-or-delete-a-job-using-rest-api"></a>使用 REST API 停止或删除作业

若要使用 REST API 停止作业，请参阅[停止](https://docs.microsoft.com/rest/api/streamanalytics/2016-03-01/streamingjobs/stop)方法。 若要使用 REST API 删除作业，请参阅[删除](https://docs.microsoft.com/rest/api/streamanalytics/2016-03-01/streamingjobs/delete)方法。

<!-- Update_Description: update meta properties -->
