---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 05/19/2020
title: StreamSets 集成 - Azure Databricks
description: 了解如何设置 Azure Databricks 来使其与 StreamSets 进行集成。
ms.openlocfilehash: 6ce298fbada35414c93b39e71ed1c97f70936388
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059960"
---
# <a name="streamsets-integration"></a>StreamSets 集成

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。

StreamSets 可帮助你在数据流的整个生命周期管理和监视数据流。 通过 Streamsets 与 Azure Databricks 和 Delta Lake 的本机集成，可从各种来源拉取数据，轻松管理管道。

下面是结合使用 StreamSets 与 Azure Databricks 的步骤。

## <a name="step-1-generate-a-databricks-personal-access-token"></a><a id="step-1-generate-a-databricks-personal-access-token"> </a><a id="token"> </a>步骤 1：生成 Databricks 个人访问令牌

StreamSets 使用 Azure Databricks 个人访问令牌在 Azure Databricks 中进行身份验证。 若要生成个人访问令牌，请按照[生成个人访问令牌](../../dev-tools/api/latest/authentication.md#token-management)中的说明操作。

## <a name="step-2-set-up-a-cluster-to-support-integration-needs"></a><a id="cluster"> </a><a id="step-2-set-up-a-cluster-to-support-integration-needs"> </a>步骤2：设置群集来支持集成需求

StreamSets 会将数据写入 Azure Data Lake Storage 路径，而 Azure Databricks 集成群集将从该位置读取数据。 因此，集成群集需要能够安全地访问 Azure Data Lake Storage 路径。

### <a name="secure-access-to-an-azure-data-lake-storage-path"></a>安全地访问 Azure Data Lake Storage 路径

若要安全地访问 Azure Data Lake Storage (ADLS) 中的数据，可使用 Azure 存储帐户访问密钥（推荐）或 Azure 服务主体。

#### <a name="use-an-azure-storage-account-access-key"></a>使用 Azure 存储帐户访问密钥

可在配置 Apache Spark 期间在集成群集上配置存储帐户访问密钥。 确保存储帐户可访问用于暂存数据的 ADLS 容器和文件系统，以及要在其中写入 Delta Lake 表的 ADLS 容器和文件系统。 若要将集成群集配置为使用密钥，请按照[使用存储密钥访问 ADLS Gen2](../../data/data-sources/azure/azure-datalake-gen2.md#adls-gen2-access-key) 中的步骤操作。

#### <a name="use-an-azure-service-principal"></a>使用 Azure 服务主体

可在配置 Apache Spark 期间在 Azure Databricks 集成群集上配置服务主体。 确保服务主体可访问用于暂存数据的 ADLS 容器，以及要在其中写入 Delta 表的 ADLS 容器。 若要将集成群集配置为使用服务主体，请按照[使用服务主体访问 ADLS Gen2](../../data/data-sources/azure/azure-datalake-gen2.md#adls-gen2-oauth-2) 或[使用服务主体访问 ADLS Gen1](../../data/data-sources/azure/azure-datalake.md#adls-gen1-oauth-2) 中的步骤操作。

###  <a name="specify-the-cluster-configuration"></a>指定群集配置

1. 在“群集模式”下拉列表中，选择“标准” 。
2. 在“Databricks Runtime 版本”下拉列表中，选择 Runtime 6.3 或更高版本。
3. 将以下属性添加到 [Spark 配置](../../clusters/configure.md#spark-config)来打开[自动优化](../../delta/optimizations/auto-optimize.md)：

   ```ini
   spark.databricks.delta.optimizeWrite.enabled true
   spark.databricks.delta.autoCompact.enabled true
   ```

4. 根据集成和缩放需求配置群集。

有关群集配置的详细信息，请参阅[配置群集](../../clusters/configure.md)。

有关获取 JDBC URL 和 HTTP 路径的步骤，请参阅[获取服务器主机名、端口、HTTP 路径和 JDBC URL](../bi/jdbc-odbc-bi.md#get-server-hostname-port-http-path-and-jdbc-url)。

## <a name="step-3-obtain-jdbc-and-odbc-connection-details-to-connect-to-a-cluster"></a><a id="connection"> </a><a id="step-3-obtain-jdbc-and-odbc-connection-details-to-connect-to-a-cluster"> </a>步骤 3：获取 JDBC 和 ODBC 连接详细信息以连接到群集

若要将 Azure Databricks 群集连接到 StreamSets，需要以下 JDBC/ODBC 连接属性：

* JDBC URL
* HTTP 路径

## <a name="step-4-get-streamsets-for-azure-databricks"></a>步骤 4：获取适用于 Azure Databricks 的 Streamsets

注册并启动 [Azure 上适用于 Databricks 的 Streamsets](https://go.streamsets.com/databricks-connector-azure.html)。

## <a name="step-5-learn-how-to-use-streamsets-to-load-data-into-delta-lake"></a>步骤 5：了解如何使用 Streamsets 将数据加载到 Delta Lake

从示例管道开始，或查看 [Streamsets 解决方案](https://streamsets.com/documentation/datacollector/latest/help/index.html?contextID=concept_a5b_wvk_ckb)，了解如何构建将数据引入到 Delta Lake 的管道。