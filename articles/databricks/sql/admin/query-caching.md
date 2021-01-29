---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 查询缓存 - Azure Databricks
description: 了解如何缓存 Azure Databricks SQL Analytics 查询结果。
ms.openlocfilehash: 052128e856b34a75a75284b409505e3cf71f7cd1
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692283"
---
# <a name="query-caching"></a>查询缓存

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

Azure Databricks SQL Analytics 支持以下类型的查询缓存：

* SQL Analytics UI 缓存：SQL Analytics [UI](../user/queries/index.md) 中按每位用户缓存所有查询和仪表板结果。

  > [!NOTE]
  >
  > 在公共预览版期间，查询和查询结果的默认行为是两种查询结果永久缓存并置于 Azure Databricks [控制平面](../../getting-started/overview.md#architecture)中。 可通过重新运行不再需要存储的查询来删除查询结果。 重新运行后，将从缓存中删除旧的查询结果。

* 查询结果缓存：按每个群集缓存通过 SQL 终结点进行的所有查询的查询结果。
* [增量缓存](../../delta/optimizations/delta-cache.md)：本地 SSD 缓存，针对通过 SQL 终结点进行的查询从数据存储中读取数据。

  查询结果缓存和增量缓存会影响 SQL Analytics [UI](../user/queries/index.md) 以及 [BI 和其他外部客户端](../../integrations/bi/index.md)中的查询。