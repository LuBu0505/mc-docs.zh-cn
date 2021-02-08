---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 截断表 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x 和 Delta Lake SQL 语言的 TRUNCATE TABLE 语法。
ms.openlocfilehash: 6c0590c98d30a83321e98621be1e3d63a284ee2d
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060726"
---
# <a name="truncate-table"></a>截断表

```sql
TRUNCATE TABLE table_name [PARTITION part_spec]

part_spec:
  : (part_col1=value1, part_col2=value2, ...)
```

删除表中的所有行或表中匹配的分区。 该表不得为外部表或视图。

**``PARTITION``**

用于匹配要截断的分区的部分分区规范。 在 Spark 2.0 中，只有使用 Hive 格式创建的表才支持此功能。 从 Spark 2.1 开始，还支持数据源表。 Delta 表不支持。