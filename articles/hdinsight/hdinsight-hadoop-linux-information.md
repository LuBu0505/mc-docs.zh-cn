---
title: 有关在基于 Linux 的 HDInsight 上使用 Hadoop 的提示 - Azure | Azure
description: 获取有关在 Azure 云中运行的你所熟悉的 Linux 环境中使用基于 Linux 的 HDInsight (Hadoop) 群集的实施提示。
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 04/29/2020
ms.date: 06/22/2020
ms.author: v-yiso
ms.openlocfilehash: b02da933f2f975c00d35344b82954e6039413916
ms.sourcegitcommit: 537d52cb783892b14eb9b33cf29874ffedebbfe3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92472654"
---
# <a name="information-about-using-hdinsight-on-linux"></a>有关在 Linux 上使用 HDInsight 的信息

Azure HDInsight 群集提供了基于熟悉的 Linux 环境并在 Azure 云中运行的 Apache Hadoop。 在大多数情况下，它的工作方式应该与其他任何 Hadoop-on-Linux 安装完全相同。 本文档指出了你应该注意的具体差异。

## <a name="prerequisites"></a>先决条件

本文档中的许多步骤使用以下实用程序，这些程序可能需要在系统上安装。

* [cURL](https://curl.haxx.se/) - 用于与基于 Web 的服务进行通信。
* 命令行 JSON 处理程序 **jq** 。  请参阅 [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)。
* [Azure CLI](/cli/install-azure-cli) - 用于远程管理 Azure 服务。
* **SSH 客户端** 。 有关详细信息，请参阅[使用 SSH 连接到 HDInsight (Apache Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md)。

## <a name="users"></a>用户

除非 [加入域](./domain-joined/hdinsight-security-overview.md)，HDInsight 应被视为 **单用户** 系统。 单一 SSH 用户帐户是使用具有管理员级别权限的群集创建的。 可创建其他 SSH 帐户，但它们也具有对群集的管理员权限。

加入域的 HDInsight 支持多个用户、更具体的权限以及角色设置。 有关详细信息，请参阅[管理已加入域的 HDInsight 群集](./domain-joined/apache-domain-joined-manage.md)。

## <a name="domain-names"></a>域名

从 Internet 连接到群集时要使用的完全限定域名 (FQDN) 是 `CLUSTERNAME.azurehdinsight.cn` 或 `CLUSTERNAME-ssh.azurehdinsight.cn`（仅适用于 SSH）。

就内部来说，群集中的每个节点都有一个在群集配置期间分配的名称。 若要查找群集名称，请参阅 Ambari Web UI 上的 **主机** 页。 还可以使用以下方法从 Ambari REST API 返回主机列表：

```console
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.cn/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'
```

将 `CLUSTERNAME` 替换为群集的名称。 出现提示时，请输入管理员帐户的密码。 此命令返回包含群集中主机列表的 JSON 文档。 [jq](https://stedolan.github.io/jq/) 用于为每个主机提取 `host_name` 元素值。

若需查找特定服务的节点的名称，可查询 Ambari 以获取该组件。 例如，若需查找 HDFS 名称节点的主机，请使用以下命令：

```console
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.cn/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'
```

此命令会返回一个描述该服务的 JSON 文档，然后 [jq](https://stedolan.github.io/jq/) 就会只拉取主机的 `host_name` 值。

## <a name="remote-access-to-services"></a>对服务的远程访问

* **Ambari (web)**  - `https://CLUSTERNAME.azurehdinsight.cn`

    使用群集管理员用户和密码进行身份验证，并登录到 Ambari。

    身份验证是纯文本身份验证 - 始终使用 HTTPS 来帮助确保连接是安全的。

    > [!IMPORTANT]
    > 某些 Web UI 可使用内部域名通过 Ambari 访问节点。 内部域名不可通过 Internet 公开访问。 在尝试通过 Internet 访问某些功能时，可能会收到“找不到服务器”错误。
    >
    > 要使用 Ambari web UI 的全部功能，请使用 SSH 隧道通过代理将 Web 流量传送到群集头节点。 请参阅[使用 SSH 隧道访问 Apache Ambari Web UI、ResourceManager、JobHistory、NameNode、Oozie 和其他 Web UI](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)**  - `https://CLUSTERNAME.azurehdinsight.cn/ambari`

    > [!NOTE]
    > 通过使用群集管理员用户和密码进行身份验证。
    >
    > 身份验证是纯文本身份验证 - 始终使用 HTTPS 来帮助确保连接是安全的。

* **WebHCat (Templeton)**  - `https://CLUSTERNAME.azurehdinsight.cn/templeton`

    > [!NOTE]
    > 通过使用群集管理员用户和密码进行身份验证。
    >
    > 身份验证是纯文本身份验证 - 始终使用 HTTPS 来帮助确保连接是安全的。

* **SSH** - CLUSTERNAME-ssh.azurehdinsight.cn，使用端口 22 或 23。 端口 22 用于连接主要头节点，而端口 23 用于连接辅助头节点。 有关头节点的详细信息，请参阅 [HDInsight 中的 Apache Hadoop 群集的可用性和可靠性](hdinsight-high-availability-linux.md)。

    > [!NOTE]
    > 只能通过 SSH 从客户端计算机访问群集头节点。 在连接后，可以通过使用 SSH 从头节点访问从节点。

有关详细信息，请参阅 [HDInsight 上的 Apache Hadoop 服务使用的端口](hdinsight-hadoop-port-settings-for-services.md)文档。

## <a name="file-locations"></a>文件位置

Hadoop 相关文件可在群集节点上的 `/usr/hdp`中找到。 此目录包含以下子目录：

* **2.6.5.3009-43** ：目录名称是 HDInsight 使用的 Hadoop 平台版本。 群集上的数字可能与这里列出的有所不同。
* **current** ：此目录包含 2.6.5.3009-43 目录下的子目录的链接。 由于该目录存在，因此无需记住版本号。

可以在 Hadoop 分布式文件系统上的 `/example` 和 `/HdiSamples` 处找到示例数据和 JAR 文件。

## <a name="hdfs-azure-storage-and-data-lake-storage"></a>HDFS、Azure 存储和 Data Lake Storage

在大多数 Hadoop 分发版中，数据存储在 HDFS 中。 HDFS 受群集中计算机上的本地存储的支持。 为基于云的解决方案使用本地存储可能费用高昂，因为计算资源以小时或分钟为单位来计费。

使用 HDInsight 时，数据文件使用 Azure Blob 存储以及可选的 Azure Data Lake Storage Gen2 以自适应且可复原的方式存储在云中。 这些服务提供以下优势：

* 成本低廉的长期存储。
* 可从外部服务访问，例如网站、文件上传/下载实用程序、各种语言 SDK 和 Web 浏览器。
* 大型文件容量和大型自适应存储。

有关详细信息，请参阅 [Azure Blob 存储](../storage/common/storage-introduction.md)、[Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)。

使用 Azure Blob 存储或 Data Lake Storage Gen2 时，不需要从 HDInsight 进行任何特殊操作即可访问数据。 例如，以下命令将列出 `/example/data` 文件夹中的文件，而无论它是存储在 Azure 存储还是 Data Lake Storage 上：

```console
hdfs dfs -ls /example/data
```

在 HDInsight 中，数据存储资源（Azure Blob 存储和 Azure Data Lake Storage）与计算资源相分离。 可以根据需要创建 HDInsight 群集用于执行计算，然后在工作完成后将其删除。 另外，如有需要，可以将数据文件安全持久地保存在云存储空间中。


### <a name="uri-and-scheme"></a><a name="URI-and-scheme"></a>URI 和方案

在访问文件时，一些命令可能需要用户将方案指定为 URI 的一部分。 例如，Storm-HDFS 组件需要用户指定方案。 使用非默认存储（作为“附加”存储添加到群集的存储）时，必须始终将方案作为 URI 的一部分来使用。

使用 [Azure 存储](./hdinsight-hadoop-use-blob-storage.md)时，可以使用以下 URI 方案之一：

* `wasb:///`：使用未加密的通信访问默认存储。

* `wasbs:///`：使用加密的通信访问默认存储。  仅 HDInsight 3.6 及以上版本支持 wasbs 方案。

* `wasb://<container-name>@<account-name>.blob.core.chinacloudapi.cn/`：与非默认存储帐户通信时使用。 例如，具有其他存储帐户或访问可公开访问的存储帐户中存储的数据时。

使用 [Azure Data Lake Storage Gen2](./hdinsight-hadoop-use-data-lake-storage-gen2.md) 时，可以使用以下 URI 方案：

* `abfs://`：使用加密的通信访问默认存储。

* `abfs://<container-name>@<account-name>.dfs.core.chinacloudapi.cn/`：与非默认存储帐户通信时使用。 例如，具有其他存储帐户或访问可公开访问的存储帐户中存储的数据时。

> [!IMPORTANT]  
> 使用 Data Lake Storage 作为 HDInsight 的默认存储时，必须在存储中指定一个用作 HDInsight 存储根目录的路径。 默认路径为 `/clusters/<cluster-name>/`。
>
> 使用 `/` 或 `adl:///` 访问数据时，只能访问存储在群集根目录（例如 `/clusters/<cluster-name>/`）中的数据。 若要在商店中的任意位置访问数据，请使用 `adl://<storage-name>.azuredatalakestore.net/` 格式。

### <a name="what-storage-is-the-cluster-using"></a>群集正在使用哪种存储

可以使用 Ambari 来检索群集的默认存储配置。 可以使用以下命令通过 curl 检索 HDFS 配置信息，并使用 [jq](https://stedolan.github.io/jq/)对其进行筛选：

```bash
curl -u admin -G "https://CLUSTERNAME.azurehdinsight.cn/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```
```

> [!NOTE]
> 此命令会返回应用到服务器的第一个配置 (`service_config_version=1`)，其中包含此信息。 可能需要列出所有配置版本，才能找到最新版本。

此命令返回类似于以下 URI 的值：

* `wasb://<container-name>@<account-name>.blob.core.chinacloudapi.cn` 。

    帐户名是 Azure 存储帐户的名称。 容器名称是作为群集存储的根的 blob 容器。

也可以在 Azure 门户中使用以下步骤查找存储信息：

1. 在 [Azure 门户](https://portal.azure.cn/)中，选择 HDInsight 群集。

2. 在“属性”部分中，选择“存储帐户”。 会显示群集的存储信息。

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>如何从 HDInsight 外部访问文件

从 HDInsight 群集外部访问数据的方法有多种。 以下是一些可用于处理数据的实用工具和 SDK 的链接：

如果使用的是 Azure Blob 存储，请参阅以下链接了解可用于访问数据的方式：

* [Azure CLI](/cli/install-az-cli2)：适用于 Azure 的命令行接口命令。 在安装后，使用 `az storage` 命令获取有关使用存储的帮助，或者使用 `az storage blob` 获取特定于 Blob 的命令。
* [blobxfer.py](https://github.com/Azure/blobxfer)：用于处理 Azure 存储中的 blob 的 python 脚本。
* 多种 SDK：

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [存储 REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

## <a name="scaling-your-cluster"></a><a name="scaling"></a>缩放你的群集

使用群集缩放功能可动态更改群集使用的数据节点数。 可以在其他作业或进程在群集上运行时执行缩放操作。  请参阅[缩放 HDInsight 群集](./hdinsight-scaling-best-practices.md)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>如何安装 Hue（或其他 Hadoop 组件）？

HDInsight 是托管服务。 如果 Azure 检测到群集存在问题，则可能会删除故障节点，再创建一个节点来代替。 在群集上手动安装节点时，这些节点在发生此操作时不会保留。 应该改用 [HDInsight 脚本操作](hdinsight-hadoop-customize-cluster-linux.md)。 脚本操作可用于进行以下更改：

* 安装并配置服务或网站。
* 安装和配置需要在群集的多个节点上进行配置更改的组件。

脚本操作是 Bash 脚本。 该脚本在群集创建期间运行，用于安装并配置其他组件。 有关开发自己的脚本操作的信息，请参阅[使用 HDInsight 进行脚本操作开发](hdinsight-hadoop-script-actions-linux.md)。

### <a name="jar-files"></a>Jar 文件

一些 Hadoop 方法提供独立的 jar 文件。 这些文件包含作为 MapReduce 作业的一部分使用的函数，或包含 Pig 或 Hive 内部的函数。 它们通常不需要进行任何设置，并可以在创建后上传到群集和直接使用。 如需确保组件在群集重置映像后仍存在，请将 jar 文件存储在群集的默认存储中。

例如，如果要使用 [Apache DataFu](https://datafu.incubator.apache.org/) 的最新版本，可以下载包含项目的 jar，并将其上传到 HDInsight 群集。 然后按照 DataFu 文档的说明通过 Pig 或 Hive 使用它。

> [!IMPORTANT]
> 某些属于独立 jar 文件的组件通过 HDInsight 提供，但不在路径中。 若要查找特定组件，可使用以下命令在群集上搜索：
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> 此命令会返回任何匹配的 jar 文件的路径。

要使用不同版本的组件，请上传所需版本，并在作业中使用它。

> [!IMPORTANT]
> 完全支持通过 HDInsight 群集提供的组件，Azure 支持部门帮助找出并解决与这些组件相关的问题。
>
> 自定义组件可获得合理范围的支持，有助于进一步解决问题。 这可能会促进解决问题，或要求使用可用的开源技术渠道，在渠道中可找到该技术的深厚的专业知识。 有许多可以使用的社区站点，例如：[面向 HDInsight 的 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight)、[Azure CSDN](http://azure.csdn.net)。 此外，Apache 项目在 [https://apache.org](https://apache.org) 上提供了项目站点，例如：[Hadoop](https://hadoop.apache.org/)、[Spark](https://spark.apache.org/)。

## <a name="next-steps"></a>后续步骤

* [使用 Apache Ambari REST API 管理 HDInsight 群集](./hdinsight-hadoop-manage-ambari-rest-api.md)
* [将 Apache Hive 和 HDInsight 配合使用](hadoop/hdinsight-use-hive.md)
* [将 MapReduce 作业与 HDInsight 配合使用](hadoop/hdinsight-use-mapreduce.md)
