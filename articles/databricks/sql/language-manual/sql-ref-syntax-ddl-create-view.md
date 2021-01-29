---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: CREATE VIEW - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 CREATE VIEW 语法。
ms.openlocfilehash: 594fa31b2225095b777a5d1cd2190c34b7155ea2
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692229"
---
# <a name="create-view"></a>CREATE VIEW

基于 SQL 查询的结果集构造没有物理数据的虚拟表。 ``ALTER VIEW`` 和 ``DROP VIEW`` 仅更改元数据。

## <a name="syntax"></a>语法

```sql
CREATE [ OR REPLACE ] [ [ GLOBAL ] TEMPORARY ] VIEW [ IF NOT EXISTS ] view_identifier
    create_view_clauses AS query
```

## <a name="parameters"></a>参数

* **OR REPLACE**

  如果已存在具有相同名称的视图，则会替换该视图。

* **[ GLOBAL ] TEMPORARY**

  临时视图是会话范围内的，在会话终止时会被删除，因为它会跳过在基础元存储中持久保存定义（如果有）的操作。
  全局临时视图与系统保留的临时数据库 ``global_temp`` 关联。

* **IF NOT EXISTS**

  创建一个视图（如果不存在）。

* **view_identifier**

  视图名称，可选择使用数据库名称进行限定。

  **语法：** ``[database_name.] view_name``

* **create_view_clauses**

  这些子句是可选的，不区分顺序。 可采用以下格式。

  * ``[ ( column_name [ COMMENT column_comment ], ... ) ]``，用于指定列级注释。
  * ``[ COMMENT view_comment ]``，用于指定视图级别的注释。
  * ``[ TBLPROPERTIES ( property_name = property_value [ , ... ] ) ]``，用于添加元数据键值对。
* **查询** 从基表或其他视图构造视图的 [SELECT](sql-ref-syntax-qry-select.md) 语句。

## <a name="examples"></a>示例

```sql
-- Create or replace view for `experienced_employee` with comments.
CREATE OR REPLACE VIEW experienced_employee
    (ID COMMENT 'Unique identification number', Name)
    COMMENT 'View for experienced employees'
    AS SELECT id, name FROM all_employee
        WHERE working_years > 5;

-- Create a global temporary view `subscribed_movies` if it does not exist.
CREATE GLOBAL TEMPORARY VIEW IF NOT EXISTS subscribed_movies
    AS SELECT mo.member_id, mb.full_name, mo.movie_title
        FROM movies AS mo INNER JOIN members AS mb
        ON mo.member_id = mb.id;
```

## <a name="related-statements"></a>相关语句

* [ALTER VIEW](sql-ref-syntax-ddl-alter-view.md)
* [DROP VIEW](sql-ref-syntax-ddl-drop-view.md)
* [SHOW VIEWS](sql-ref-syntax-aux-show-views.md)