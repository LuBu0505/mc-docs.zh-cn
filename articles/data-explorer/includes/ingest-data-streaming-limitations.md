---
title: include 文件
description: include 文件
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 07/13/2020
ms.date: 01/22/2021
ms.author: v-tawe
ms.reviewer: alexefro
ms.custom: include file
ms.openlocfilehash: 8e28f8093d523f41c4b6c2f3e258a42bb3736d43
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98612936"
---
## <a name="limitations"></a>限制

* 如果数据库本身或其任何表已定义并启用了[流式引入策略](../kusto/management/streamingingestionpolicy.md)，则数据库不支持[数据库游标](../kusto/management/databasecursor.md)。
* [数据映射](../kusto/management/mappings.md)必须[已预先创建](../kusto/management/create-ingestion-mapping-command.md)，以便在流式引入中使用。 单个流式处理引入请求不包含内联数据映射。
* 增大 VM 和群集大小可以提高流式引入的性能和容量。 并发引入请求数量限制为每个核心六个。 例如，对于 16 核 SKU（如 D14 和 L16），支持的最大负载为 96 个并发引入请求。 对于双核 SKU（例如 D11），支持的最大负载为 12 个并发引入请求。
* 每个流式引入请求的数据大小限制为 4 MB。
* 对流式引入服务进行架构更新（例如创建和修改表与引入映射）最长可能需要花费 5 分钟时间。 有关详细信息，请参阅[流式引入和架构更改](../kusto/management/data-ingestion/streaming-ingestion-schema-changes.md)。
* 即使数据不是流式引入的，在群集上启用流式引入也会占用群集计算机的一部分本地 SSD 磁盘用于存储流式引入数据，因此会减少热缓存的可用存储。
* [盘区标记](../kusto/management/extents-overview.md#extent-tagging)不能在流式引入数据上设置。
* [更新策略](../kusto/management/updatepolicy.md)。 更新策略只能引用源表中新引入的数据，而不能引用数据库中的任何其他数据或表。

<!-- * If streaming ingestion is used on any of the tables of the database, this database cannot be used as leader for [follower databases](../follower.md) or as a [data provider](../data-share.md#data-provider---share-data) for Azure Data Share. -->
