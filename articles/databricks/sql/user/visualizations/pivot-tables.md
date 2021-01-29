---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 透视表 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的透视表。
ms.openlocfilehash: 8099c6f7411f696d4f842a5e24e559e73561b7e0
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692267"
---
# <a name="pivot-tables"></a>透视表

透视表可视化效果会将查询结果中的记录聚合到新的表格显示中。 它类似于 SQL 中的 ``PIVOT`` 或 ``GROUP BY`` 语句。 可使用拖放字段（而不是 SQL 代码）来配置透视表可视化效果。

> [!IMPORTANT]
>
> 如果查询结果太大，透视表的性能可能会降低。 确切的大小阈值由访问 SQL Analytics 时使用的计算机和浏览器决定。 通常，在不超过 50,000 个字段的情况下性能最佳。 也就是说有 10,000 条记录，每条记录有 5 个字段， 或者有 1,000 条记录，每条记录有 50 个字段。

透视表的查询应至少返回 3 列。 透视表的源查询通常是非聚合或“融合”的。

以下示例显示了学校评分系统中的模拟数据。 查询不会对数据进行分组或排序。

> [!div class="mx-imgBorder"]
> ![透视表查询示例](../../../_static/images/sql/pivot-table-query.png)

## <a name="create-a-pivot-table-visualization"></a>创建透视表可视化效果

1. 单击“+添加可视化效果”。
2. 在“可视化效果类型”下拉菜单中，选择“透视表” 。 右侧的可视化效果预览会更新显示透视表。

   查询结果中的所有字段别名都将出现在透视表控制界面的顶部。 可将它们拖到行侧或列侧 。 也可对它们进行嵌套。

   下面是使用评分数据的一些示例：

   > [!div class="mx-imgBorder"]
   > ![透视示例](../../../_static/images/sql/pivot-table-configuration-examples.png)