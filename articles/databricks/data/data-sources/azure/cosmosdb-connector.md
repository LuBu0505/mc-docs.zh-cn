---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/03/2020
title: Azure Cosmos DB - Azure Databricks
description: 了解如何使用 Azure Databricks 读取数据并将数据写入 Azure Cosmos DB。
ms.openlocfilehash: c50ef90c4ecf40607d34afd84dd48e9e9f10f2c6
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059164"
---
# <a name="azure-cosmos-db"></a>Azure Cosmos DB

[Azure Cosmos DB](/cosmos-db/) 是由 Microsoft 提供的全球分布式多模型数据库。 使用 Azure Cosmos DB 可跨任意数量的 Azure 地理区域弹性且独立地缩放吞吐量和存储。
它通过综合服务级别协议 (SLA) 提供吞吐量、延迟、可用性和一致性保证。 Azure Cosmos DB 为以下数据模型提供 API，并提供多种语言的 SDK：

* SQL API
* MongoDB API
* Cassandra API
* 图形 (Gremlin) API
* 表 API

本文介绍如何使用 Azure Databricks 从 Azure Cosmos DB 读取数据或将数据写入 Azure Cosmos DB。 有关 Azure Cosmos DB 的最新详细信息，请参阅[使用 Apache Spark 到 Azure Cosmos DB 连接器加速大数据分析](https://docs.microsoft.com/azure/cosmos-db/spark-connector#bk_working_with_connector)。

> [!IMPORTANT]
>
> 此连接器支持 Azure Cosmos DB 的核心 (SQL) API。 对于 Cosmos DB for MongoDB API，请使用 [MongoDB Spark 连接器](https://docs.mongodb.com/spark-connector/master/)。 对于 Cosmos DB Cassandra API，请使用 [Cassandra Spark 连接器](https://github.com/datastax/spark-cassandra-connector)。

> [!NOTE]
>
> 无法从运行 Databricks Runtime 7.0 或更高版本的群集访问此数据源，因为支持 Apache Spark 3.0 的 Azure Cosmos DB 连接器不可用。

## <a name="create-and-attach-required-libraries"></a>创建并附加所需的库

1. 下载[最新版 azure-cosmosdb-spark 库](/cosmos-db/spark-connector#bk_working_with_connector)以获取你正在运行的 Apache Spark 版本。
2. 按照[上传 Jar、Python Egg 或 Python Wheel](../../../libraries/workspace-libraries.md#uploading-libraries) 中的说明，将下载的 JAR 文件上传到 Databricks。
3. [安装上传的库](../../../libraries/cluster-libraries.md#install-libraries)，将其安装到 Databricks 群集中。

## <a name="use-the-azure-cosmos-db-spark-connector"></a>使用 Azure Cosmos DB Spark 连接器

下面的 Scala 笔记本提供了一个简单的示例，说明如何将数据写入 Cosmos DB 以及如何从 Cosmos DB 读取数据。 有关详细文档，请参阅 [Azure Cosmos DB Spark 连接器](https://github.com/Azure/azure-cosmosdb-spark)项目。 由 Microsoft 开发的 [Azure Cosmos DB Spark 连接器用户指南](https://github.com/Azure/azure-cosmosdb-spark/wiki/Azure-Cosmos-DB-Spark-Connector-User-Guide)也介绍了如何在 Python 中使用此连接器。

### <a name="azure-cosmos-db-notebook"></a>Azure Cosmos DB 笔记本

[获取笔记本](../../../_static/notebooks/cosmosdb.html)