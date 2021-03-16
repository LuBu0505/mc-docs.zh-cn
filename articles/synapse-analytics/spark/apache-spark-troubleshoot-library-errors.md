---
title: 排查库安装错误
description: 本教程概述了如何排查库安装错误。
services: synapse-analytics
author: WenJason
ms.author: v-jay
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: conceptual
origin.date: 01/04/2021
ms.date: 03/08/2021
ms.openlocfilehash: b5e8aca8e49e55f7070b0491800be02e25db417a
ms.sourcegitcommit: 5707919d0754df9dd9543a6d8e6525774af738a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102207164"
---
# <a name="troubleshoot-library-installation-errors"></a>排查库安装错误 
要使第三方或本地生成的代码可用于你的应用程序，可将库安装到某个无服务器 Apache Spark 池上。 当池启动时，将从 PyPi 下载 requirements.txt 文件中列出的包。 每当通过该 Spark 池创建 Spark 实例时，都会使用此要求文件。 为 Spark 池安装库后，它就可用于使用同一池的所有会话。 

在某些情况下，你可能会发现你尝试安装的库未显示在 Apache Spark 池中。 当提供的 requirements.txt 或指定的库中存在错误时，通常会发生这种情况。 当库安装过程中出现错误时，Apache Spark 池会恢复为 Synapse 基本运行时中指定的库。

本文档的目的是提供常见问题并帮助你调试库安装错误。

## <a name="force-update-your-apache-spark-pool"></a>强制更新 Apache Spark 池
更新 Apache Spark 池中的库时，这些更改会在重新启动池后恢复原样。 如果有活动的作业，这些作业将继续在 spark 池的原始版本上运行。

可以通过选择“强制使用新设置”选项来强制应用更改。 此设置将结束所选 Spark 池的所有当前会话。 会话结束后，需要等待池重启。 

![添加 Python 库](./media/apache-spark-azure-portal-add-libraries/update-libraries.png "添加 Python 库")

## <a name="validate-your-permissions"></a>验证你的权限
若要安装和更新库，必须在关联到 Azure Synapse Analytics 工作区的主 Azure Data Lake Storage Gen2 存储帐户上拥有“存储 Blob 数据参与者”或“存储 Blob 数据所有者”权限 。

若要验证是否具有这些权限，可以运行以下代码：

```python
from pyspark.sql.types import StructType,StructField, StringType, IntegerType
data2 = [("James","Smith","Joe","4355","M",3000),
    ("Michael","Rose","Edward","40288","F",4000)
  ]

schema = StructType([ \
    StructField("firstname",StringType(),True), \
    StructField("middlename",StringType(),True), \
    StructField("lastname",StringType(),True), \
    StructField("id", StringType(), True), \
    StructField("gender", StringType(), True), \
    StructField("salary", IntegerType(), True) \
  ])
 
df = spark.createDataFrame(data=data2,schema=schema)

df.write.csv("abfss://<<ENTER NAME OF FILE SYSTEM>>@<<ENTER NAME OF PRIMARY STORAGE ACCOUNT>>.dfs.core.chinacloudapi.cn/validate_permissions.csv")

```
如果收到错误，则你可能缺少所需的权限。 若要了解如何获取所需的权限，请访问此文档：[分配存储 Blob 数据参与者或存储 Blob 数据所有者权限](/storage/common/storage-auth-aad-rbac-portal#assign-an-azure-built-in-role)。

此外，如果你运行的是管道，则工作区 MSI 也必须具有存储 Blob 数据所有者或存储 Blob 数据参与者权限。 若要了解如何向工作区标识授予此权限，请访问：[向工作区托管标识授予权限](../security/how-to-grant-workspace-managed-identity-permissions.md)。

## <a name="check-the-requirements-file"></a>检查要求文件
可使用 requirements.txt 文件（pip freeze 命令的输出）升级虚拟环境。 此文件遵循 [pip freeze](https://pip.pypa.io/en/stable/reference/pip_freeze/) 参考文档中所述的格式。

务必注意以下限制：
   -  PyPI 包名称必须与具体的版本一起列出。 
   -  要求文件的内容不能包含额外的空白行或字符。 
   -  [Synapse 运行时](apache-spark-version-support.md)包含一组预先安装在每个无服务器 Apache Spark 池上的库。 不能降级预先安装到基本运行时上的包。 只能添加或升级包。
   -  不支持更改 PySpark、Python、Scala/Java、.NET 或 Spark 版本。

以下代码片段显示了要求文件的所需格式。

```
absl-py==0.7.0
adal==1.2.1
alabaster==0.7.10
```

## <a name="validate-wheel-files"></a>验证 wheel 文件
Synapse 无服务器 Apache Spark 池基于 Linux 分发。 直接从 PyPI 下载和安装 Wheel 文件时，请确保选择基于 Linux 构建的版本，并在与 Spark 池相同的 Python 版本上运行。

>[!IMPORTANT]
>你可以在会话之间添加或修改自定义包。 但需要等待池和会话重启才能看到更新的包。

## <a name="check-for-dependency-conflicts"></a>检查依赖关系冲突
 一般来说，Python 依赖关系的解析很难管理。 为了帮助在本地调试依赖关系冲突，你可以创建自己的基于 Synapse 运行时的虚拟环境并验证所做的更改。

若要重新创建环境并验证更新，请进行以下操作：
 1. [下载](https://github.com/Azure-Samples/Synapse/blob/main/Spark/Python/base_environment.yml)模板以在本地重新创建 Synapse 运行时。 模板与实际的 Synapse 环境之间可能存在细微的差异。
   
 2. 按照[以下说明](https://docs.conda.io/projects/conda/latest/user-guide/tasks/manage-environments.html)创建虚拟环境。 此环境让你能够使用指定的库列表创建独立的 Python 安装。 
    
    ```
    conda myenv create -f environment.yml
    conda activate myenv
    ```
   
 3. 通过 ``pip install -r <provide your req.txt file>`` 使用指定的包更新虚拟环境。 如果安装导致错误，则 Synapse 基本运行时中预安装的内容与提供的要求文件中指定的内容之间可能存在冲突。 必须解决这些依赖关系冲突，才能在无服务器 Apache Spark 池上获取更新后的库。

## <a name="next-steps"></a>后续步骤
- 查看默认库：[Apache Spark 版本支持](apache-spark-version-support.md)

