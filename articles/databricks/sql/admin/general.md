---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 常规设置 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 的常规设置。
ms.openlocfilehash: 72e980e23c2cbe55b19c197e7c7fcc87e647d419
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692287"
---
# <a name="general-settings"></a>常规设置

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

常规设置可控制适用于组织中所有 Azure Databricks SQL Analytics 用户的 SQL Analytics 演示和行为。

只有 Azure Databricks [管理员](../../administration-guide/users-groups/users.md)可管理常规设置。

若要配置常规设置：

1. 单击边栏底部的![用户设置图标](../../_static/images/icons/user-settings-icon.png)图标，然后选择“设置”。
2. 单击“常规”选项卡。
   * **日期格式**：包含查询可视化效果中的默认日期格式。
   * **时间格式**：包含查询可视化效果中的默认时间格式。

   若要更改单个查询可视化效果的日期和时间格式，请参阅[常用数据类型](../user/visualizations/tables.md#common-data-types)。

   * **图表可视化效果**：切换是否在[图表可视化效果](../user/visualizations/index.md)中显示“Plotly”工具栏，并选择“隐藏 Plotly 模式栏”复选框。
   * **功能标记**：控制查询行为的各个方面：
     * 在计划查询失败时向查询所有者发送电子邮件
     * 启用多字节（中文、日语和韩语）搜索查询名称和说明（较慢）