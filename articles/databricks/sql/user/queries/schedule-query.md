---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 计划查询 - Azure Databricks
description: 了解如何在 Azure Databricks SQL Analytics 中计划查询。
ms.openlocfilehash: bd604f38568487ee8fe1ac8464ac72400b862d38
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692197"
---
# <a name="schedule-a-query"></a>计划查询

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

可使用计划的查询执行来使仪表板保持最新状态或为例程警报提供支持。 默认情况下，你的查询没有计划。 在查询编辑器的左下角，你将看到计划区域：

> [!div class="mx-imgBorder"]
> ![刷新设置](../../../_static/images/sql/refresh-settings.png)

要设置计划，请单击“从不”，打开包含允许的计划间隔的选取器。

> [!div class="mx-imgBorder"]
> ![计划间隔](../../../_static/images/sql/schedule-modal.png)

设置计划，然后单击“确定”。 设置计划后，查询将自动运行。

> [!NOTE]
>
> 如果将查询计划为在一天中的特定时间运行，SQL Analytics 会使用计算机的本地时区将你的选择转换为 UTC。 这意味着，如果你希望查询在特定 UTC 时间运行，则必须按本地偏移量调整选取器。
>
> 例如，如果希望每天 ``00:00`` (UTC) 执行一次查询，但当前时区为 CDT (UTC-5)，则应在计划程序中输入 ``19:00``。 UTC 值显示在你选择的值右侧，以帮助确认你的选择正确。

> [!IMPORTANT]
>
> 不支持使用[参数](query-parameters.md)的计划查询。

## <a name="scheduled-query-failure-reports"></a>计划查询失败报告

如果一个或多个查询失败，则 SQL Analytics 会每小时向查询所有者发送一次电子邮件。 这些电子邮件将持续发送，直到没有失败的查询为止。 失败报告电子邮件在与实际查询计划无关的进程上运行。 执行查询失败后，SQL Analytics 可能在最长一小时后才会发送失败报告。

要切换失败报告，请参阅[常规设置](../../admin/general.md)。