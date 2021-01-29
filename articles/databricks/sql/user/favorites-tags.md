---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 收藏夹和标记 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 上的收藏夹和标记。
ms.openlocfilehash: 337d27b4afb83328d81c58d650edc8a55fb455ad
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692359"
---
# <a name="favorites-and-tags"></a>收藏夹和标签

可使用收藏夹和标记筛选在 SQL Analytics 登陆页面以及[查询](queries/index.md)和[仪表板](dashboards/index.md)列表中显示的查询和仪表板列表。

## <a name="favorites"></a>收藏夹

若要将查询或仪表板放入收藏夹，请在“查询”或“仪表板”列表中单击其标题左侧的星号。 星号将变为黄色。

## <a name="tags"></a>Tags

可使用对组织有意义的任何字符串标记查询和仪表板。

### <a name="add-a-tag"></a>添加标记

在查询和仪表板编辑器中添加标记。

1. 将鼠标悬停在查询或仪表板标题上。 这将显示“+添加标记”按钮。

   > [!div class="mx-imgBorder"]
   > ![添加标记](../../_static/images/sql/add-tag-button.png)

2. 单击“+添加标记”。 这将显示一个模式。
3. 若要创建标记，请输入标记字符串。

   > [!div class="mx-imgBorder"]
   > ![标记字符串](../../_static/images/sql/add-tag.png)

4. 若要应用标记，请单击标记名称。

   > [!div class="mx-imgBorder"]
   > ![应用标记](../../_static/images/sql/added-tag.png)

5. 单击“确定”以关闭模式。

### <a name="edit-tags"></a>编辑标记

1. 将鼠标悬停在查询或仪表板标题上。 必须向 ![编辑标记图标](../../_static/images/sql/edit-tag-icon.png) 图标。
2. 单击 ![编辑标记图标](../../_static/images/sql/edit-tag-icon.png) 图标。
3. 添加和删除标记。
4. 单击“确定”。

### <a name="filter-lists"></a>筛选器列表

标记显示在“查询”和“仪表板”列表的右侧。

* 单击标记可筛选列表。
* 再次单击可删除筛选器。
* 若要选择多个筛选器，请选择“Shift + 单击”。