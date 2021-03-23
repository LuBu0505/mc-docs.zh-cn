---
title: 使用一键式引入将 CSV 数据从容器引入到 Azure 数据资源管理器中的新表
description: 使用一键式引入轻松地将数据引入（载入）新的 Azure 数据资源管理器表中。
author: orspod
ms.author: v-junlch
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: how-to
ms.date: 03/18/2021
ms.openlocfilehash: e33a32a559997752679b3add615a5cc5e490a44f
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767833"
---
# <a name="use-one-click-ingestion-to-ingest-csv-data-from-a-container-to-a-new-table-in-azure-data-explorer"></a>使用一键式引入将 CSV 数据从容器引入到 Azure 数据资源管理器中的新表

> [!div class="op_single_selector"]
> * [将 CSV 数据从容器引入新表](one-click-ingestion-new-table.md)
> * [将 JSON 数据从本地文件引入现有表](one-click-ingestion-existing-table.md)

借助[一键式引入](ingest-data-one-click.md)，可将 JSON、CSV 和其他格式的数据快速引入表中并轻松创建映射结构。 数据可以从存储、本地文件或容器引入，可以是一次性引入过程，也可以是持续引入过程。  

本文档介绍如何在特定用例中使用直观的一键式向导将容器中的 CSV 数据引入新表  。 引入后，可[设置事件网格引入管道](#create-continuous-ingestion-for-container)，它会侦听源容器中的新文件，并将符合条件的数据引入到新表中。 可以使用稍微改动的同一过程来覆盖各种不同的用例。

有关一键式引入的概述和先决条件列表，请参阅[一键式引入](ingest-data-one-click.md)。
若要了解如何将数据引入 Azure 数据资源管理器中的现有表，请参阅[一键式引入到现有表中](one-click-ingestion-existing-table.md)

## <a name="ingest-new-data"></a>引入新数据

1. 在 Web UI 的左侧菜单中，右键单击“数据库”并选择“引入新数据”。

    :::image type="content" source="media/one-click-ingestion-new-table/one-click-ingestion-in-web-ui.png" alt-text="引入新数据":::

1. 在“引入新数据”窗口中，“源”选项卡处于选中状态 。 系统会自动填充“群集”和“数据库”字段。

    [!INCLUDE [one-click-cluster](includes/one-click-cluster.md)]

1. 选择“表” > “新建”并输入新表的名称。 可以使用字母数字字符、连字符和下划线。 不支持特殊字符。

    > [!NOTE]
    > 表名称必须介于 1 到 1024 个字符之间。

    :::image type="content" source="media/one-click-ingestion-new-table/create-new-table.png" alt-text="创建新表一键式引入":::

## <a name="select-an-ingestion-type"></a>选择引入类型

在“源类型”下，执行以下步骤：
   
  1. 选择“从 blob 容器”（Blob 容器、ADLS Gen1 容器、ADLS Gen2 容器）。 最多可以从一个容器中引入 1000 个 blob。
  1. 在“链接到存储”字段中，添加容器的 [SAS URL](/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container)，并选择性地输入样本大小。 若要从此容器中的文件夹进行引入，请参阅[从容器中的文件夹引入](#ingest-from-folder-in-a-container)。

      :::image type="content" source="media/one-click-ingestion-new-table/from-container.png" alt-text="从容器进行一键式引入":::

     > [!TIP] 
     > 如需了解如何“从文件”引入，请参阅[使用一键式引入将 JSON 数据从本地文件引入到 Azure 数据资源管理器中的现有表](one-click-ingestion-existing-table.md#select-an-ingestion-type)

### <a name="ingest-from-folder-in-a-container"></a>从容器中的文件夹引入

若要从某个容器内的特定文件夹引入，[请生成下列格式的字符串](kusto/api/connection-strings/storage.md#azure-data-lake-store)：

container_path`/`folder_path`;`access_key_1  

你将在[选择引入类型](#select-an-ingestion-type)中使用此字符串，而不是 SAS URL。

1. 导航到存储帐户，然后选择“存储资源管理器”>“选择 Blob 容器”

    :::image type="content" source="media/one-click-ingestion-new-table/blob-containers.png" alt-text="显示在 Azure 存储帐户中访问 Blob 容器的屏幕截图":::

1. 浏览到所选文件夹，然后选择“复制 URL”。 将该值粘贴到临时文件中，再将 `;` 添加到此字符串的末尾。

    :::image type="content" source="media/one-click-ingestion-new-table/copy-url.png" alt-text="Blob 容器中文件夹内复制 URL 的屏幕截图 - Azure 存储帐户":::

1. 在左侧菜单的“设置”下，选择“访问密钥” 。

    :::image type="content" source="media/one-click-ingestion-new-table/copy-key-1.png" alt-text="访问密钥存储帐户副本密钥字符串的屏幕截图":::

1. 在“密钥 1”下，复制密钥字符串 。 将该值粘贴到步骤 2 中字符串的末尾。 

### <a name="storage-subscription-error"></a>存储订阅错误

如果在从存储帐户引入时收到以下错误消息：

> 在所选订阅下找不到存储。 请在门户中将存储帐户 `storage_account_name` 订阅添加到你选择的订阅中。

1. 在右上方菜单任务栏中选择 :::image type="icon" source="media/ingest-data-one-click/directory-subscription-icon.png" border="false"::: 图标。 这会打开“目录 + 订阅”窗格。

1. 在“所有订阅”下拉列表中，将你的存储帐户的订阅添加到所选列表中。 

    :::image type="content" source="media/ingest-data-one-click/subscription-dropdown.png" alt-text="“目录 + 订阅”窗格的屏幕截图，其中用红色框突出显示了订阅下拉列表。":::

## <a name="sample-data"></a>示例数据

此时会显示数据样本。 如果需要，请筛选数据，仅引入以特定字符开头或结尾的文件。 调整筛选器时，预览会自动更新。

例如，筛选以 .csv 扩展名开头的所有文件。

:::image type="content" source="media/one-click-ingestion-new-table/from-container-with-filter.png" alt-text="一键式引入筛选器":::

系统将随机选择一个文件，并且将根据架构定义文件生成架构。 你可选择不同的文件。

## <a name="edit-the-schema"></a>编辑架构

选择“编辑架构”以查看并编辑表列配置。  服务会通过检查源的名称自动确定该源是否已压缩。

在“架构”选项卡中：

   1. 选择“数据格式”：

        在这种情况下，数据格式为 CSV

        > [!TIP]
        > 如果要使用 JSON 文件，请参阅[使用一键式引入将 JSON 数据从本地文件引入到 Azure 数据资源管理器中的现有表](one-click-ingestion-existing-table.md#edit-the-schema)。

   1. 可以选中“包括列名”复选框，以忽略文件的标题行。

        :::image type="content" source="media/one-click-ingestion-new-table/non-json-format.png" alt-text="选择“包含列名称”":::

在“映射名称”字段中输入映射名称。 可以使用字母数字字符和下划线。 不支持空格、特殊字符和连字符。

:::image type="content" source="media/one-click-ingestion-new-table/table-mapping.png" alt-text="表映射名称一键式引入":::

### <a name="edit-the-table"></a>编辑表

引入到新表后，在创建表时可以更改表的各个方面。

[!INCLUDE [data-explorer-one-click-column-table](includes/data-explorer-one-click-column-table.md)]

> [!NOTE]
> 对于表格格式，无法映射列两次。 若要映射到现有列，请先删除新列。

[!INCLUDE [data-explorer-one-click-command-editor](includes/data-explorer-one-click-command-editor.md)]

## <a name="start-ingestion"></a>开始引入

选择“开始引入”，在创建表和映射后开始引入数据。

:::image type="content" source="media/one-click-ingestion-new-table/start-ingestion.png" alt-text="开始引入一键式引入":::

## <a name="complete-data-ingestion"></a>完成数据引入

如果数据引入成功完成，则“数据引入已完成”窗口中的所有三个步骤都会带有绿色的对勾标记。

:::image type="content" source="media/one-click-ingestion-new-table/one-click-data-ingestion-complete.png" alt-text="一键式数据引入已完成"::: 

[!INCLUDE [data-explorer-one-click-ingestion-query-data](includes/data-explorer-one-click-ingestion-query-data.md)]

## <a name="create-continuous-ingestion-for-container"></a>为容器创建持续引入

通过持续引入，可创建一个事件网格，它会在源容器中侦听新文件。 任何满足预定义参数（前缀、后缀等）条件的新文件都会自动引入到目标表中。 

1. 在“连续引入”磁贴中选择“事件网格”，以打开 Azure 门户 。 此时会打开数据连接页，其中打开了事件网格数据连接器，并已输入了源和目标参数（源容器、表和映射）。
    
    :::image type="content" source="media/one-click-ingestion-new-table/continuous-button.png" alt-text="“连续引入”按钮":::

1. 选择“创建”，以创建用于侦听该容器中发生的任何更改、更新或新数据的数据连接。 

    :::image type="content" source="media/one-click-ingestion-new-table/event-hub-create.png" alt-text="创建事件中心连接":::

## <a name="next-steps"></a>后续步骤

* [在 Azure 数据资源管理器 Web UI 中查询数据](web-query-data.md)
* [使用 Kusto 查询语言编写 Azure 数据资源管理器的查询](write-queries.md)
