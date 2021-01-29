---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/07/2021
title: SQL 终结点 - Azure Databricks
description: 了解 Azure Databricks SQL 终结点及其管理方式。
ms.openlocfilehash: 1b6164f8954c448ea0d8c8a4b1b63f3dbf6d73ce
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692281"
---
# <a name="sql-endpoints"></a>SQL 终结点

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

Azure Databricks SQL 终结点是一种计算资源，支持对 Azure Databricks 环境中的数据对象运行 SQL 命令。

SQL 终结点显示在[查询历史记录](query-history.md)中，记录了运行查询的用户。

SQL 终结点支持 [SQL Analytics 的 SQL 参考](../language-manual/index.md)中的 SQL 命令。

另一种数据源类型是[外部数据源](external-data-sources/index.md)。

此部分介绍如何通过 UI 来使用 SQL 终结点。 若要使用 API 处理 SQL 终结点，请参阅 [SQL 终结点 API](../api/sql-endpoints.md)。

通过 SQL 终结点进行的查询由[查询监视器](../../spark/latest/spark-sql/query-watchdog.md)管理，采用本文所述的默认值。

## <a name="requirements"></a>要求

* 若要创建 SQL 终结点，必须在 Azure Databricks 工作区中具有[群集创建权限](../../administration-guide/access-control/cluster-acl.md#cluster-create-permission)。
* 若要管理 SQL 终结点，必须在 Azure Databricks SQL Analytics 中具有该终结点的[可管理](../user/security/access-control/sql-endpoint-acl.md)权限。

## <a name="view-sql-endpoints"></a>查看 SQL 终结点

单击 ![边栏中的](../../_static/images/icons/endpoints-icon.png) “模型”图标。

默认情况下，终结点按字母顺序进行排序。 可以通过单击列标题重新对列表进行排序。

若要筛选终结点列表，请在搜索框中输入文本：

> [!div class="mx-imgBorder"]
> ![筛选终结点](../../_static/images/sql/filter-endpoints.png)

## <a name="create-a-sql-endpoint"></a>创建 SQL 终结点

1. 单击 ![边栏中的](../../_static/images/icons/endpoints-icon.png) “模型”图标。
2. 单击“+新建 SQL 终结点”。

   > [!div class="mx-imgBorder"]
   > ![创建终结点](../../_static/images/sql/create-endpoint.png)

3. 为终结点输入名称。 接受或编辑[终结点属性](#edit-a-sql-endpoint)。
4. 单击 **“创建”** 。

## <a name="start-stop-or-delete-a-sql-endpoint"></a><a id="start"> </a><a id="start-stop-or-delete-a-sql-endpoint"> </a>启动、停止或删除 SQL 终结点

1. 单击 ![边栏中的](../../_static/images/icons/endpoints-icon.png) “模型”图标。
2. 在“操作”列表中，单击垂直省略号 ![“垂直省略号”](../../_static/images/sql/vertical-ellipsis.png)，然后选择“启动”、“停止”或“删除”  。

## <a name="edit-a-sql-endpoint"></a><a id="edit-a-sql-endpoint"> </a><a id="edit-endpoint"> </a>编辑 SQL 终结点

1. 单击 ![边栏中的](../../_static/images/icons/endpoints-icon.png) “模型”图标。
2. 单击一个终结点。
3. 单击 **“编辑”** 。
4. 编辑终结点属性。
   * **群集大小**：群集辅助角色数和协调器大小。 默认值为“X-大”。 若要减少查询延迟，请增加大小。 大小越大，协调器越大，群集辅助角色数也将翻倍。  有关详细信息，请查看[群集大小](#cluster-size)。
   * **自动停止**：决定在终结点空闲指定分钟数的情况下是否停止终结点。 默认为“开”，值为 120 分钟。

     > [!NOTE]
     >
     > 空闲的 SQL 终结点会继续累积 DBU 和云实例费用，直到被停止。

   * **多群集负载均衡**：分布发送到终结点的查询所依据的群集最小数量和最大数量。 默认为“关”；如果设为“开”，则默认是 1 个群集 。 若要针对给定查询处理更多并发用户，请启用负载均衡并增加群集数。
   * **Photon**：决定是否在可加快查询执行的本机向量化引擎上执行查询。 默认为“关”。 若要启用，请选择“开”，阅读免责声明，然后单击“启用 Photon” 。

     > [!IMPORTANT]
     >
     > 多群集负载均衡和 Photon 以预览版提供。 请联系 Azure Databricks 代表，以申请访问权限。

5. 单击“保存”或“保存并重启” 。

## <a name="add-an-endpoint-tag"></a>添加终结点标记

可使用标记轻松监视组织中各种组所使用的云资源的成本。 可在创建终结点时将标记指定为键值对，Azure Databricks 会将这些标记应用于云资源。

若要添加终结点标记：

1. 单击 ![边栏中的](../../_static/images/icons/endpoints-icon.png) “模型”图标。
2. 单击一个终结点。
3. 单击 **“编辑”** 。
4. 选择“更多选项”。
5. 单击“标记”选项卡。
6. 输入标记键和值。

   > [!div class="mx-imgBorder"]
   > ![添加标记](../../_static/images/sql/add-endpoint-tag.png)

7. 单击“保存并重启”。

## <a name="monitor-a-sql-endpoint"></a>监视 SQL 终结点

可检查终结点处理的查询数和分配给终结点的群集数。

1. 单击 ![边栏中的](../../_static/images/icons/endpoints-icon.png) “模型”图标。
2. 单击一个终结点。
3. 单击 **“监视”** 。

   这会显示一个图表，其中有过去 6 小时内终结点处理的查询数量以及分配给终结点的群集数量。

   单击图表右上方的时间刻度按钮可更改显示的时间段。 例如，以下屏幕截图显示了 7 天的这类统计信息：

   > [!div class="mx-imgBorder"]
   > ![监视终结点](../../_static/images/sql/monitor-endpoint.png)

   > [!NOTE]
   >
   > 仅当启用并配置了[多群集负载均衡](#edit-endpoint)时，群集计数才能大于 1。

## <a name="cluster-size"></a>群集大小

本部分中的表将 SQL 终结点群集大小映射到 Azure Databricks 群集驱动程序大小和辅助角色计数。

| 群集大小                      | 驱动程序大小                       | 辅助角色计数                      |
|-----------------------------------|-----------------------------------|-----------------------------------|
| 2X-小                          | Standard_E8_v3                    | 1                                 |
| X-小                           | Standard_E8_v3                    | 2                                 |
| 小                             | Standard_E16_v3                   | 4                                 |
| 中                            | Standard_E32_v3                   | 8                                 |
| 大                             | Standard_E32_v3                   | 16                                |
| X-大                           | Standard_E64_v3                   | 32                                |
| 2X-大                          | Standard_E64_v3                   | 64                                |
| 3X-大                          | Standard_E64_v3                   | 128                               |
| 4X-大                          | Standard_E64_v3                   | 256                               |

所有辅助角色的实例大小都是 Standard_E8_v3。