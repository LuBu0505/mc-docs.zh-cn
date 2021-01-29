---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 查询任务 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中的查询任务。
ms.openlocfilehash: 0fb147af89deefea72621929c06610484e6effd1
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692167"
---
# <a name="query-tasks"></a>查询任务

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

## <a name="create-a-query"></a>创建查询

1. 单击 ![边栏中的](../../../_static/images/icons/queries-icon.png) “模型”图标。
2. 单击“+新建查询”。

   此时会显示查询编辑器。

3. 在“新建查询”下面的框中，单击 ![向下箭头图标](../../../_static/images/sql/down-arrow-icon.png) 图标，然后选择 [SQL 终结点](../../admin/sql-endpoints.md)或[外部数据源](../../admin/external-data-sources/index.md)。 若要筛选列表，请在文本框中输入文本。

   > [!div class="mx-imgBorder"]
   > ![选择连接](../../../_static/images/sql/query-connection.png)

   第一次创建查询时，可用连接列表按字母顺序显示。 后面创建查询时，将选择上次使用的连接。

   连接旁的图标表示状态为：

   * 运行 ![运行](../../../_static/images/sql/endpoint-running.png)
   * 正在启动 ![正在启动](../../../_static/images/sql/endpoint-starting.png)
   * 已停止 ![已停止](../../../_static/images/sql/endpoint-stopped.png)

   > [!NOTE]
   >
   > 如果没有连接，请与 SQL Analytics 管理员联系。

4. 在连接下面的框中，单击 ![向下箭头图标](../../../_static/images/sql/down-arrow-icon.png) 图标，并选择一个数据库。 如果具有元数据读取权限，则架构浏览器会显示可用的表。

   > [!div class="mx-imgBorder"]
   > ![默认数据库](../../../_static/images/sql/default-database.png)

   > [!NOTE]
   > * 必须选择正在运行的连接。
   > * 并非所有连接类型都支持加载架构。
   >
   > 如果具有元数据读取权限，则架构浏览器会显示可用的表。 若要刷新架构，请单击 ![刷新架构](../../../_static/images/sql/refresh-schema.png) 按钮。

   可在搜索框中键入筛选器字符串来筛选架构：

   > [!div class="mx-imgBorder"]
   > ![筛选架构](../../../_static/images/sql/filter-schema.png)

5. 单击一个表来显示表中的列。

   > [!div class="mx-imgBorder"]
   > ![表列](../../../_static/images/sql/table-columns.png)

6. 若要将某项插入到查询中，请单击右侧的双箭头。

   > [!div class="mx-imgBorder"]
   > ![插入架构项](../../../_static/images/sql/insert-from-schema.png)

7. 完成查询。
8. 单击“保存” 。

## <a name="execute-a-query"></a>执行查询

若要执行查询：

1. 选择 SQL 终结点。
2. 在查询编辑器中指定一个查询。
3. 按 Ctrl/Cmd + Enter 或单击“执行”按钮 。

   > [!div class="mx-imgBorder"]
   > ![执行查询](../../../_static/images/sql/execute-query.png)

> [!NOTE]
>
> * 如果终结点已停止，则你执行查询时终结点将启动。 若要手动启动终结点，请按照[启动终结点](../../admin/sql-endpoints.md#start)中的步骤操作。
> * “限制 1000”复选框已默认选中，以确保查询最多返回 1000 行。 如果知道有更多的行，可取消选中此复选框，并在查询中指定 ``LIMIT`` 子句。

查询结果将显示在“表”选项卡中。

> [!div class="mx-imgBorder"]
> ![执行查询](../../../_static/images/sql/query-result.png)

## <a name="refresh-a-query"></a>刷新查询

如果查询有[计划](schedule-query.md)，则它会自动刷新。 要在自动刷新间隙手动刷新查询，请单击“刷新”按钮。

> [!div class="mx-imgBorder"]
> ![刷新查询](../../../_static/images/sql/refresh-query.png)

## <a name="save-a-query"></a>保存查询

若要保存查询，请按 Ctrl/Cmd + S 或单击“保存”按钮 。 只有已保存的查询才会显示在[查询](index.md)列表中。

## <a name="publish-and-unpublish-a-query"></a>发布和取消发布查询

每个查询最初都是一个名为“新查询”的未发布草稿。

若要在[仪表板](../dashboards/index.md)和[警报](../alerts/index.md)中使用查询，必须将其发布。 若要发布查询，请更改其名称或单击“发布”按钮。

若要切换已发布状态，请打开查询编辑器右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“取消发布”。

> [!div class="mx-imgBorder"]
> ![取消发布查询](../../../_static/images/sql/unpublish-query.png)

取消发布查询不会将其从仪表板和警报中删除，但你无法将其添加到任何其他仪表板或警报。

> [!NOTE]
>
> 发布或取消发布查询不会影响其可见性。 你组织中的所有查询对所有已登录的用户来说都是可见的。

## <a name="archive-a-query"></a>存档查询

无法删除 SQL Analytics 中的查询，但可对其进行存档。 存档与删除类似，只是保留了指向查询的直接链接。

若要存档查询，请单击查询编辑器右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“存档”。

> [!div class="mx-imgBorder"]
> ![存档查询](../../../_static/images/sql/archive-query.png)

## <a name="copy-a-query"></a>复制查询

若要创建查询副本（该查询由你或其他人创建），可为其创建分支。 若要为查询创建分支，请单击查询编辑器右上方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png)，然后选择“创建分支”：

> [!div class="mx-imgBorder"]
> ![为查询创建分支](../../../_static/images/sql/fork-query.png)

## <a name="download-a-query-result"></a>下载查询结果

1. 单击结果窗格下方的垂直省略号 ![垂直省略号](../../../_static/images/sql/vertical-ellipsis.png) 按钮。
2. 选择“下载为 [CSV | TSV | Excel] 文件”。

> [!div class="mx-imgBorder"]
> ![下载查询结果](../../../_static/images/sql/download-dataset.png)

## <a name="query-editor-tools"></a>查询编辑器工具

### <a name="schema-browser"></a>架构浏览器

若要切换架构浏览器，请按 Alt/Option + D，或者单击架构浏览器和查询窗格之间的窗格句柄 ![窗格句柄](../../../_static/images/sql/pane-handle.png)。

### <a name="auto-complete"></a>自动完成

查询编辑器具有自动补全功能，可提高查询编写的速度。 自动补全功能可补全架构令牌、查询语法标识符（如 ``SELECT`` 和 ``JOIN``）和[查询片段](query-snippets.md)的标题。

自动补全功能默认启用，除非你的数据库架构超过 5000 个令牌（表或列）。

* 若要禁用自动补全功能，请按“Ctrl + 空格”，或者单击查询编辑器下方的 ![已启用自动补全](../../../_static/images/sql/auto-complete-enabled.png) 按钮：
* 若要启用自动补全功能，请按“Ctrl + 空格”，或者单击查询编辑器下方的 ![已禁用自动补全](../../../_static/images/sql/auto-complete-disabled.png) 按钮。

## <a name="configure-query-permissions"></a>配置查询权限

若要配置可管理和运行查询的人员，请查看[查询访问控制](../security/access-control/query-acl.md)。