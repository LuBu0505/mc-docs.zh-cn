---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/18/2020
title: 适用于 SQL 开发人员的 Azure Databricks - Azure Databricks
description: 了解 Azure Databricks 中支持的 Apache Spark 和 Delta Lake SQL 语言构造及其示例用例。
keywords: Spark SQL, 参考
ms.openlocfilehash: 2d2b45daf23f03919d864e9fb36a678540abd9b0
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060135"
---
# <a name="azure-databricks-for-sql-developers"></a>适用于 SQL 开发人员的 Azure Databricks

本部分提供了一个指南，用于指导如何使用 SQL 语言在 Azure Databricks 工作区中开发笔记本。

若要了解如何使用 Azure Databricks SQL Analytics 开发 SQL 查询，请参阅 [SQL Analytics 中的查询](../../../sql/user/queries/index.md)和 [SQL Analytics 的 SQL 参考](../../../sql/language-manual/index.md)。

## <a name="sql-language"></a><a id="spark-sql-lang-manual"> </a><a id="sql-language"> </a>SQL 语言

本部分提供了 Azure Databricks SQL 参考以及有关与 Apache Hive SQL 的兼容性的信息。

* Spark SQL

  * [Databricks Runtime 7.x (Spark SQL 3.0)](language-manual/index.md)
  * [Databricks Runtime 5.5 LTS 和 6.x (Spark SQL 2.x)](../../2.x/spark-sql/language-manual/index.md)

* [Apache Hive 兼容性](compatibility/hive.md)

## <a name="use-cases"></a><a id="spark-sql-examples"> </a><a id="use-cases"> </a>用例

* [基于成本的优化器](cbo.md)
* [“跳过数据”索引](dataskipping-index.md)
* [通过 DBIO 向云存储进行事务性写入](dbio-commit.md)
* [处理错误的记录和文件](handling-bad-records.md)
* [在交互式工作流中处理大型查询](query-watchdog.md)
* [自适应查询执行](aqe.md)

## <a name="visualizations"></a>可视化效果

Azure Databricks SQL 笔记本使用 ``display`` 函数支持各种类型的可视化效果。

* [使用 SQL 进行可视化](../../../notebooks/visualizations/index.md#visualizations-in-sql)

## <a name="interoperability"></a>互操作性

本部分介绍功能，这些功能支持 SQL 与 Azure Databricks 中支持的其他语言之间的互操作性。

* Databricks Runtime 7.x
  * [用户定义的标量函数 (UDF)](language-manual/sql-ref-functions-udf-scalar.md)
  * [用户定义的聚合函数 (UDAF)](language-manual/sql-ref-functions-udf-aggregate.md)
* Databricks Runtime 5.5 LTS 和 6.x

  * [用户定义的函数 - Python](udf-python.md)
  * [用户定义的函数 - Scala](udf-scala.md)
  * [用户定义的聚合函数 - Scala](udaf-scala.md)

## <a name="tools"></a>工具

除了 Azure Databricks 笔记本以外，还可以使用以下商业智能工具：

* [商业智能工具](../../../integrations/bi/index.md)

## <a name="access-control"></a>访问控制

本文介绍如何使用 SQL 构造来控制对数据库对象的访问：

* [数据对象特权](../../../security/access-control/table-acls/object-privileges.md)

## <a name="resources"></a>资源

* [Apache Spark SQL 指南](https://spark.apache.org/docs/latest/sql-programming-guide.html)
* [Delta Lake 和 Delta Engine 指南](../../../delta/index.md)
* [知识库](https://docs.microsoft.com/azure/databricks/kb/sql/)