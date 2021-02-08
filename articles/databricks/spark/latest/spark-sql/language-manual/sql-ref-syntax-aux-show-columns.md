---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: SHOW COLUMNS - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 SHOW COLUMNS 语法。
ms.openlocfilehash: 310e052c57658cef8d868aef884861e2eaef628d
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061157"
---
# <a name="show-columns"></a>SHOW COLUMNS

返回表中的列的列表。 如果该表不存在，则会引发异常。

## <a name="syntax"></a>语法

```
SHOW COLUMNS { IN | FROM } table_identifier [ database ]
```

> [!NOTE]
>
> 关键字 ``IN`` 和 ``FROM`` 可互换。

## <a name="parameters"></a>参数

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **database**

  可选的数据库名称。 指定表后，将从该数据库中解析该表。 指定此参数后，不得使用其他数据库名称来限定表名称。

  **语法：** ``{ IN | FROM } database_name``

  > [!NOTE]
  >
  > 关键字 ``IN`` 和 ``FROM`` 可互换。

## <a name="examples"></a>示例

```sql
-- Create `customer` table in `salesdb` database;
USE salesdb;
CREATE TABLE customer(
    cust_cd INT,
    name VARCHAR(100),
    cust_addr STRING);

-- List the columns of `customer` table in current database.
SHOW COLUMNS IN customer;
+---------+
| col_name|
+---------+
|  cust_cd|
|     name|
|cust_addr|
+---------+

-- List the columns of `customer` table in `salesdb` database.
SHOW COLUMNS IN salesdb.customer;
+---------+
| col_name|
+---------+
|  cust_cd|
|     name|
|cust_addr|
+---------+

-- List the columns of `customer` table in `salesdb` database
SHOW COLUMNS IN customer IN salesdb;
+---------+
| col_name|
+---------+
|  cust_cd|
|     name|
|cust_addr|
+---------+
```

## <a name="related-statements"></a>相关语句

* [DESCRIBE TABLE](sql-ref-syntax-aux-describe-table.md)
* [SHOW TABLE EXTENDED](sql-ref-syntax-aux-show-table.md)