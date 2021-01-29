---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/07/2021
title: 每工作区 URL - Azure Databricks
description: 了解 Azure Databricks 工作区 URL，学习如何开始使用唯一的每工作区 URL。
ms.openlocfilehash: 4737c3f6944a30da1b21f4e4be5398f01aa69d6e
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692310"
---
# <a name="per-workspace-urls"></a>每工作区 URL

2020 年 4 月，Azure Databricks 为每个工作区添加了新的唯一每工作区 URL。 此每工作区 URL 采用以下格式

``adb-<workspace-id>.<random-number>.databricks.azure.cn``

每工作区 URL 替换已弃用的区域 URL (``<region>.databricks.azure.cn``) 以访问工作区。

> [!IMPORTANT]
>
> 不要使用旧的区域 URL。 它们可能不适用于新的工作区、可靠性更低，而且性能比每工作区 URL 的低。

## <a name="launch-a-workspace-using-the-per-workspace-url"></a>使用每工作区 URL 启动工作区

在 Azure 门户中，转到工作区的 Azure Databricks 服务资源页面，单击“启动工作区”，或者复制此资源页上显示的每工作区 URL 并将其粘贴到浏览器地址栏。

> [!div class="mx-imgBorder"]
> ![资源页](../_static/images/workspace/resource-per-workspace-url.png)

## <a name="get-a-per-workspace-url-using-the-azure-api"></a>使用 Azure API 获取每工作区 URL

使用 Azure API [工作区 - Get](https://docs.microsoft.com/rest/api/databricks/workspaces/get) 终结点获取工作区详细信息，包括每工作区 URL。 每工作区 URL 在响应对象的 ``properties.workspaceUrl`` 字段中返回。

## <a name="migrate-your-scripts-to-use-per-workspace-urls"></a>迁移脚本以使用每工作区 URL

Azure Databricks 用户通常采用下面两种方式之一编写脚本或其他自动化来引用工作区：

* 可在同一区域中创建所有工作区，并在脚本中对旧区域 URL 进行硬编码。

  由于每个工作区都需要一个 API 令牌，因此你还会有一个令牌列表，它存储在脚本本身或其他数据库中。 如果是这种情况，建议存储 ``<per-workspace-url, api-token>`` 对的列表，并删除所有硬编码的区域 URL。

* 可在一个或多个区域中创建工作区，并将 ``<regional-url, api-token>`` 对的列表存储在脚本本身或某数据库中。 如果是这种情况，建议将每工作区 URL 而非区域 URL 存储在列表中。

> [!NOTE]
>
> 由于区域 URL 和每工作区 URL 均受支持，因此任何现有自动化（它们使用区域 URL 引用在引入每工作区 URL 之前创建的工作区）都将继续运作。 尽管 Databricks 建议你更新所有自动化来使用每工作区 URL，但此情况下无需这样做。

## <a name="find-the-legacy-regional-url-for-a-workspace"></a>查找工作区的旧区域 URL

如果需要查找工作区的旧区域 URL，请在每工作区 URL 上运行 ``nslookup``。

```bash
$ nslookup adb-<workspace-id>.<random-number>.databricks.azure.cn
Server:   192.168.50.1
Address:  192.168.50.1#53

Non-authoritative answer:
adb-<workspace-id>.<random-number>.databricks.azure.cn canonical name = eastus-c3.databricks.azure.cn.
Name: eastus-c3.databricks.azure.cn
Address: 20.42.4.211
```