---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: DESCRIBE HISTORY（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 DESCRIBE HISTORY 语法。
ms.openlocfilehash: ab5e56d0c646f887a1e8c6f26c3a199a75207eff
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692415"
---
# <a name="describe-history-delta-lake-on-azure-databricks"></a><a id="describe-history"> </a><a id="describe-history-delta-lake-on-azure-databricks"> </a>DESCRIBE HISTORY（Azure Databricks 上的 Delta Lake）

返回对表的每次写入的出处信息，包括操作、用户等。  表历史记录会保留 30 天。

## <a name="syntax"></a>语法

```
DESCRIBE HISTORY table_identifier
```

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。

有关详细信息，请参阅[检索 Delta 表历史记录](../../delta/delta-utility.md#delta-history)。