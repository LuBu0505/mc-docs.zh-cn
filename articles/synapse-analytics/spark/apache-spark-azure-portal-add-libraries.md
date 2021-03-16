---
title: 管理 Apache Spark 使用的库
description: 了解如何在 Azure Synapse Analytics 中添加和管理 Apache Spark 使用的库。
services: synapse-analytics
author: WenJason
ms.service: synapse-analytics
ms.topic: conceptual
origin.date: 10/16/2020
ms.date: 03/08/2021
ms.author: v-jay
ms.reviewer: jrasnick
ms.subservice: spark
ms.openlocfilehash: 4f0cae9c3c360f93e1c245f7d2f6a9f8fdcb1f68
ms.sourcegitcommit: 5707919d0754df9dd9543a6d8e6525774af738a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102207176"
---
# <a name="manage-libraries-for-apache-spark-in-azure-synapse-analytics"></a>在 Azure Synapse Analytics 中管理 Apache Spark 使用的库

这些库提供了可重用的代码，你可能想要在程序或项目中包含这些代码。 要使第三方或本地生成的代码可用于你的应用程序，可将库安装到某个无服务器 Apache Spark 池上。 为 Spark 池安装库后，它就可用于使用同一池的所有会话。 

## <a name="before-you-begin"></a>在开始之前
- 若要安装和更新库，必须在关联到 Azure Synapse Analytics 工作区的主 Gen2 存储帐户上拥有“存储 Blob 数据参与者”或“存储 Blob 数据所有者”权限 。
  
## <a name="default-installation"></a>默认安装
Azure Synapse Analytics 中的 Apache Spark 具有完整的 Anacondas 安装和额外的库。 你可在 [Apache Spark 版本支持](apache-spark-version-support.md)中找到完整的库列表。 

当 Spark 实例启动时，将自动包含这些库。 在 Spark 池级别可以添加额外的 Python 和自定义包。


## <a name="manage-python-packages"></a>管理 Python 包
确定要用于 Spark 应用程序的库后，就可将它们安装到 Spark 池中。 

 可使用 requirements.txt 文件（`pip freeze` 的输出）升级虚拟环境。 当池启动时，将从 PyPI 下载此文件中列出的要安装或升级的包。 每当通过该 Spark 池创建 Spark 实例时，都会使用此要求文件。

> [!IMPORTANT]
> - 如果要安装的包很大，或者需要很长时间才能完成安装，则会影响 Spark 实例的启动时间。
> - 不支持在安装时需要编译器支持的包（例如 GCC）。
> - 包不能降级，而只能添加或升级。
> - 不支持更改 PySpark、Python、Scala/Java、.NET 或 Spark 版本。
> - 在启用 DEP 的工作区中不支持从 PyPI 安装包。


### <a name="requirements-format"></a>要求格式

以下代码片段显示了要求文件的格式。 PyPi 包名称将与具体的版本一起列出。 此文件遵循 [pip freeze](https://pip.pypa.io/en/stable/reference/pip_freeze/) 参考文档中所述的格式。 此示例固定使用一个特定版本。 

```
absl-py==0.7.0
adal==1.2.1
alabaster==0.7.10
```

### <a name="install-python-packages"></a>安装 Python 包
开发 Spark 应用程序时，你可能会发现需要更新现有库或安装新库。 在创建池期间或创建后，可对库进行更新。

> [!IMPORTANT]
> 若要安装库，必须在关联到 Synapse 工作区的主 Gen2 存储帐户上具有“存储 Blob 数据参与者”或“存储 Blob 数据所有者”权限。

#### <a name="install-packages-during-pool-creation"></a>在创建池期间安装包
在创建池期间将库安装到 Spark 池：
   
1. 从 Azure 门户导航到 Azure Synapse Analytics 工作区。
   
2. 选择“创建 Apache Spark 池”，然后选择“其他设置”选项卡 。 
   
3. 使用该页面“包”部分中的文件选择器上传环境配置文件。 
   
    ![在创建池期间添加 Python 库](./media/apache-spark-azure-portal-add-libraries/apache-spark-azure-portal-add-library-python.png "添加 Python 库")
 

#### <a name="install-packages-from-the-synapse-workspace"></a>从 Synapse 工作区安装包
从 Azure Synapse Analytics 门户更新库或将更多库添加到 Spark 池：

1.  从 Azure 门户导航到 Azure Synapse Analytics 工作区。
   
2.  从 Azure 门户启动 Azure Synapse Analytics 工作区。

3.  从主导航面板中选择“管理”，然后选择“Apache Spark 池” 。
   
4. 选择一个 Spark 池，使用该页面“包”部分中的文件选择器上传环境配置文件。

    ![将 Python 库添加到 Synapse 中](./media/apache-spark-azure-portal-add-libraries/apache-spark-azure-portal-update.png)
   
#### <a name="install-packages-from-the-azure-portal"></a>从 Azure 门户安装包
直接从 Azure 门户将库安装到 Spark 池：
   
 1. 从 Azure 门户导航到 Azure Synapse Analytics 工作区。
   
 2. 在“Synapse 资源”部分下，选择“Apache Spark 池”选项卡，然后从列表中选择一个 Spark 池 。
   
 3. 从 Spark 池的“设置”部分选择“包” 。 

 4. 使用文件选择器上传环境配置文件。

    ![突出显示“上传环境配置文件”按钮的屏幕截图。](./media/apache-spark-azure-portal-add-libraries/apache-spark-add-library-azure.png "添加 Python 库")

### <a name="verify-installed-libraries"></a>验证已安装的库

若要验证是否安装了正确的库版本，请运行以下代码

```python
import pkg_resources
for d in pkg_resources.working_set:
     print(d)
```
### <a name="update-python-packages"></a>更新 Python 包
你可以在会话之间随时添加或修改包。 新的包配置文件将覆盖现有的包和版本。  

更新或卸载库：
1. 导航到 Azure Synapse Analytics 工作区。 

2. 使用“Azure 门户”或“Azure Synapse”工作区，选择要更新的 Apache Spark 池。

3. 导航到“包”部分，上传新的环境配置文件
   
4. 保存更改后，需要结束活动会话并等待池重启。 或者，你可以选中“强制使用新设置”复选框，强制结束活动会话。

    ![添加 Python 库](./media/apache-spark-azure-portal-add-libraries/update-libraries.png "添加 Python 库")
   

> [!IMPORTANT]
> 通过选择“强制使用新设置”选项，将结束所选 Spark 池的所有当前会话。 会话结束后，需要等待池重启。 
>
> 如果未选中此设置，则需要等待当前 Spark 会话结束或手动将其停止。 会话结束后，需要重启池。 


## <a name="manage-a-python-wheel"></a>管理 Python Wheel

### <a name="install-a-custom-wheel-file"></a>安装自定义 Wheel 文件
通过将所有 wheel 文件上传到与 Synapse 工作区关联的 Azure Data Lake Storage (Gen2) 帐户，可以在 Apache Spark 池上安装自定义 wheel 包。 

应将这些文件上传到存储帐户默认容器中的以下路径： 

```
abfss://<file_system>@<account_name>.dfs.core.chinacloudapi.cn/synapse/workspaces/<workspace_name>/sparkpools/<pool_name>/libraries/python/
```

最好在 ```python``` 文件夹中添加文件夹 ```libraries```（如果该文件夹不存在）。

>[!IMPORTANT]
>你可以在会话之间添加或修改自定义包。 但需要等待池和会话重启才能看到更新的包。

## <a name="next-steps"></a>后续步骤
- 查看默认库：[Apache Spark 版本支持](apache-spark-version-support.md)
