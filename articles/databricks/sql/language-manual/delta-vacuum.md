---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: VACUUM - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 VACUUM 语法。
ms.openlocfilehash: 6c723b5cc1c21235d1e1be749b72953b2dcb6e53
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692280"
---
# <a name="vacuum"></a>VACUUM

清除与表关联的文件。 对于 Apache Spark 和 Delta 表，此命令有不同版本。

## <a name="vacuum-a-spark-table"></a>清空 Spark 表

以递归方式清空与 Spark 表关联的目录，并删除超过保留期阈值的未提交文件。 默认阈值为 7 天。 Azure Databricks 在数据写入时自动触发 ``VACUUM`` 操作。 请参阅[清除未提交的文件](../../spark/latest/spark-sql/dbio-commit.md#vacuum-spark)。

## <a name="syntax"></a>语法

```
VACUUM [ table_identifier | path] [RETAIN num HOURS]
```

* **table_identifier**

  ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。

* **路径**

  表文件的路径。

* **RETAIN num HOURS**

  保留期阈值。

## <a name="vacuum-a-delta-table-delta-lake-on-azure-databricks"></a><a id="vacuum-a-delta-table-delta-lake-on-azure-databricks"> </a><a id="vacuum-delta"> </a>清空 Delta 表（Azure Databricks 上的 Delta Lake）

以递归方式清空与 Delta 表关联的目录，并删除不再处于表事务日志最新状态且超过保留期阈值的数据文件。
根据从 Delta 的事务日志中以逻辑方式删除文件的时间和保留时间（而不是其在存储系统上的修改时间戳）删除这些文件。
默认阈值为 7 天。 Azure Databricks 不会对 Delta 表自动触发 ``VACUUM`` 操作。 请参阅[删除 Delta 表不再引用的文件](../../delta/delta-utility.md#delta-vacuum)。

如果对 Delta 表运行 ``VACUUM``，则将无法再回头[按时间顺序查看](../../delta/delta-batch.md#deltatimetravel)在指定数据保留期之前创建的版本。

```
VACUUM table_identifier [RETAIN num HOURS] [DRY RUN]
```

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **RETAIN num HOURS**

  保留期阈值。

* **DRY RUN**

  返回要删除的文件的列表。