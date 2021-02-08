---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: INSERT OVERWRITE DIRECTORY - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 INSERT OVERWRITE DIRECTORY 语法。
ms.openlocfilehash: 81965dbcfb0a8c0f60d1f7ae553ab8b6053aa499
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060872"
---
# <a name="insert-overwrite-directory"></a>INSERT OVERWRITE DIRECTORY

按给定的 Spark 文件格式，使用新值覆盖目录中的现有数据。 通过值表达式或查询的结果指定插入的行。

## <a name="syntax"></a>语法

```
INSERT OVERWRITE [ LOCAL ] DIRECTORY [ directory_path ]
    USING file_format [ OPTIONS ( key = val [ , ... ] ) ]
    { VALUES ( { value | NULL } [ , ... ] ) [ , ( ... ) ] | query }
```

## <a name="parameters"></a>参数

* **directory_path**

  目标目录。 也可以在 ``OPTIONS`` 中使用 ``path`` 指定此项。
  ``LOCAL`` 关键字用于指定此目录位于本地文件系统上。

* **file_format**

  要用于插入的文件格式。 有效选项为 ``TEXT``、``CSV``、``JSON``、``JDBC``、``PARQUET``、``ORC``、``HIVE``、``LIBSVM``，或 ``org.apache.spark.sql.execution.datasources.FileFormat`` 的自定义实现的完全限定类名。

* **OPTIONS ( key = val [ , … ] )**

  指定用于写入文件格式的一个或多个选项。

* **VALUES ( { value | NULL } [ , … ] ) [ , ( … ) ]**

  要插入的值。 可以插入显式指定的值或 NULL。
  必须使用逗号分隔子句中的每个值。 可以指定多个值集以插入多行。

* **查询**

  生成要插入的行的查询。 可采用以下格式之一：

  * ``SELECT`` 语句
  * ``TABLE`` 语句
  * ``FROM`` 语句

## <a name="examples"></a>示例

```sql
INSERT OVERWRITE DIRECTORY '/tmp/destination'
    USING parquet
    OPTIONS (col1 1, col2 2, col3 'test')
    SELECT * FROM test_table;

INSERT OVERWRITE DIRECTORY
    USING parquet
    OPTIONS ('path' '/tmp/destination', col1 1, col2 2, col3 'test')
    SELECT * FROM test_table;
```

## <a name="related-statements"></a>相关语句

* [INSERT INTO](sql-ref-syntax-dml-insert-into.md)
* [INSERT OVERWRITE](sql-ref-syntax-dml-insert-overwrite-table.md)
* [INSERT OVERWRITE DIRECTORY with Hive format](sql-ref-syntax-dml-insert-overwrite-directory-hive.md)