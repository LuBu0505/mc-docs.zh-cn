---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: FSCK REPAIR TABLE（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 FSCK REPAIR TABLE 语法。
ms.openlocfilehash: a6efdf3cf26afca6a20e5825bb07192f80dc28cb
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692412"
---
# <a name="fsck-repair-table-delta-lake-on-azure-databricks"></a>FSCK REPAIR TABLE（Azure Databricks 上的 Delta Lake）

从 Delta 表的事务日志中删除无法再从基础文件系统中找到的文件条目。 手动删除这些文件时，可能会发生这种情况。

## <a name="syntax"></a>语法

```
FSCK REPAIR TABLE table_identifier [DRY RUN]
```

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **DRY RUN**

  返回要从事务日志中删除的文件的列表。