---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/24/2020
title: 快速入门 - 运行和可视化查询 - Azure Databricks
description: Azure Databricks SQL Analytics 中的用户任务快速入门。
ms.openlocfilehash: 1830b0bae71126542cf5d1725b44f6c9587f016c
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692277"
---
# <a name="quickstart-run-and-visualize-a-query"></a>快速入门：运行查询并将其可视化

本快速入门介绍如何基于 [Databricks 数据集](../../data/databricks-datasets.md)创建一个包含 1000 万人的表，查询该表来查找名为 ``Mary`` 的按出生年份分组的女性人数，并直观显示结果。

## <a name="requirements"></a>要求

SQL Analytics 管理员必须完成 SQL Analytics [管理员快速入门](../admin/admin-quickstart.md)。

## <a name="step-1-create-a-table-of-10-million-people"></a>步骤 1：创建一个包含 1000 万人的表

1. 登录 SQL Analytics。

   如果既可访问 Azure Databricks 工作区又可访问 SQL Analytics，则可能需要从工作区切换到 SQL Analytics。 在边栏底部，单击 ![应用切换器图标](../../_static/images/icons/app-switcher-icon.png) 应用切换器图标，然后选择 ![应用切换器 - SQL Analytics](../../_static/images/icons/app-sql.png)

2. 单击 ![边栏中的](../../_static/images/icons/queries-icon.png) “模型”图标。
3. 单击“+新建查询”。

   此时会显示查询编辑器。

4. 在“新建查询”下面的框中，单击 ![向下箭头图标](../../_static/images/sql/down-arrow-icon.png) 图标，然后选择“QS 终结点” 。

   > [!div class="mx-imgBorder"]
   > ![查询编辑器](../../_static/images/sql/qs-query-editor.png)

   第一次创建查询时，可用 SQL 终结点的列表按字母顺序显示。 后面创建查询时，将选择上次使用的终结点。

5. 在终结点下面的框中，单击 ![向下箭头图标](../../_static/images/sql/down-arrow-icon.png) 图标；如果未选中，请选择默认数据库。

   > [!div class="mx-imgBorder"]
   > ![默认数据库](../../_static/images/sql/default-database.png)

6. 将以下内容粘贴到查询编辑器中：

   ```sql
    CREATE TABLE default.people10m OPTIONS (PATH 'dbfs:/databricks-datasets/learning-spark-v2/people/people-10m.delta')
   ```

   此语句使用存储在 Databricks 数据集中的 Delta Lake 文件创建增量表。

7. 按 Ctrl/Cmd + Enter 或单击“执行”按钮 。 此查询将返回 ``No data was returned.``
8. 若要刷新架构，请单击架构浏览器底部的 ![刷新架构](../../_static/images/sql/refresh-schema.png) 按钮。
9. 在数据库右侧的文本框中键入 ``peo``。 架构浏览器将显示新表。

   > [!div class="mx-imgBorder"]
   > ![架构浏览器](../../_static/images/sql/qs-table.png)

## <a name="step-2-query-the-table"></a>步骤 2：查询该表

1. 粘贴查询名为 ``Mary`` 的女性人数的 ``SELECT`` 语句：

   ```sql
    SELECT year(birthDate) as birthYear, count(*) AS total
    FROM default.people10m
    WHERE firstName = 'Mary' AND gender = 'F'
    GROUP BY birthYear
    ORDER BY birthYear
   ```

2. 按 Ctrl/Cmd + Enter 或单击“执行”按钮 。

   > [!div class="mx-imgBorder"]
   > ![执行查询](../../_static/images/sql/execute-query.png)

   “限制 1000”复选框已默认选中，以确保查询最多返回 1000 行。 如果需要更多行，可取消选中此复选框，并在查询中指定 ``LIMIT`` 子句。 查询结果将显示在“表”选项卡中。

   > [!div class="mx-imgBorder"]
   > ![查询结果](../../_static/images/sql/query-result.png)

## <a name="step-3-create-a-visualization"></a>步骤 3：创建可视化效果

1. 单击“+添加可视化效果”选项卡。

   这会显示可视化效果编辑器。

   > [!div class="mx-imgBorder"]
   > ![可视化效果编辑器](../../_static/images/sql/visualization-editor.png)

2. 在“X 列”下拉列表中，选择“出生年” 。
3. 在“Y 列”下拉列表中，选择“总计” 。
4. 单击“X 轴”选项卡。
5. 在“名称”字段中，输入 ``Birth Year``。
6. 单击“Y 轴”选项卡。
7. 在“名称”字段中，输入 ``Number of Marys``。
8. 单击“保存” 。

   保存的图表显示在查询编辑器中。

   > [!div class="mx-imgBorder"]
   > ![Mary 人数图](../../_static/images/sql/marys-chart-qs.png)