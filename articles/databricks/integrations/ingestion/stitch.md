---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 04/20/2020
title: Stitch 集成 - Azure Databricks
description: 了解如何设置 Azure Databricks 来使其与 Stitch 进行集成。
ms.openlocfilehash: 0296b18bd14c463111a63f0937ee08e01215e19c
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058498"
---
# <a name="stitch-integration"></a>Stitch 集成

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。

Stitch 可帮助你将不同数据库和 SaaS 应用程序（Salesforce、Hubspot 和 Marketo 等）中的所有业务数据整合到 Delta Lake。

下面是结合使用 Stitch 与 Azure Databricks 的步骤。

## <a name="step-1-generate-a-databricks-personal-access-token"></a><a id="step-1-generate-a-databricks-personal-access-token"> </a><a id="token"> </a>步骤 1：生成 Databricks 个人访问令牌

Stitch 使用 Azure Databricks 个人访问令牌在 Azure Databricks 中进行身份验证。 若要生成个人访问令牌，请按照[生成个人访问令牌](../../dev-tools/api/latest/authentication.md#token-management)中的说明操作。

## <a name="step-2-set-up-a-cluster-to-support-integration-needs"></a><a id="cluster"> </a><a id="step-2-set-up-a-cluster-to-support-integration-needs"> </a>步骤2：设置群集来支持集成需求

Stitch 会将数据写入 Azure Data Lake Storage 路径，而 Azure Databricks 集成群集将从该位置读取数据。 因此，集成群集需要能够安全地访问 Azure Data Lake Storage 路径。

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

## <a name="step-3-configure-stitch-with-azure-databricks"></a>步骤 3：使用 Azure Databricks 配置 Stitch

转到 [Stitch](https://www.stitchdata.com/signup/?utm_source=partner&utm_medium=app&utm_campaign=databricks) 登录页面，然后按照说明进行操作。