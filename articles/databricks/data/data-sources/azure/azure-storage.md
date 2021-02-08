---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 09/11/2020
title: Azure Blob 存储 - Azure Databricks
description: 了解如何使用 Azure Databricks 读取数据并将数据写入 Azure Blob 存储。
ms.openlocfilehash: 595feee0e66dfc6522c7c2c9fd460baac8bb1092
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059169"
---
# <a name="azure-blob-storage"></a><a id="azure-blob-storage"> </a><a id="azure-storage"> </a>Azure Blob 存储

[Azure Blob 存储](/storage/blobs/)是一项用于存储大量非结构化对象数据（例如文本数据或二进制数据）的服务。 可以使用 Blob 存储向外公开数据，或者私下存储应用程序数据。 Blob 存储的常见用途包括：

* 直接向浏览器提供图像或文档
* 存储文件以供分布式访问
* 对视频和音频进行流式处理
* 存储用于备份和还原、灾难恢复及存档的数据
* 存储数据以供本地或 Azure 托管服务执行分析

> [!NOTE]
>
> Azure Databricks 还支持以下 Azure 数据源：[Azure Data Lake Storage Gen2](azure-datalake-gen2.md)、[Azure Cosmos DB](cosmosdb-connector.md) 和 [Azure Synapse Analytics](synapse-analytics.md)。

本文介绍了如何通过以下方式访问 Azure Blob 存储：使用 [Databricks 文件系统 (DBFS)](../../databricks-file-system.md) 来装载存储，或直接使用 API。

## <a name="requirements"></a>要求

可从公共存储帐户读取数据，无需任何其他设置。 若要从专用存储帐户读取数据，必须配置[共享密钥](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key)或[共享访问签名 (SAS)](/storage/common/storage-dotnet-shared-access-signature-part-1)。 为了在 Azure Databricks 中安全地使用凭据，建议按[机密管理](../../../security/secrets/index.md#secrets-user-guide)用户指南进行操作，如[装载 Azure Blob 存储容器](#mount-azure-blob)中所示。

## <a name="mount-azure-blob-storage-containers-to-dbfs"></a><a id="mount-azure-blob-storage"> </a><a id="mount-azure-blob-storage-containers-to-dbfs"> </a>将 Azure Blob 存储容器装载至 DBFS

可将 Blob 存储容器或容器中的某个文件夹装载到 DBFS。 此装载是指向一个 Blob 存储容器的指针，因此数据永远不会在本地同步。

> [!IMPORTANT]
>
> * Azure Blob 存储支持[三种 Blob 类型](/storage/blobs/storage-blobs-introduction#blobs)：块 Blob、追加 Blob 和页 Blob。 只能将块 Blob 装载到 DBFS。
> * 所有用户都对装载到 DBFS 的 Blob 存储容器中的对象具有读写访问权限。
> * 通过群集创建装入点后，该群集的用户可立即访问装入点。 若要在另一个正在运行的群集中使用装入点，则必须在运行的群集上运行 `dbutils.fs.refreshMounts()`，使新创建的装入点可供使用。

DBFS 使用在创建装入点时提供的凭据来访问已装载的 Blob 存储容器。
如果 Blob 存储容器是使用存储帐户访问密钥装载的，则 DBFS 在访问此装入点时会使用从存储帐户密钥派生的临时 SAS 令牌。

### <a name="mount-an-azure-blob-storage-container"></a><a id="mount-an-azure-blob-storage-container"> </a><a id="mount-azure-blob"> </a>装载 Azure Blob 存储容器

1. 若要在容器内装载 Blob 存储容器或文件夹，请使用以下命令：

   #### <a name="python"></a>Python

   ```python
   dbutils.fs.mount(
     source = "wasbs://<container-name>@<storage-account-name>.blob.core.chinacloudapi.cn",
     mount_point = "/mnt/<mount-name>",
     extra_configs = {"<conf-key>":dbutils.secrets.get(scope = "<scope-name>", key = "<key-name>")})
   ```

   #### <a name="scala"></a>Scala

   ```scala
   dbutils.fs.mount(
     source = "wasbs://<container-name>@<storage-account-name>.blob.core.chinacloudapi.cn/<directory-name>",
     mountPoint = "/mnt/<mount-name>",
     extraConfigs = Map("<conf-key>" -> dbutils.secrets.get(scope = "<scope-name>", key = "<key-name>")))
   ```

   where

   * ``<storage-account-name>`` 是你的 Azure Blob 存储帐户的名称。
   * ``<container-name>`` 是你的 Azure Blob 存储帐户中某个容器的名称。
   * `<mount-name>` 是一个 DBFS 路径，表示 Blob 存储容器或该容器中的某个文件夹（在 `source` 中指定）要装载到 DBFS 中的什么位置。
   * `<conf-key>` 可以是 `fs.azure.account.key.<storage-account-name>.blob.core.chinacloudapi.cn` 或 `fs.azure.sas.<container-name>.<storage-account-name>.blob.core.chinacloudapi.cn`
   * `dbutils.secrets.get(scope = "<scope-name>", key = "<key-name>")` 获取在[机密范围](../../../security/secrets/secret-scopes.md)中存储为[机密](../../../security/secrets/secrets.md)的密钥。
2. 像访问本地文件一样访问容器中的文件，例如：

   #### <a name="python"></a>Python

   ```python
   # python
   df = spark.read.text("/mnt/<mount-name>/...")
   df = spark.read.text("dbfs:/<mount-name>/...")
   ```

   #### <a name="scala"></a>Scala

   ```scala
   // scala
   val df = spark.read.text("/mnt/<mount-name>/...")
   val df = spark.read.text("dbfs:/<mount-name>/...")
   ```

   #### <a name="sql"></a>SQL

   ```sql
   -- SQL
   CREATE DATABASE <db-name>
   LOCATION "/mnt/<mount-name>"
   ```

### <a name="unmount-a-mount-point"></a>卸载装入点

若要卸载装入点，请使用以下命令：

```python
dbutils.fs.unmount("/mnt/<mount-name>")
```

## <a name="access-azure-blob-storage-directly"></a>直接访问 Azure Blob 存储

本部分介绍了如何使用 Spark 数据帧 API、RDD API 和 Hive 客户端访问 Azure Blob 存储。

### <a name="access-azure-blob-storage-using-the-dataframe-api"></a>使用数据帧 API 访问 Azure Blob 存储

你需要先配置凭据（会话凭据或群集凭据），然后才能访问 Azure Blob 存储中的数据。

在笔记本中运行以下命令来配置会话凭据：

* 设置帐户访问密钥：

  ```python
  spark.conf.set(
    "fs.azure.account.key.<storage-account-name>.blob.core.chinacloudapi.cn",
    "<storage-account-access-key>")
  ```

* 为容器设置 SAS：

  ```python
  spark.conf.set(
    "fs.azure.sas.<container-name>.<storage-account-name>.blob.core.chinacloudapi.cn",
    "<complete-query-string-of-sas-for-the-container>")
  ```

若要配置群集凭据，请在创建群集时设置 [Spark 配置](../../../clusters/configure.md#spark-config)属性：

* 配置帐户访问密钥：

  ```ini
  fs.azure.account.key.<storage-account-name>.blob.core.windows.net <storage-account-access-key>
  ```

* 为容器配置 SAS：

  ```ini
  fs.azure.sas.<container-name>.<storage-account-name>.blob.core.windows.net <complete-query-string-of-sas-for-the-container>
  ```

> [!WARNING]
>
> 这些凭据可供访问群集的所有用户使用。

在笔记本或群集配置中设置帐户访问密钥或 SAS 后，可使用标准 Spark 和 Databricks API 读取存储帐户中的内容：

```scala
val df = spark.read.parquet("wasbs://<container-name>@<storage-account-name>.blob.core.chinacloudapi.cn/<directory-name>")

dbutils.fs.ls("wasbs://<container-name>@<storage-account-name>.blob.core.chinacloudapi.cn/<directory-name>")
```

### <a name="access-azure-blob-storage-using-the-rdd-api"></a>使用 RDD API 访问 Azure Blob 存储

不能通过 ``SparkContext`` 访问 Hadoop 配置选项。 如果使用 RDD API 从 Azure Blob 存储读取数据，则必须在创建群集时将 Hadoop 凭据配置属性设置为 [Spark 配置](../../../clusters/configure.md#spark-config)选项，并将 ``spark.hadoop.`` 前缀添加到相应的 Hadoop 配置密钥，以便将其传播到用于 RDD 作业的 Hadoop 配置：

* 配置帐户访问密钥：

  ```ini
  spark.hadoop.fs.azure.account.key.<storage-account-name>.blob.core.chinacloudapi.cn <storage-account-access-key>
  ```

* 为容器配置 SAS：

  ```ini
  spark.hadoop.fs.azure.sas.<container-name>.<storage-account-name>.blob.core.chinacloudapi.cn <complete-query-string-of-sas-for-the-container>
  ```

> [!WARNING]
>
> 这些凭据可供访问群集的所有用户使用。

### <a name="access-azure-blob-storage-from-the-hive-client"></a>从 Hive 客户端访问 Azure Blob 存储

Hive 客户端无法访问在笔记本的会话配置中设置的凭据。 若要将凭据传播到 Hive 客户端，必须在创建群集时将 Hadoop 凭据配置属性设置为 [Spark 配置](../../../clusters/configure.md#spark-config)选项：

* 配置帐户访问密钥：

  ```ini
  spark.hadoop.fs.azure.account.key.<storage-account-name>.blob.core.windows.net <storage-account-access-key>
  ```

* 为容器配置 SAS：

  ```ini
  # Using a SAS token
  spark.hadoop.fs.azure.sas.<container-name>.<storage-account-name>.blob.core.chinacloudapi.cn <complete-query-string-of-sas-for-the-container>
  ```

> [!WARNING]
>
> 这些凭据可供访问群集的所有用户使用。

在群集配置中设置帐户访问密钥或 SAS 后，可以对 Azure Blob 存储使用标准 Hive 查询：

```sql
-- SQL
CREATE DATABASE <db-name>
LOCATION "wasbs://<container-name>@<storage-account-name>.blob.core.chinacloudapi.cn/";
```

以下笔记本演示了如何通过 Spark API、Databricks API 和 Hive 装载 Azure Blob 存储并访问数据。

#### <a name="azure-blob-storage-notebook"></a>Azure Blob 存储笔记本

[获取笔记本](../../../_static/notebooks/data-sources/mount-azure-blob-storage.html)