---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 可视化任务 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 上的可视化任务。
ms.openlocfilehash: 1ddc2033e5b881dc7f0f01afe24d68c97e92c3fe
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692246"
---
# <a name="visualization-tasks"></a>可视化任务

## <a name="create-a-visualization"></a>创建可视化效果

1. 单击结果表上方的“+ 添加可视化效果”按钮。

   > [!div class="mx-imgBorder"]
   > ![添加可视化效果](../../../_static/images/sql/add-visualization.png)

2. 可选择配置轴、轴、标题和其他属性。

   > [!div class="mx-imgBorder"]
   > ![配置图表](../../../_static/images/sql/configure-chart.png)

3. 单击“保存” 。

   > [!div class="mx-imgBorder"]
   > ![保存图表](../../../_static/images/sql/mary-chart.png)

## <a name="visualization-tools"></a>可视化工具

如果将鼠标悬停在图表可视化效果的右上方，则会显示一个 Plotly 工具栏，可在其中执行“选择”、“缩放”和“平移”等操作。

> [!div class="mx-imgBorder"]
> ![Plotly 工具栏](../../../_static/images/sql/plotly-bar.png)

要隐藏所有图表的 Plotly 工具栏，请更改 [Plotly 工具栏设置](../../admin/general.md)。

## <a name="edit-a-visualization"></a>编辑可视化效果

可在查询编辑器中修改可视化效果的设置。

1. 单击选项卡栏上的可视化效果。
2. 单击可视化效果下方的“编辑可视化效果”按钮。

   > [!div class="mx-imgBorder"]
   > ![编辑可视化效果](../../../_static/images/sql/edit-visualization.png)

   该可视化效果的当前设置（如类型、X 轴、Y 轴和分组）随即显示。

3. 编辑设置。
4. 单击“保存”应用所做的更改，或单击“取消” 。

## <a name="add-a-visualization-to-a-dashboard"></a>将可视化效果添加到仪表板

1. 单击可视化效果下方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png) 按钮。
2. 选择“添加到仪表板”。
3. 输入仪表板名称。 匹配的仪表板列表随即显示。
4. 选择仪表板。

   > [!div class="mx-imgBorder"]
   > ![选择仪表板](../../../_static/images/sql/choose-dashboard.png)

5. 单击“确定”。 将显示一个弹出窗口，其中包含指向仪表板的链接。

   > [!div class="mx-imgBorder"]
   > ![已添加到仪表板](../../../_static/images/sql/added-to-dashboard.png)

## <a name="download-a-chart-visualization-as-an-image-file"></a>将图表可视化效果下载为图像文件

要下载图表可视化效果的本地图像文件，请显示[可视化工具](#visualization-tools)，然后单击照相机图标。

> [!div class="mx-imgBorder"]
> ![将可视化效果下载为图像](../../../_static/images/sql/download-visualization.png)

一个 png 文件随即下载到设备上。