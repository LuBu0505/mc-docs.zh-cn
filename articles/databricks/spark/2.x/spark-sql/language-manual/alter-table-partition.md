---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: 更改表分区 - Azure Databricks
description: 了解如何使用 Azure Databricks 中的 Apache Spark SQL 语言的 ALTER TABLE ... Azure Databricks 中 Apache Spark 2.x SQL 语言的 PARTITION 语法。
ms.openlocfilehash: 017fd709dd18531907c9991ab869ca8917e3f16a
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061193"
---
# <a name="alter-table-partition"></a>更改表分区

## <a name="add-partition"></a>添加分区

```sql
ALTER TABLE table_name ADD [IF NOT EXISTS]
    (PARTITION part_spec [LOCATION path], ...)

part_spec:
    : (part_col_name1=val1, part_col_name2=val2, ...)
```

向表中添加分区，可以选择为每个添加的分区使用自定义位置。 只有那些使用 Hive 格式创建的表支持此功能。 但从 Spark 2.1 开始，使用数据源 API 定义的表也支持 ``Alter Table Partitions``。

**``IF NOT EXISTS``**

如果指定的分区已存在，则不会执行任何操作。

## <a name="change-partition"></a>更改分区

```sql
ALTER TABLE table_name PARTITION part_spec RENAME TO PARTITION part_spec

part_spec:
    : (part_col_name1=val1, part_col_name2=val2, ...)
```

更改某个分区的分区字段值。 仅允许对使用 Hive 格式创建的表执行此操作。

## <a name="drop-partition"></a>删除分区

```sql
ALTER TABLE table_name DROP [IF EXISTS] (PARTITION part_spec, ...)
part_spec:
    : (part_col_name1=val1, part_col_name2=val2, ...)
```

从表或视图中删除分区。 仅允许对使用 Hive 格式创建的表执行此操作。

**``IF EXISTS``**

如果指定的分区不存在，则不会执行任何操作。

## <a name="set-partition-location"></a>设置分区位置

```sql
ALTER TABLE table_name PARTITION part_spec SET LOCATION path

part_spec:
    : (part_col_name1=val1, part_col_name2=val2, ...)
```

设置指定分区的位置。 仅允许为使用 Hive 格式创建的表设置各个分区的位置。