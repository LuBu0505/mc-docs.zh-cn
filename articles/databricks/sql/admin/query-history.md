---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 查询历史记录 - Azure Databricks
description: 了解如何使用 Azure Databricks SQL Analytics 查询历史记录以进行故障排除。
ms.openlocfilehash: feec8b77a803195fb01135f90dbbdce281d675f6
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692282"
---
# <a name="query-history"></a>查询历史记录

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

查询历史记录显示使用 [SQL 终结点](sql-endpoints.md)执行的 SQL 查询。

你可以使用此屏幕提供的信息来帮助调试查询问题。

此部分介绍如何通过 UI 来处理查询历史。 若要使用 API 处理查询历史记录，请参阅[查询历史记录 API](../api/query-history.md)。

## <a name="view-query-history"></a>查看查询历史记录

若要查看查询历史记录，请执行以下操作之一：

* 单击 ![历史记录图标](../../_static/images/icons/history-icon.png) “模型”图标。
* 单击侧栏中的 ![终结点图标](../../_static/images/icons/endpoints-icon.png) 图标。 在“操作”列表中，单击垂直省略号 ![垂直省略号](../../_static/images/sql/vertical-ellipsis.png)，然后选择“历史记录”。

你可以按用户、日期范围、终结点类型和查询状态筛选列表。

> [!div class="mx-imgBorder"]
> ![查询历史记录](../../_static/images/sql/query-history.png)

## <a name="view-query-details"></a>查看查询详细信息

1. 查看[查询历史记录](#view-query-history)。
2. 单击“查询”列中的字符串可显示查询详细信息：

> [!div class="mx-imgBorder"]
> ![查询历史记录](../../_static/images/sql/query-details.png)

在“详细信息”字段中，单击“打开”链接以显示查询计划 。

> [!NOTE]
>
> “打开”链接仅适用于在具有”可以管理”[权限](../user/security/access-control/sql-endpoint-acl.md)的终结点上执行的查询 。

> [!div class="mx-imgBorder"]
> ![查询历史记录](../../_static/images/sql/query-plan.png)

## <a name="view-query-metrics"></a>查看查询指标

1. 查看[查询历史记录](#view-query-history)。
2. 单击“查询”列中的字符串以显示查询详细信息。
3. 单击“执行详细信息”选项卡以显示查询指标。

   > [!div class="mx-imgBorder"]
   > ![查询指标](../../_static/images/sql/query-metrics.png)