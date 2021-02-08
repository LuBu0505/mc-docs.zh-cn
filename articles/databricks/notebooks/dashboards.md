---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 07/14/2020
title: 仪表板 - Azure Databricks
description: 了解如何发布从笔记本输出派生的图和可视化效果，以及如何将其以演示格式与组织共享。
ms.openlocfilehash: f0ba43d8ad9a9d655010393a9c67827cebbd26ed
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058583"
---
# <a name="dashboards"></a>仪表板

通过仪表板可以发布从笔记本输出派生的图和可视化效果，并将其以演示格式与组织共享。 查看笔记本以了解如何创建和组织仪表板。 其余部分介绍如何计划作业以刷新仪表板以及如何查看特定仪表板版本。

## <a name="dashboards-notebook"></a>仪表板笔记本

[获取笔记本](../_static/notebooks/dashboards.html)

## <a name="create-a-scheduled-job-to-refresh-a-dashboard"></a>创建计划作业以刷新仪表板

通过仪表板视图呈现仪表板时，仪表板不会实时刷新。 若要对仪表板进行计划，使之按特定的时间间隔刷新，请对生成仪表板图的[笔记本进行计划](notebooks-manage.md#schedule-notebook)。

## <a name="view-a-specific-dashboard-version"></a>查看特定的仪表板版本

1. 单击 ![计划图标](../_static/images/notebooks/schedule.png) 按钮。
2. 单击计划按所需时间间隔运行的笔记本作业的“上次成功运行”链接。

   > [!div class="mx-imgBorder"]
   > ![查看仪表板版本](../_static/images/dashboards/dashboard-run.png)