---
title: 支持 Azure Data Lake Storage Gen2 的 Azure 服务 | Microsoft Docs
description: 了解哪些 Azure 服务与 Azure Data Lake Storage Gen2 集成
author: WenJason
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 08/05/2020
ms.date: 01/18/2021
ms.author: v-jay
ms.reviewer: stewu
ms.openlocfilehash: 8d85e056c39a5c95681d7dac84e7200369ae7a26
ms.sourcegitcommit: f086abe8bd2770ed10a4842fa0c78b68dbcdf771
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98163225"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>支持 Azure Data Lake Storage Gen2 的 Azure 服务

可以使用 Azure 服务来引入数据、执行分析和创建可视化表示形式。 本文提供了受支持的 Azure 服务的列表，公开了它们的支持级别，并提供了一些文章的链接，这些文章可帮助你将这些服务与 Azure Data Lake Storage Gen2 配合使用。

## <a name="supported-azure-services"></a>支持的 Azure 服务

下表列出了可与 Azure Data Lake Storage Gen2 配合使用的 Azure 服务。 随着支持的不断扩展，这些表中出现的项将随着时间的推移而变化。

> [!NOTE]
> 支持级别仅指 Data Lake Storage Gen2 支持该服务的方式。

|Azure 服务 |支持级别 |Azure AD |共享密钥| 相关文章 |
|---------------|-------------------|---|---|---|
|Azure 数据工厂|正式发布|是|是|[使用 Azure 数据工厂将数据加载到 Azure Data Lake Storage Gen2 中](../../data-factory/load-azure-data-lake-storage-gen2.md?toc=%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|正式发布|是|是|与 Azure Databricks 配合使用 <br> [快速入门：使用 Azure Databricks 分析 Azure Data Lake Storage Gen2 中的数据](data-lake-storage-quickstart-create-databricks-account.md) <br>[教程：使用 Spark 通过 Azure Databricks 访问 Data Lake Storage Gen2 数据](data-lake-storage-use-databricks-spark.md)|
|Azure 事件中心|正式发布|否|是|[通过 Azure Blob 存储或 Azure Data Lake Storage 中的 Azure 事件中心来捕获事件](../../event-hubs/event-hubs-capture-overview.md)|
|Azure 事件网格|正式发布|是|是|[教程：实现数据湖捕获模式以更新 Databricks Delta 表](data-lake-storage-events.md)|
|Azure 逻辑应用|正式发布|否|是|[概述 - 什么是 Azure 逻辑应用？](../../logic-apps/logic-apps-overview.md)|
|Azure 流分析|正式发布|是|是|[快速入门：使用 Azure 门户创建流分析作业](../../stream-analytics/stream-analytics-quick-create-portal.md) <br> [Azure Data Lake Gen2 流出量](../../stream-analytics/stream-analytics-define-outputs.md)|
|HDInsight |正式发布|是|是|[将 Azure Data Lake Storage Gen2 用于 Azure HDInsight 群集](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md?toc=%2fstorage%2fblobs%2ftoc.json)<br>[将 HDFS CLI 与 Data Lake Storage Gen2 配合使用](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[教程：使用 Apache Hive on Azure HDInsight 提取、转换和加载数据](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT 中心 |正式发布|是|是|[使用 IoT 中心消息路由将设备到云的消息发送到不同的终结点](../../iot-hub/iot-hub-devguide-messages-d2c.md)|
|Power BI|正式发布|是|是|[使用 Power BI 分析 Data Lake Storage Gen2 中的数据](https://docs.microsoft.com/power-query/connectors/datalakestorage)|
|Azure Synapse Analytics（以前称为 SQL 数据仓库）|正式发布|是|是|[用于 Azure Synapse Analytics（以前称为 SQL 数据仓库）](../../azure-sql/database/vnet-service-endpoint-rule-overview.md?toc=%2fstorage%2fblobs%2ftoc.json)|
|SQL Server 集成服务 (SSIS)|正式发布|是|是|[Azure 存储连接管理器](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager)|
|Azure 数据资源管理器|正式发布|是|是|[使用 Azure 数据资源管理器查询 Azure Data Lake 中的数据](/data-explorer/data-lake-query-data)|
|Azure 认知搜索|预览|是|是|[为 Azure Data Lake Storage Gen2 文档编制索引及对其进行搜索（预览）](../../search/search-howto-index-azure-data-lake-storage.md)|
|Azure 内容分发网络|尚不支持|不适用|不适用|[为 Azure Data Lake Storage Gen2 文档编制索引及对其进行搜索（预览）](/cdn/cdn-overview)|
|Azure SQL 数据库|尚不支持|不适用|不适用|[什么是 Azure SQL 数据库？](/azure-sql/database/sql-database-paas-overview)|

## <a name="see-also"></a>另请参阅

- [Azure Data Lake Storage Gen2 的已知问题](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage Gen2 中可用的 Blob 存储功能](data-lake-storage-supported-blob-storage-features.md)
- [支持 Azure Data Lake Storage Gen2 的开源平台](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage 的多协议访问](data-lake-storage-multi-protocol-access.md)