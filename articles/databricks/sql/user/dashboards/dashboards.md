---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 仪表板任务 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的仪表板任务。
ms.openlocfilehash: 3629cc5e6da65a4bcb9d2ec47303a73d20442f1f
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692363"
---
# <a name="dashboard-tasks"></a>仪表板任务

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

## <a name="create-a-dashboard"></a>创建仪表板

1. 单击 ![仪表板图标](../../../_static/images/icons/dashboards-icon.png) “模型”图标。
2. 单击“+ 新建仪表板”。
3. 输入仪表板的名称。
4. 将内容添加到仪表板：
   * 单击“添加文本框”以添加注释。
     1. 输入文字。 可使用 [Markdown](https://daringfireball.net/projects/markdown/syntax) 为文本框制定样式。 若要在仪表板的文本框中包含静态图像，请使用 Markdown 图像语法：``![alt-text](<image-url>)``。
   * 单击“添加小组件”，添加查询可视化效果。
     1. 选择查询。 搜索现有查询或从预先填充的列表中选取最近的查询。
     2. 在“选择可视化效果”下拉列表中，选择可视化效果类型。

        > [!div class="mx-imgBorder"]
        > ![添加到仪表板](../../../_static/images/sql/add-chart-widget.png)

5. 单击“添加到仪表板”。

   > [!div class="mx-imgBorder"]
   > ![将小组件添加到仪表板](../../../_static/images/sql/add-widget-to-dashboard.png)

6. 拖放仪表板上的内容块。
7. 单击“完成编辑”。

   > [!div class="mx-imgBorder"]
   > ![完成仪表板](../../../_static/images/sql/dashboard.png)

## <a name="remove-content-from-a-dashboard"></a>删除仪表板中的内容

1. 单击小组件右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“从仪表板中删除”。
2. 单击 **“删除”** 。

## <a name="edit-a-dashboard"></a>编辑仪表板

若要打开仪表板进行编辑，请打开仪表板右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“编辑”。

> [!div class="mx-imgBorder"]
> ![编辑仪表板](../../../_static/images/sql/edit-dashboard.png)

编辑时，可添加和删除内容并应用筛选器。

### <a name="add-content-to-a-dashboard"></a>将内容添加到仪表板

1. 打开仪表板进行[编辑](#edit-a-dashboard)。
2. 单击“添加文本框”或“添加小组件” 。
3. 单击“添加到仪表板”。
4. 单击“完成编辑”。

还可[将可视化效果添加到查询编辑器中的仪表板](../visualizations/visualizations.md#add-a-visualization-to-a-dashboard)。

### <a name="remove-content-from-a-dashboard"></a>删除仪表板中的内容

1. 单击 ![SQL 删除图标](../../../_static/images/sql/delete-icon.png) 或将鼠标悬停在对象上，单击小组件右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“从仪表板中删除”。
2. 单击 **“删除”** 。

### <a name="dashboard-filters"></a>仪表板筛选器

当查询[具有筛选器](../queries/query-filters.md)时，还必须在仪表板级别应用筛选器。 选择“使用仪表板级别筛选器”复选框以将筛选器应用到所有查询。

## <a name="refresh-a-dashboard"></a>刷新仪表板

由于仪表板从缓存中提取数据，而只要运行查询，就会更新缓存，因此仪表板的加载速度会很快。 但是，如果你最近没有运行查询，则仪表板可能已过时。 如果最近运行了某些查询而未运行其他查询，仪表板甚至可以将旧数据与新数据混合在一起。

### <a name="manually-refresh-a-dashboard"></a>手动刷新仪表板

若要强制刷新，请单击仪表板右上方的“刷新”按钮。 此操作将运行所有仪表板查询并更新其可视化效果。

### <a name="automatically-refresh-a-dashboard"></a>自动刷新仪表板

若要定期刷新仪表板，请选择“刷新”下拉列表并选择时间段：

> [!div class="mx-imgBorder"]
> ![刷新周期](../../../_static/images/sql/refresh-dashboard.png)

> [!NOTE]
>
> 只要登录用户在其浏览器中打开仪表板，刷新计划就会生效。 为了保证定期执行查询（这对警报很重要），应改为使用[计划查询](../queries/schedule-query.md)。

## <a name="publish-a-dashboard"></a>发布仪表板

若要共享仪表板，请单击仪表板编辑器右上方的“发布”按钮。 仪表板发布之后，组织中具有足够权限的任何登录成员都可以看到该仪表板。

## <a name="unpublish-a-dashboard"></a>取消发布仪表板

若要取消共享仪表板，请单击查询编辑器右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“取消发布”。

> [!div class="mx-imgBorder"]
> ![取消发布仪表板](../../../_static/images/sql/unpublish-dashboard.png)

## <a name="archive-a-dashboard"></a>存档仪表板

你无法删除 SQL Analytics 中的仪表板，但可以对其进行存档。 若要存档仪表板，请单击查询编辑器右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“存档”。

> [!div class="mx-imgBorder"]
> ![取消发布仪表板](../../../_static/images/sql/archive-dashboard.png)

## <a name="open-a-query"></a>打开查询

若要打开查询编辑器的小组件中显示的查询，请单击小组件右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“查看查询”。

## <a name="configure-dashboard-permissions"></a>配置仪表板权限

若要配置可以管理和运行仪表板的人员，请参阅[仪表板访问控制](../security/access-control/dashboard-acl.md)。