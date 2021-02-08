---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 包含 Hive 格式的 INSERT OVERWRITE DIRECTORY - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的包含 Hive 格式的 INSERT OVERWRITE DIRECTORY 语法。
ms.openlocfilehash: 1e9ad3b3a737e668f01f204ed593871b8edb78fd
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060873"
---
# <a name="insert-overwrite-directory-with-hive-format"></a>INSERT OVERWRITE DIRECTORY with Hive format

通过 Hive ``SerDe``，使用新值覆盖目录中的现有数据。
必须启用 Hive 支持才能使用此命令。 通过值表达式或查询的结果指定插入的行。

## <a name="syntax"></a>语法

```
INSERT OVERWRITE [ LOCAL ] DIRECTORY directory_path
    [ ROW FORMAT row_format ] [ STORED AS file_format ]
    { VALUES ( { value | NULL } [ , ... ] ) [ , ( ... ) ] | query }
```

## <a name="parameters"></a>参数

* **directory_path**

  目标目录。 ``LOCAL`` 关键字指定此目录位于本地文件系统上。

* **row_format**

  此插入的行格式。 有效选项是 ``SERDE`` 子句和 ``DELIMITED`` 子句。 ``SERDE`` 子句可用于为此插入指定自定义 ``SerDe``。 或者，可以使用 ``DELIMITED`` 子句来指定原生 ``SerDe``，并指明分隔符、转义字符和空字符等。

* **file_format**

  此插入的文件格式。 有效选项为 ``TEXTFILE``、``SEQUENCEFILE``、``RCFILE``、``ORC``、``PARQUET`` 和 ``AVRO``。 还可以使用 ``INPUTFORMAT`` 和 ``OUTPUTFORMAT`` 指定你自己的输入和输出格式。 ``ROW FORMAT SERDE`` 只能与 ``TEXTFILE``、``SEQUENCEFILE`` 或 ``RCFILE`` 一起使用，而 ``ROW FORMAT DELIMITED`` 只能与 ``TEXTFILE`` 一起使用。

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
INSERT OVERWRITE LOCAL DIRECTORY '/tmp/destination'
    STORED AS orc
    SELECT * FROM test_table;

INSERT OVERWRITE LOCAL DIRECTORY '/tmp/destination'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    SELECT * FROM test_table;
```

## <a name="related-statements"></a>相关语句

* [INSERT INTO](sql-ref-syntax-dml-insert-into.md)
* [INSERT OVERWRITE](sql-ref-syntax-dml-insert-overwrite-table.md)
* [INSERT OVERWRITE DIRECTORY](sql-ref-syntax-dml-insert-overwrite-directory.md)