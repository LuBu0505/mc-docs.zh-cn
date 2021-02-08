---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/01/2020
title: 使用 Apache Spark 连接器的 SQL 数据库 - Azure Databricks
description: 了解如何使用 Apache Spark 连接器在 Azure Databricks 中读取数据并将数据写入到 Azure SQL 数据库和 Microsoft SQL Server。
ms.openlocfilehash: 7d20b8a216e54cfb7eb407bf1bcff24694b2efd1
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059877"
---
# <a name="sql-databases-using-the-apache-spark-connector"></a><a id="azure-db"> </a><a id="sql-databases-using-the-apache-spark-connector"> </a>使用 Apache Spark 连接器的 SQL 数据库

借助[适用于 Azure SQL 数据库和 SQL Server 的 Apache Spark 连接器](/sql-database/sql-database-spark-connector)，这些数据库可以充当 Apache Spark 作业的输入数据源和输出数据接收器。 由此，可在大数据分析中使用实时事务数据，并保留临时查询或报告的结果。

与内置 JDBC 连接器相比，此连接器能够将数据批量插入 SQL 数据库。 它的性能可以比逐行插入快 10 倍到 20 倍。 适用于 SQL Server 和 Azure SQL 数据库的 Spark 连接器还支持 Azure Active Directory (Azure AD) 身份验证，从而使你可以使用 Azure AD 帐户从 Azure Databricks 安全地连接到 Azure SQL 数据库。 它提供类似于内置 JDBC 连接器的接口。 可以轻松迁移现有的 Spark 作业以使用此连接器。

> [!NOTE]
>
> 支持 Databricks Runtime 7.x 的 Spark 连接器尚不可用。 Databricks 建议使用 [JDBC](sql-databases.md) 连接器或 Databricks Runtime 6.4 或 Databricks Runtime 5.5 LTS。

## <a name="requirements"></a>要求

| 组件                               | 支持的版本                |
|-----------------------------------------|-----------------------------------|
| Databricks Runtime                      | 6.6 和更低版本                     |
| Apache Spark                            | 2.4.x（不支持 Spark 3.0）   |
| Scala                                   | 2.11                              |
| Microsoft JDBC Driver for SQL Server    | 8.2                               |
| Microsoft SQL Server                    | SQL Server 2008 及更高版本         |
| Azure SQL 数据库                      | 支持                         |

## <a name="use-the-spark-connector"></a>使用 Spark 连接器

有关使用 Spark 连接器的说明，请参阅 [Apache Spark 连接器：SQL Server 和 Azure SQL](https://docs.microsoft.com/sql/connect/spark/connector)。