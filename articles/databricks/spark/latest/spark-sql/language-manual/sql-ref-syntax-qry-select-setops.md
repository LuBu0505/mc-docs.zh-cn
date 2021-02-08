---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: set 运算符 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 set 运算符 EXCEPT、MINUS、INTERSECT 和 UNION。
ms.openlocfilehash: 8c81dcfa3d720068accf87389680930cce94511e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060894"
---
# <a name="set-operators"></a>集合运算符

将两个输入关系合二为一。 Spark SQL 支持 3 种类型的 set 运算符：

* ``EXCEPT`` 或 ``MINUS``
* ``INTERSECT``
* ``UNION``

输入关系必须具有相同数量的列，并且各个列的数据类型兼容。

## <a name="except"></a>EXCEPT

``EXCEPT`` 和 ``EXCEPT ALL`` 返回在一个关系中存在，但在另一关系中不存在的行。 ``EXCEPT``（或 ``EXCEPT DISTINCT``）仅采用不同的行，而 ``EXCEPT ALL`` 不会从结果行中删除重复项。 请注意，``MINUS`` 是 ``EXCEPT`` 的别名。

### <a name="syntax"></a>语法

```sql
[ ( ] relation [ ) ] EXCEPT | MINUS [ ALL | DISTINCT ] [ ( ] relation [ ) ]
```

### <a name="examples"></a>示例

```sql
-- Use number1 and number2 tables to demonstrate set operators in this page.
SELECT * FROM number1;
+---+
|  c|
+---+
|  3|
|  1|
|  2|
|  2|
|  3|
|  4|
+---+

SELECT * FROM number2;
+---+
|  c|
+---+
|  5|
|  1|
|  2|
|  2|
+---+

SELECT c FROM number1 EXCEPT SELECT c FROM number2;
+---+
|  c|
+---+
|  3|
|  4|
+---+

SELECT c FROM number1 MINUS SELECT c FROM number2;
+---+
|  c|
+---+
|  3|
|  4|
+---+

SELECT c FROM number1 EXCEPT ALL (SELECT c FROM number2);
+---+
|  c|
+---+
|  3|
|  3|
|  4|
+---+

SELECT c FROM number1 MINUS ALL (SELECT c FROM number2);
+---+
|  c|
+---+
|  3|
|  3|
|  4|
+---+
```

## <a name="intersect"></a>INTERSECT

``INTERSECT`` 和 ``INTERSECT ALL`` 返回在两个关系中都存在的行。 ``INTERSECT``（或 ``INTERSECT DISTINCT``）仅采用不同的行，而 ``INTERSECT ALL`` 不会从结果行中删除重复项。

### <a name="syntax"></a>语法

```sql
[ ( ] relation [ ) ] INTERSECT [ ALL | DISTINCT ] [ ( ] relation [ ) ]
```

### <a name="examples"></a>示例

```sql
(SELECT c FROM number1) INTERSECT (SELECT c FROM number2);
+---+
|  c|
+---+
|  1|
|  2|
+---+

(SELECT c FROM number1) INTERSECT DISTINCT (SELECT c FROM number2);
+---+
|  c|
+---+
|  1|
|  2|
+---+

(SELECT c FROM number1) INTERSECT ALL (SELECT c FROM number2);
+---+
|  c|
+---+
|  1|
|  2|
|  2|
+---+
```

## <a name="union"></a>UNION

``UNION`` 和 ``UNION ALL`` 返回在任一关系中存在的行。 ``UNION``（或 ``UNION DISTINCT``）仅采用不同的行，而 ``UNION ALL`` 不会从结果行中删除重复项。

### <a name="syntax"></a>语法

```sql
[ ( ] relation [ ) ] UNION [ ALL | DISTINCT ] [ ( ] relation [ ) ]
```

## <a name="examples"></a>示例

```sql
(SELECT c FROM number1) UNION (SELECT c FROM number2);
+---+
|  c|
+---+
|  1|
|  3|
|  5|
|  4|
|  2|
+---+

(SELECT c FROM number1) UNION DISTINCT (SELECT c FROM number2);
+---+
|  c|
+---+
|  1|
|  3|
|  5|
|  4|
|  2|
+---+

SELECT c FROM number1 UNION ALL (SELECT c FROM number2);
+---+
|  c|
+---+
|  3|
|  1|
|  2|
|  2|
|  3|
|  4|
|  5|
|  1|
|  2|
|  2|
+---+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)