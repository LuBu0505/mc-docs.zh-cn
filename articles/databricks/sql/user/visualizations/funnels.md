---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 漏斗图可视化效果 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 上的漏斗图可视化效果。
ms.openlocfilehash: 3efee075cd6452b9f4ba00a075953b814b7520ce
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692268"
---
# <a name="funnel-visualization"></a>漏斗图可视化效果

漏斗图可视化效果在“可视化效果类型”菜单上提供。 它只需要查询中的 ``step`` 和 ``value`` 这两个字段，因此很容易实现。

> [!div class="mx-imgBorder"]
> ![漏斗](../../../_static/images/sql/funnel-example.png)

在前面的示例中，数据如下所示：

> [!div class="mx-imgBorder"]
> ![漏斗图数据](../../../_static/images/sql/funnel-data.png)