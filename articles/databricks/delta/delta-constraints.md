---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/01/2020
title: 约束 - Azure Databricks
description: 了解 Delta 表如何应用约束。
ms.openlocfilehash: f47056e84a8d97e0b8445c131bc89f5a357626f2
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061047"
---
# <a name="constraints"></a>约束

Delta 表支持标准 SQL 约束管理子句，以确保自动验证添加到表中的数据的质量和完整性。 当违反约束时，Delta Lake 将引发 ``InvariantViolationException`` 以指示无法添加新数据。

支持两种类型的约束：

* ``NOT NULL``：指示特定列中的值不能为 null。
* ``CHECK``：指示每个输入行的指定的布尔表达式必须为 true。

## <a name="not-null-constraint"></a>``NOT NULL`` 约束

在创建表时，在架构中指定 ``NOT NULL`` 约束，并使用 ``ALTER TABLE CHANGE COLUMN`` 命令删除 ``NOT NULL`` 约束。

```sql
CREATE TABLE events(
  id LONG NOT NULL,
  date STRING NOT NULL,
  location STRING,
  description STRING
);

ALTER TABLE events CHANGE COLUMN date DROP NOT NULL;
```

可以使用 ``ALTER TABLE CHANGE COLUMN`` 命令向现有 Delta 表添加 ``NOT NULL`` 约束。

```sql
CREATE TABLE events(
  id LONG,
  date STRING,
  location STRING,
  description STRING
);

ALTER TABLE events CHANGE COLUMN id SET NOT NULL;
```

如果在结构中嵌套的列上指定了 ``NOT NULL`` 约束，则父结构也被约束为 not null。 在数组或映射类型中嵌套的列不接受 ``NOT NULL`` 约束。

有关详细信息，请参阅[创建 Delta 表](../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-table-datasource.md#create-delta-table)。

## <a name="check-constraint"></a>``CHECK`` 约束

> [!NOTE]
>
> * 适用于 Databricks Runtime 7.4 及更高版本。
> * 在 Databricks Runtime 7.3 LTS 中，你可以写入已定义 ``CHECK`` 约束的表，但不能创建 ``CHECK`` 约束。

使用 ``ALTER TABLE ADD CONSTRAINT`` 和 ``ALTER TABLE DROP CONSTRAINT`` 命令管理 ``CHECK`` 约束。 在将约束添加到表中之前，``ALTER TABLE ADD CONSTRAINT`` 会验证所有现有行是否满足约束。

```sql
CREATE TABLE events(
  id LONG NOT NULL,
  date STRING,
  location STRING,
  description STRING
);

ALTER TABLE events ADD CONSTRAINT dateWithinRange CHECK date > '1900-01-01';
ALTER TABLE events DROP CONSTRAINT dateWithinRange;
```

``CHECK`` 约束在 ``DESCRIBE DETAIL`` 和 ``SHOW TBLPROPERTIES`` 命令的输出中显示为表属性。

```sql
ALTER TABLE events ADD CONSTRAINT validIds CHECK (id > 1000 and id < 999999);
DESCRIBE DETAIL events;
```

> [!div class="mx-imgBorder"]
> ![显示的约束](../_static/images/delta/constraint-output.png)

有关详细信息，请参阅 [ADD CONSTRAINT](../spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-alter-table.md#add-constraint)。