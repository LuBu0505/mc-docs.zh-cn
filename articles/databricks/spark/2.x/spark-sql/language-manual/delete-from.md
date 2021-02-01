---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/08/2020
title: 从（Azure Databricks 上的 Delta Lake）中删除 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 DELETE FROM 语法。
ms.openlocfilehash: ee7e3050feabc89af8c074b0d6c0c720217b5bcb
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061082"
---
# <a name="delete-from--delta-lake-on-azure-databricks"></a>从（Azure Databricks 上的 Delta Lake）中删除

```sql
DELETE FROM [db_name.]table_name [AS alias] [WHERE predicate]
```

删除与谓词匹配的行。 如果未提供谓词，则删除所有行。

**``WHERE``**

按谓词筛选行。

``WHERE`` 谓词支持子查询，包括 ``IN``、``NOT IN``、``EXISTS``、``NOT EXISTS`` 和标量子查询。 不支持以下类型的子查询：

* 嵌套子查询，即一个子查询内的另一个子查询
* ``OR`` 中的 ``NOT IN`` 子查询，例如 ``a = 3 OR b NOT IN (SELECT c from t)``

在大多数情况下，可以使用 ``NOT EXISTS`` 重写 ``NOT IN`` 子查询。 建议尽可能使用 ``NOT EXISTS``，因为执行带有 ``NOT IN`` 子查询的 ``DELETE`` 可能会速度较慢。

## <a name="example"></a>示例

```sql
DELETE FROM events WHERE date < '2017-01-01'
```

## <a name="subquery-examples"></a>子查询示例

```sql
DELETE FROM all_events
  WHERE session_time < (SELECT min(session_time) FROM good_events)

DELETE FROM orders AS t1
  WHERE EXISTS (SELECT oid FROM returned_orders WHERE t1.oid = oid)

DELETE FROM events
  WHERE category NOT IN (SELECT category FROM events2 WHERE date > '2001-01-01')
```