---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/23/2020
title: Vacuum - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 VACUUM 语法。
ms.openlocfilehash: 33f854185d51b8d922443bd255f6077cc72a5f17
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060667"
---
# <a name="vacuum"></a>清空

清除与表关联的文件。 对于 Apache Spark 和 Delta 表，此命令有不同版本。

## <a name="vacuum-a-spark-table"></a>清空 Spark 表

```sql
VACUUM [ [db_name.]table_name | path] [RETAIN num HOURS]
```

**``RETAIN num HOURS``**

保留期阈值。

以递归方式清空与 Spark 表关联的目录，并删除超过保留期阈值的未提交文件。 默认阈值为 7 天。 Azure Databricks 在数据写入时自动触发 ``VACUUM`` 操作。 请参阅[清除未提交的文件](../../../latest/spark-sql/dbio-commit.md#vacuum-spark)。

## <a name="vacuum-a-delta-table-delta-lake-on-azure-databricks"></a><a id="vacuum-a-delta-table-delta-lake-on-azure-databricks"> </a><a id="vacuum-delta"> </a>清空 Delta 表（Azure Databricks 上的 Delta Lake）

```sql
VACUUM [ [db_name.]table_name | path] [RETAIN num HOURS] [DRY RUN]
```

以递归方式清空与 Delta 表关联的目录，并删除不再处于表事务日志最新状态且超过保留期阈值的数据文件。
根据从 Delta 的事务日志中以逻辑方式删除文件的时间和保留时间（而不是其在存储系统上的修改时间戳）删除这些文件。

默认阈值为 7 天。 Azure Databricks 不会对 Delta 表自动触发 ``VACUUM`` 操作。 请参阅[删除 Delta 表不再引用的文件](../../../../delta/delta-utility.md#delta-vacuum)。

如果对 Delta 表运行 ``VACUUM``，则将无法再回头[按时间顺序查看](../../../../delta/delta-batch.md#deltatimetravel)在指定数据保留期之前创建的版本。

**``RETAIN num HOURS``**

保留期阈值。

**``DRY RUN``**

返回要删除的文件的列表。