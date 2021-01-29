---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 图表 - Azure Databricks
description: 了解有关 Azure Databricks SQL Analytics 的图表。
ms.openlocfilehash: 688a2b1ed0e495dff2717590666da4541d4be631
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692275"
---
# <a name="charts"></a>图表

SQL Analytics 将使用 X 和 Y 轴的图表捆绑到“图表”可视化类型中。 此可视化效果可以有八种不同的形式。 由于形式是相似的，因此你可以经常在它们之间无缝切换，找到一个最能表达你的意图的形式。 在下面的动画中，所有八个图表都是基于相同的 SQL 查询结果生成的：

> [!div class="mx-imgBorder"]
> ![图表示例](../../../_static/images/sql/gifs/visualization/chart-examples.gif)

## <a name="setup"></a>设置

查询应至少返回两列：一列为 X 轴的值，一列为 Y 轴的值 。 它还可以返回跟踪[分组](#grouping)的值，显示[误差线](#error-bars)，以及气泡大小。

前面动画中的图表都是基于以下表格结果生成的：

> [!div class="mx-imgBorder"]
> ![表格数据](../../../_static/images/sql/animation-table-data.png)

查询返回正确的列后，就从设置 X 和 Y 轴值开始。 可视化预览会立即更新。 无需保存可视化效果即可查看更改对其外观产生的影响。 可使用“可视化效果设置”屏幕顶部的选项卡对图表其余部分进行细粒度控制。

使用 X 轴和 Y 轴选项卡可修改轴范围和标签 。

“系列”选项卡功能强大。 可使用该选项更改数据别名、Z-索引行为、在左右两个 Y 轴之间分配跟踪。 还可通过该选项在一个图表上组合不同的跟踪形式，如下图所示。

> [!div class="mx-imgBorder"]
> ![“序列”选项卡](../../../_static/images/sql/multi-form-chart.png)

“颜色”提供了一个颜色选取器，用于更改图表上跟踪的外观。

“数据标签”控制将鼠标悬停在图表上时显示的内容。

## <a name="grouping"></a>分组

“分组依据”设置可以针对相同的 X 和 Y 轴生成多个跟踪。 它通过将记录分组到不同的跟踪而不是绘制折线图来实现这一点。 几乎每次在 SQL Analytics 图表中看到多个颜色的折线图或条形图时，都是因为查询结果包含一个分组列。

如下面的示例所示，分组列用于将 ``(x,y)`` 对排序在一起。

> [!div class="mx-imgBorder"]
> ![按示例分组](../../../_static/images/sql/group-by-ex.png)

“分组依据”通常比编写为 X 值返回多个 Y 列的查询更容易。 以下两个数据集是相同的。

> [!div class="mx-imgBorder"]
> ![已分组与透视](../../../_static/images/sql/grouped-vs-pivot.png)

> [!NOTE]
>
> 对融合的数据集使用“分组依据”列。 对透视数据集使用多个 Y 列。

## <a name="stacking"></a>堆叠

SQL Analytics 可以将 Y 轴值“堆叠”在另一个值之上。 名称是从[堆叠的条形图]借用的，但它也可以用于面积图。 下图显示了相同的数据，左侧未堆叠，右侧堆叠。

> [!div class="mx-imgBorder"]
> ![堆叠](../../../_static/images/sql/stacked_vs_not_stacked.png)

注意看看每个 Y 轴值是如何显示为其自身和其“下方”Y 值的总和的。

> [!NOTE]
>
> 堆栈和分组是相互关联的。 除非还对数据进行了分组，否则不会堆叠数据。

可使用可视化编辑器的“系列”选项卡来控制跟踪的堆叠顺序。 还可通过向查询中添加 ``ORDER BY`` 语句来控制它。 堆叠遵循组名首次出现在查询结果中的顺序。 堆叠仅适用于折线图、条形图和面积图。

## <a name="error-bars"></a>误差线

对于某些图表形式，SQL Analytics 可以使用查询结果中的值在数据点周围绘制误差线。 关于误差线，有几点始终是真的：

* 误差线始终对称。 给定 ``(x,y)`` 对上下方的距离始终相同。
* 误差的颜色与目标跟踪的颜色相同
* 显示所有跟踪或无跟踪的误差。 应将它们配置为显示在所有跟踪上。
* “误差”列中的值将绘制在与其关联跟踪相同的轴上。 这意味着，误差值必须是绝对的。 例如，对于以百表示的 Y 值，不能以百分比表示误差。

> [!div class="mx-imgBorder"]
> ![误差线](../../../_static/images/sql/area_grouped_stacked_errors.png)

还要记住，堆叠记录时，不会聚合误差。 每个跟踪都会显示一条误差线。 可通过只为应突出显示误差的那些记录提供非零误差值来解决这个问题。 在前面的示例中，每个跟踪点都会显示一条水平误差线。 但只有 ``Paid`` 跟踪误差线具有任意长度。

## <a name="chart-forms"></a>图表形式

每种图表形式都可用于某些类型的呈现。 你可以根据需要在同一图表上混合和匹配多种形式。

* 折线图几乎只用于表示一个或多个指标随时间的变化。
* 条形图可用于表示指标随时间的变化，也可以像饼图一样显示比例。 条形图可以与[堆叠]结合使用，并且呈现效果非常好。
* 面积图通常用于显示销售漏斗图随时间的变化。 它们经常与[堆叠]结合在一起，以显示广阔的画面。
* 饼图旨在显示指标之间的比例关系。 它们不是用来传输时序数据的。
* 散点图用于显示多组数据点。 在封面下，散点图就像折线图，但没有连接线。 散点图更精确，但对时序数据不太有用。

  > [!NOTE]
  >
  > 散点图对于某些组只出现一次的可视化效果是必需的。 折线图不显示单一数据库值，因为它只能显示存在两个或多个点的数据。 一种选择是，在可视化编辑器的“系列”选项卡上将单一数据库强制为散点形式，同时将其他跟踪保持为线条形式。

* 气泡图是散点图，其中每个点标记的大小反映了相关的指标。
* 热度地图可视化效果融合了条形图、堆叠图和气泡图的功能。 有几种内置的配色方案可供选择。 不能对热度地图进行分组，因为从技术上而言，整个图表就是一个跟踪。
* 盒须图可以自动显示数据点在分组类别中的分布。

## <a name="common-mistakes"></a>常见错误

### <a name="multiple-records-per-x-axis-value"></a>每个 X 轴值多条记录

如果查询返回具有相同 X 轴值的两行或多行，则 SQL Analytics 可能会生成一些模糊的形状。 如果无意中 ``JOIN`` 一个具有一对多关系的表，SQL 中通常就会出现这种情况。

> [!div class="mx-imgBorder"]
> ![双重条目](../../../_static/images/sql/error_double_entries.png)

在本例中，因为 1 月 1 日有两条记录，所以绘制了一条竖线。 可以通过筛选掉 X 轴上的双重条目来解决这个问题，或者修改查询以包含[分组](#grouping)字段，如下图所示。

> [!div class="mx-imgBorder"]
> ![筛选双重条目](../../../_static/images/sql/error_double_entries__solved.png)

### <a name="unordered-x-axis-records"></a>无序 X 轴记录

SQL Analytics 非常智能，可以计算出最常见的 X 轴刻度：时间戳、线性和对数。 但如果它不能将 X 列解析为有序系列，它就会将每个 X 值作为一个“类别”来处理。 这可能会产生混合的结果：

> [!div class="mx-imgBorder"]
> ![无序 x 轴](../../../_static/images/sql/charted_redash_logo__broken.png)

如果你看到不期望的形状，可以检查 X 轴是否已经在可视化编辑器的“X 轴”选项卡上排序。 只需切换“对值进行排序”选项。 如果已禁用“对值进行排序”，则 SQL Analytics 将保留源查询的顺序。

> [!div class="mx-imgBorder"]
> ![对值进行排序](../../../_static/images/sql/charted_redash_logo__working.png)

这两个图表来自同一基础数据。 唯一的区别是 SQL Analytics 是否对 X 轴值进行排序。