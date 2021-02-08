---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: MSCK REPAIR TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 MSCK REPAIR TABLE 语法。
ms.openlocfilehash: d47619dcf565af7f66a6734f08ef634f2a296d7a
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061061"
---
# <a name="msck-repair-table"></a>MSCK REPAIR TABLE

恢复表目录中的所有分区并更新 Hive 元存储。 使用 ``PARTITIONED BY`` 子句创建表时，将在 Hive 元存储中生成和注册分区。 但是，如果已分区表是根据现有数据创建的，则不会在 Hive 元存储中自动注册分区；必须运行 ``MSCK REPAIR TABLE`` 来注册分区。 对于不存在的表或不具有分区的表，``MSCK REPAIR TABLE`` 将引发异常。 恢复分区的另一种方法是使用 ``ALTER TABLE RECOVER PARTITIONS``。

## <a name="syntax"></a>语法

```sql
MSCK REPAIR TABLE table_identifier
```

## <a name="parameters"></a>参数

* **table_identifier**

  要修复的表的名称。 可以选择使用数据库名称来限定表名称。

  **语法：** ``[database_name.] table_name``

## <a name="examples"></a>示例

```sql
-- create a partitioned table from existing data /tmp/namesAndAges.parquet
CREATE TABLE t1 (name STRING, age INT) USING parquet PARTITIONED BY (age)
    LOCATION "/tmp/namesAndAges.parquet";

-- SELECT * FROM t1 does not return results
SELECT * FROM t1;

-- run MSCK REPAIR TABLE to recovers all the partitions
MSCK REPAIR TABLE t1;

-- SELECT * FROM t1 returns results
SELECT * FROM t1;
+-------+---+
|   name|age|
+-------+---+
|Michael| 20|
+-------+---+
| Justin| 19|
+-------+---+
|   Andy| 30|
+-------+---+
```

## <a name="related-statements"></a>相关语句

* [ALTER TABLE](sql-ref-syntax-ddl-alter-table.md)