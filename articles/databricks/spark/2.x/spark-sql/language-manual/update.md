---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/04/2020
title: 更新（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 UPDATE（表）语法。
ms.openlocfilehash: 9050a1e8ac8ef6f2cfd4e33e6134306042431a24
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060722"
---
# <a name="update--delta-lake-on-azure-databricks"></a>更新（Azure Databricks 上的 Delta Lake）

```sql
UPDATE [db_name.]table_name [AS alias] SET col1 = value1 [, col2 = value2 ...] [WHERE predicate]
```

更新与谓词匹配的行的列值。 如果未提供谓词，则更新所有行的列值。

> [!NOTE]
>
> （Azure Databricks 上的 Delta Lake）如果列具有 ``NOT NULL`` [约束](create-table.md#create-table-delta)，并且 ``INSERT INTO`` 语句将列值设置为 ``NULL``，则会引发 ``SparkException``。

**``WHERE``**

按谓词筛选行。

## <a name="example"></a>示例

```sql
UPDATE events SET eventType = 'click' WHERE eventType = 'clk'
```

``UPDATE`` 以 ``WHERE`` 谓词支持子查询，包括 ``IN``、``NOT IN``、``EXISTS``、``NOT EXISTS`` 和标量子查询

## <a name="subquery-examples"></a>子查询示例

```sql
UPDATE all_events
  SET session_time = 0, ignored = true
  WHERE session_time < (SELECT min(session_time) FROM good_events)

UPDATE orders AS t1
  SET order_status = 'returned'
  WHERE EXISTS (SELECT oid FROM returned_orders WHERE t1.oid = oid)

UPDATE events
  SET category = 'undefined'
  WHERE category NOT IN (SELECT category FROM events2 WHERE date > '2001-01-01')
```

> [!NOTE]
>
> 不支持以下类型的子查询：
>
> * 嵌套子查询，即一个子查询内的另一个子查询
> * ``OR`` 中的 ``NOT IN`` 子查询，例如 ``a = 3 OR b NOT IN (SELECT c from t)``
>
> 在大多数情况下，可以使用 ``NOT EXISTS`` 重写 ``NOT IN`` 子查询。 建议尽可能使用 ``NOT EXISTS``，因为执行带有 ``NOT IN`` 子查询的 ``UPDATE`` 可能会速度较慢。