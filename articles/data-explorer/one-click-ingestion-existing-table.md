---
title: 使用一键式引入将 JSON 数据从本地文件引入到 Azure 数据资源管理器中的现有表
description: 使用一键式引入轻松地将数据引入（载入）现有的 Azure 数据资源管理器表中。
author: orspod
ms.author: v-junlch
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: how-to
ms.date: 03/18/2021
ms.openlocfilehash: 42330ca7f2097ddb1167ab74afc67e69275efcc5
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767835"
---
# <a name="use-one-click-ingestion-to-ingest-json-data-from-a-local-file-to-an-existing-table-in-azure-data-explorer"></a>使用一键式引入将 JSON 数据从本地文件引入到 Azure 数据资源管理器中的现有表


> [!div class="op_single_selector"]
> * [将 CSV 数据从容器引入新表](one-click-ingestion-new-table.md)
> * [将 JSON 数据从本地文件引入现有表](one-click-ingestion-existing-table.md)

借助[一键式引入](ingest-data-one-click.md)，可将 JSON、CSV 和其他格式的数据快速引入表中并轻松创建映射结构。 数据可以从存储、本地文件或容器引入，可以是一次性引入过程，也可以是持续引入过程。  

本文档介绍如何在特定用例中使用直观的一键式向导将本地文件中的 JSON 数据引入到现有表中  。 使用稍微改动的同一过程来覆盖各种不同的用例。

有关一键式引入的概述和先决条件列表，请参阅[一键式引入](ingest-data-one-click.md)。
如需了解数据的不同类型或源，请参阅[使用一键式引入将 CSV 数据从容器引入到 Azure 数据资源管理器中的新表](one-click-ingestion-new-table.md)。

## <a name="ingest-new-data"></a>引入新数据

在 Web UI 的左侧菜单中，右键单击“数据库”或“表”，然后选择“引入新数据” 。

   :::image type="content" source="media/one-click-ingestion-existing-table/one-click-ingestion-in-webui.png" alt-text="在 Web UI 中选择一键式引入":::
 
## <a name="select-an-ingestion-type"></a>选择引入类型

1. 在“引入新数据”窗口中，“源”选项卡处于选中状态 。

1. 如果“群集”和“数据库”字段未自动填充，请从下拉菜单中选择一个现有群集和数据库名称 。
    
    [!INCLUDE [one-click-cluster](includes/one-click-cluster.md)]

1. 如果“表”字段未自动填充，请从下拉菜单中选择一个现有的表名。

1. 在“源类型”下，执行以下步骤：

   1. 选择“从文件”  
   1. 选择“浏览”查找文件（最多 10 个），或将文件拖到字段中。 可以使用蓝色星形来选择架构定义文件。
    
      :::image type="content" source="media/one-click-ingestion-existing-table/from-file.png" alt-text="从文件进行一键式引入":::

## <a name="edit-the-schema"></a>编辑架构

选择“编辑架构”以查看并编辑表列配置。 在“架构”选项卡中：

   * 压缩类型将根据源文件名自动选定。 在这种情况下，压缩类型为 JSON
        
   * 如果选择了“JSON”，还必须选择从 1 到 10 的“嵌套级别”。  级别确定表列数据分割。

        :::image type="content" source="media/one-click-ingestion-existing-table/json-levels.png" alt-text="选择嵌套级别":::
    
       > [!TIP]
       > 如果要使用 CSV 文件，请参阅[使用一键式引入将 CSV 数据从容器引入到 Azure 数据资源管理器中的新表](one-click-ingestion-new-table.md#edit-the-schema)

### <a name="add-nested-json-data"></a>添加嵌套的 JSON 数据 

如果要从与上述所选的主要 JSON 级别不同的嵌套级别中添加列，请执行以下步骤：

1. 单击任何列名旁边的箭头，然后选择“新建列”。

    :::image type="content" source="media/one-click-ingestion-existing-table/new-column.png" alt-text="一张屏幕截图，显示在一键式引入过程中用于添加“新建列 - 架构”选项卡的选项 - Azure 数据资源管理器":::

1. 输入新的列名，然后从下拉菜单中选择“列类型” 。
1. 在“源”下，选择“新建” 。

    :::image type="content" source="media/one-click-ingestion-existing-table/create-new-source.png" alt-text="屏幕截图 - 在一键式引入过程中创建新源来添加嵌套的 JSON 数据 - Azure 数据资源管理器":::

1. 为此列输入新源，然后单击“确定”。 此源可来自任何 JSON 级别。

    :::image type="content" source="media/one-click-ingestion-existing-table/name-new-source.png" alt-text="屏幕截图 - 用于为添加的列命名新的数据源的弹出窗口 - Azure 数据资源管理器一键式引入":::

1. 选择“创建”。 新列将添加到表的末尾。

    :::image type="content" source="media/one-click-ingestion-existing-table/create-new-column.png" alt-text="屏幕截图 - 在Azure 数据资源管理器中，在一键式引入期间创建新列":::

### <a name="edit-the-table"></a>编辑表 

[!INCLUDE [data-explorer-one-click-column-table](includes/data-explorer-one-click-column-table.md)]

> [!NOTE]
> * 对于表格格式，无法映射列两次。 若要映射到现有列，请先删除新列。
> * 不能更改已有列类型。 如果尝试映射到其他格式的列，结果可能出现空列。

[!INCLUDE [data-explorer-one-click-command-editor](includes/data-explorer-one-click-command-editor.md)]

## <a name="start-ingestion"></a>开始引入

选择“开始引入”，在创建表和映射后开始引入数据。

:::image type="content" source="media/one-click-ingestion-existing-table/start-ingestion.png" alt-text="开始引入":::

## <a name="complete-data-ingestion"></a>完成数据引入

如果数据引入成功完成，则“数据引入已完成”窗口中的所有三个步骤都会带有绿色的对勾标记。

:::image type="content" source="media/one-click-ingestion-existing-table/one-click-data-ingestion-complete.png" alt-text="一键式数据引入已完成":::

> [!IMPORTANT]
> 若要设置从容器持续引入，请参阅[使用一键式引入将 CSV 数据从容器引入到 Azure 数据资源管理器中的新表](one-click-ingestion-new-table.md#create-continuous-ingestion-for-container)

[!INCLUDE [data-explorer-one-click-ingestion-query-data](includes/data-explorer-one-click-ingestion-query-data.md)]

## <a name="next-steps"></a>后续步骤

* [在 Azure 数据资源管理器 Web UI 中查询数据](web-query-data.md)
* [使用 Kusto 查询语言编写 Azure 数据资源管理器的查询](write-queries.md)
