---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/12/2021
title: 查询筛选器 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的查询筛选器。
ms.openlocfilehash: bd7cf19bfcab142e83192f9a071188062ba68024
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692162"
---
# <a name="query-filters"></a>查询筛选器

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

借助查询筛选器，你能够以交互方式减少在可视化效果中显示的数据量，这与[查询参数](query-parameters.md)类似，但有一些关键区别。 查询筛选器会限制已加载到浏览器中的数据。 这使得筛选器非常适合较小的数据集以及查询执行很耗时、速率受限或成本高昂的环境。

若要关注特定值，请将列的别名指定为 `` `<columnName>::filter` ``。 下面是一个示例：

```sql
SELECT action AS `action::filter`, COUNT(0) AS "actions count"
FROM events
GROUP BY action
```

> [!NOTE]
>
> 如果外部数据源（例如 BigQuery）在列名中不支持 ``::``，可使用语法 ``__filter`` 或 ``__multiFilter``。

如果需要多选筛选器，请将列的别名指定为 ``<columnName>::multi-filter``。

```sql
SELECT action AS `action::multi-filter`, COUNT (0) AS "actions count"
FROM events
GROUP BY action
```

还可在仪表板上使用查询筛选器。 默认情况下，筛选器小组件会出现在已将筛选器添加到查询的每个可视化效果的旁边。 若要将筛选器小组件链接到仪表板级别查询筛选器，请参阅[仪表板筛选器](../dashboards/dashboards.md#dashboard-filters)。

## <a name="limitations"></a>限制

查询筛选器不适用于特别大的数据集或包含数百或数千个不同字段值的查询结果。 数据过多可能会使用户体验不佳，这具体取决于计算机和浏览器配置。