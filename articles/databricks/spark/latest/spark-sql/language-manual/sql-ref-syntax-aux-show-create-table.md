---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW CREATE TABLE - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SHOW CREATE TABLE 语法。
ms.openlocfilehash: 20161d628b80f64ef1c15a550d6bdc4072a08f32
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061155"
---
# <a name="show-create-table"></a>SHOW CREATE TABLE

返回用于创建给定表或视图的 [CREATE TABLE 语句](sql-ref-syntax-ddl-create-table.md)或 [CREATE VIEW 语句](sql-ref-syntax-ddl-create-view.md)。 不存在的表或临时视图上的 ``SHOW CREATE TABLE`` 引发异常。

## <a name="syntax"></a>语法

```sql
SHOW CREATE TABLE table_identifier
```

## <a name="parameters"></a>参数

* **table_identifier**

  表或视图名称，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] table_name``

## <a name="examples"></a>示例

```sql
CREATE TABLE test (c INT) ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    STORED AS TEXTFILE
    TBLPROPERTIES ('prop1' = 'value1', 'prop2' = 'value2');

SHOW CREATE TABLE test;
+----------------------------------------------------+
|                                      createtab_stmt|
+----------------------------------------------------+
|CREATE TABLE `default`.`test` (`c` INT)
 USING text
 TBLPROPERTIES (
   'transient_lastDdlTime' = '1586269021',
   'prop1' = 'value1',
   'prop2' = 'value2')
+----------------------------------------------------+
```

## <a name="related-statements"></a>相关语句

* [CREATE TABLE](sql-ref-syntax-ddl-create-table.md)
* [CREATE VIEW](sql-ref-syntax-ddl-create-view.md)