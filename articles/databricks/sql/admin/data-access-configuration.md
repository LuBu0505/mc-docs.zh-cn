---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/23/2020
title: 数据访问配置 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 管理员如何配置从各个 SQL 终结点对数据对象的访问。
ms.openlocfilehash: f746bda870273a45c2aade988c7ab93c0a49379b
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692220"
---
# <a name="data-access-configuration"></a>数据访问配置

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

本文介绍 Azure Databricks SQL Analytics 管理员对所有 SQL 终结点执行的数据访问配置。

> [!IMPORTANT]
>
> 更改这些设置将重启所有正在运行的 SQL 终结点。

## <a name="in-this-section"></a>本节内容：

* [允许终结点访问存储](#allow-endpoints-to-access-storage)

## <a name="allow-endpoints-to-access-storage"></a>允许终结点访问存储

若要将所有终结点配置为使用 Azure 服务主体访问 Azure 存储，请在数据访问配置中设置以下属性。

1. 创建可访问资源的 [Azure AD 应用程序和服务主体](/azure-resource-manager/resource-group-create-service-principal-portal)。 请注意以下属性：
   * ``application-id``：唯一标识应用程序的 ID。
   * ``directory-id``：唯一标识 Azure AD 实例的 ID。
   * ``storage-account-name``：存储帐户的名称。
   * ``service-credential``：一个字符串，应用程序用来证明其身份。
2. 注册服务主体，并在 Azure Data Lake Storage Gen2 帐户上授予正确的[角色分配](/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)，如存储 Blob 数据参与者。
3. 在[数据访问属性](#data-access-properties)中配置以下属性：

   ```ini
   spark.hadoop.fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net OAuth
   spark.hadoop.fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider
   spark.hadoop.fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net <application-id>
   spark.hadoop.fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net {{secrets/<scope-name>/<secret-name>}}
   spark.hadoop.fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net https://login.microsoftonline.com/<directory-id>/oauth2/token
   ```

   其中，``<secret-name>`` 是包含服务主体机密的[机密](../../security/secrets/secrets.md)的密钥，而 ``<scope-name>`` 是包含密钥的范围。

### <a name="data-access-properties"></a><a id="data-access-properties"> </a><a id="instance-profile"> </a>数据访问属性

通过数据访问设置，Azure Databricks SQL Analytics 管理员可使用数据访问属性配置所有终结点。

1. 单击边栏底部的![用户设置图标](../../_static/images/icons/user-settings-icon.png)图标，然后选择“设置”。
2. 单击“SQL 终结点设置”选项卡。
3. 在“数据访问配置”文本框中，指定包含[元存储属性](#supported-properties)的键值对。
4. 单击“保存” 。

#### <a name="supported-properties"></a>支持的属性

* ``spark.sql.hive.metastore.*``:
* ``spark.sql.warehouse.dir``:
* ``spark.hadoop.datanucleus.*``:
* ``spark.hadoop.fs.*``:
* ``spark.hadoop.hive.*``:
* ``spark.hadoop.javax.jdo.option.*``:
* ``spark.hive.*``:

若要详细了解如何设置这些属性，请参阅[外部 Hive 元存储](../../data/metastores/external-hive-metastore.md)。