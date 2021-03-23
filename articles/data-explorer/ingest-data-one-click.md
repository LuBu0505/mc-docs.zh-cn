---
title: 使用一键式引入将数据引入到 Azure 数据资源管理器中
description: 概述如何使用一键式引入轻松地将数据引入（载入）Azure 数据资源管理器。
author: orspod
ms.author: v-junlch
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: how-to
ms.date: 03/18/2021
ms.openlocfilehash: abe504096fcca5011e7d1f97d9ca39b42a2c8226
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767289"
---
# <a name="what-is-one-click-ingestion"></a>什么是一键式引入？

一键式引入使数据引入过程简单、快速和直观。 一键式引入可帮助你快速开始引入数据、创建数据库表、映射结构。 从不同类型的源中选择不同数据类型的数据，可以是一次性引入过程，也可以是连续引入过程。

一键式引入提供以下实用功能：

* 引入向导引导的直观体验
* 几分钟内即可引入数据
* 从不同类型的源引入数据：本地文件、blob 和容器（最多 10,000 个 blob）
* 引入各种[格式](#file-formats)的数据
* 将数据引入新表或现有表
* 建议使用表映射和架构，它们易于更改
* 使用[事件网格](one-click-ingestion-new-table.md#create-continuous-ingestion-for-container)继续轻松快速地从容器引入

首次引入数据时，或者在你不熟悉自己数据的架构时，一键式引入特别有用。

## <a name="prerequisites"></a>先决条件

* 如果还没有 Azure 订阅，可以在开始前创建一个 [Azure 帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn/)。
* 创建 [Azure 数据资源管理器群集和数据库](create-cluster-database-portal.md)。
* 登录到 [Azure 数据资源管理器 Web UI](https://dataexplorer.azure.cn/) 并[添加与群集的连接](web-query-data.md#add-clusters)。

## <a name="access-the-one-click-wizard"></a>访问一键式向导

一键式引入向导可以引导你完成一键式引入过程。

* 若要从 [Azure 数据资源管理器 Web UI](https://dataexplorer.azure.cn/) 访问向导，请使用以下方法之一：
     * 请右键单击 Azure 数据资源管理器 Web UI 左侧菜单中的“数据库”或“表”行，然后选择“引入新数据”  。
        
        :::image type="content" source="media/ingest-data-one-click/one-click-ingestion-in-webui.png" alt-text="在 Web UI 中选择一键式引入":::

* 如需从你的群集中的“欢迎使用 Azure 数据资源管理器”主屏幕访问一键式引入向导，请完成前两个步骤（[群集创建和数据库创建](#prerequisites)），然后选择“引入新数据”。

    :::image type="content" source="media/ingest-data-one-click/welcome-ingestion.png" alt-text="从“欢迎使用 Azure 数据资源管理器”引入新数据":::


* 若要从 Azure 门户访问该向导，请从左侧菜单中选择“查询”，右键单击“数据库”或“表”，然后选择“引入新数据”   。

    :::image type="content" source="media/ingest-data-one-click/access-from-portal.png" alt-text="从 Azure 门户访问一键式引入向导":::

## <a name="one-click-ingestion-wizard"></a>一键式引入向导

> [!NOTE]
> 本部分介绍该向导的常规知识。 你选择的选项取决于要引入的数据格式、要通过其进行引入的数据源类型，以及是引入到新表还是现有表中。
>
> 有关示例场景，请参阅：
> * [从容器以 CSV 格式引入到新表](one-click-ingestion-new-table.md)
> * [从本地文件以 JSON 格式引入到现有表](one-click-ingestion-existing-table.md) 

该向导会引导你配置以下选项：
   * 引入到[现有表](one-click-ingestion-existing-table.md)
   * 引入到[新表](one-click-ingestion-new-table.md)
   * 从以下数据源引入数据：
      * Blob 存储：最多 10 个 blob
      * [本地文件](one-click-ingestion-existing-table.md)：最多 10 个文件
      * [容器](one-click-ingestion-new-table.md)（Blob 容器、ADLS Gen1 容器、ADLS Gen2 容器）

### <a name="schema-mapping"></a>架构映射

服务会自动生成架构和引入属性，你可以对其进行更改。 可以使用现有的映射结构，也可以创建一个新的映射结构，具体取决于是引入到新表还是现有表。

在“架构”选项卡中，执行以下操作：
   * 确认自动生成的压缩类型。
   * 选择[数据的格式](#file-formats)。 不同的格式会允许你进行进一步的更改。
   * 更改[编辑器窗口](#editor-window)中的映射。

#### <a name="file-formats"></a>文件格式

一键式引入支持从 [Azure 数据资源管理器支持引入的所有数据格式](ingestion-supported-formats.md)的源数据中进行引入。

### <a name="editor-window"></a>“编辑器”窗口

在“架构”选项卡的“编辑器”窗口中，可以根据需要调整数据表列 。 

[!INCLUDE [data-explorer-one-click-column-table](includes/data-explorer-one-click-column-table.md)]

>[!NOTE]
> 可以随时打开“编辑器”窗格上方的[命令编辑器](one-click-ingestion-new-table.md#command-editor)。 在命令编辑器中，可以查看和复制基于输入生成的自动命令。

#### <a name="mapping-transformations"></a>映射转换

某些数据格式映射（Parquet、JSON 和 Avro）支持简单的引入时间转换。 若要应用映射转换，请在[编辑器窗口](#editor-window)中创建或更新列。

可对具有 string 或 datetime 类型且“源”的数据类型为 int 或 long 的列执行映射转换 。 支持的映射转换为：
* DateTimeFromUnixSeconds
* DateTimeFromUnixMilliseconds
* DateTimeFromUnixMicroseconds
* DateTimeFromUnixNanoseconds

有关详细信息，请参阅[映射转换](#mapping-transformations)。

### <a name="data-ingestion"></a>数据引入

完成架构映射和列操作后，引入向导将启动数据引入进程。 

* 从非容器源引入数据时，引入会立即生效。

* 如果数据源是容器：
    * Azure 数据资源管理器的[批处理策略](kusto/management/batchingpolicy.md)将聚合数据。 
    * 引入后，可以下载引入报告并查看每个已寻址的 blob 的性能。 
    * 可选择“创建持续引入”并[使用事件网格设置持续引入](one-click-ingestion-new-table.md#create-continuous-ingestion-for-container)。
 
### <a name="initial-data-exploration"></a>初始数据探索
   
引入后，向导会允许你选择[快速命令](one-click-ingestion-existing-table.md#explore-quick-queries-and-tools)进行数据初始探索。


## <a name="next-steps"></a>后续步骤

* [使用一键式引入将 JSON 数据从本地文件引入到 Azure 数据资源管理器中的现有表](one-click-ingestion-existing-table.md)
* [使用一键式引入将 CSV 数据从容器引入到 Azure 数据资源管理器中的新表](one-click-ingestion-new-table.md)
* [在 Azure 数据资源管理器 Web UI 中查询数据](web-query-data.md)
* [使用 Kusto 查询语言编写 Azure 数据资源管理器的查询](write-queries.md)
