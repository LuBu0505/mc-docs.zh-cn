---
title: 快速入门 - 使用 Azure 门户在 Azure Databricks 工作区上运行 Spark 作业
description: 本快速入门介绍如何使用 Azure 门户创建 Azure Databricks 工作区和 Apache Spark 群集，以及如何运行 Spark 作业。
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.date: 03/23/2020
ms.custom: mvc, tracking-python
ms.openlocfilehash: 6c66fddb910232af5ef535bf35a98994c1a3d57e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059865"
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-workspace-using-the-azure-portal"></a>快速入门：使用 Azure 门户在 Azure Databricks 工作区上运行 Spark 作业

在本快速入门中，将使用 Azure 门户创建一个具有 Apache Spark 群集的 Azure Databricks 工作区。 你在群集上运行作业，并使用自定义图表根据波士顿安全数据生成实时报告。

## <a name="prerequisites"></a>先决条件

#### <a name="portal"></a>[Portal](#tab/azure-portal)

- Azure 订阅 - [创建一个](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。 不能使用 Azure 免费试用订阅完成本教程。 如果你有免费帐户，请转到个人资料并将订阅更改为“即用即付”。 有关详细信息，请参阅 [Azure 免费帐户](https://azure.microsoft.com/free/)。 然后，[移除支出限制](/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)，并为你所在区域的 vCPU [请求增加配额](/azure-portal/supportability/resource-manager-core-quotas-request)。 创建 Azure Databricks 工作区时，可以选择“试用版(高级 - 14天免费 DBU)”定价层，让工作区访问免费的高级 Azure Databricks DBU 14 天。

- 登录 [Azure 门户](https://portal.azure.com)。

#### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

如果还没有 Azure 订阅，请[创建一个](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。 不能使用 Azure 免费试用订阅完成本教程。 如果你有免费帐户，请转到个人资料并将订阅更改为“即用即付”。 有关详细信息，请参阅 [Azure 免费帐户](https://azure.microsoft.com/free/)。 然后，[移除支出限制](/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)，并为你所在区域的 vCPU [请求增加配额](/azure-portal/supportability/resource-manager-core-quotas-request)。 创建 Azure Databricks 工作区时，可以选择“试用版(高级 - 14天免费 DBU)”定价层，让工作区访问免费的高级 Azure Databricks DBU 14 天。


## <a name="create-an-azure-databricks-workspace"></a>创建 Azure Databricks 工作区

在本部分，你将使用 Azure 门户或 Azure CLI 创建 Azure Databricks 工作区。

#### <a name="portal"></a>[门户](#tab/azure-portal)

1. 在 Azure 门户中，选择“创建资源” > “分析” > “Azure Databricks”。

    ![Azure 门户上的 Databricks](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Azure 门户上的 Databricks")

2. 在“Azure Databricks 服务”  下，提供所需的值以创建 Databricks 工作区。

    ![创建 Azure Databricks 工作区](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "创建 Azure Databricks 工作区")

    提供以下值：
    
    |properties  |说明  |
    |---------|---------|
    |**工作区名称**     | 提供 Databricks 工作区的名称        |
    |**订阅**     | 从下拉列表中选择自己的 Azure 订阅。        |
    |**资源组**     | 指定是要创建新的资源组还是使用现有的资源组。 资源组是用于保存 Azure 解决方案相关资源的容器。 有关详细信息，请参阅 [Azure 资源组概述](/azure-resource-manager/management/overview)。 |
    |**位置**     | 选择“美国西部 2”  。 有关其他可用区域，请参阅[各区域推出的 Azure 服务](https://azure.microsoft.com/regions/services/)。        |
    |**定价层**     |  在“标准”、“高级”和“试用”之间进行选择。   有关这些层的详细信息，请参阅 [Databricks 价格页](https://azure.microsoft.com/pricing/details/databricks/)。       |

3. 选择“查看 + 创建”，然后选择“创建” 。 创建工作区需要几分钟时间。 创建工作区时，可以在“通知”中查看部署状态。 完成此过程后，你的用户帐户将自动添加为工作区的管理员用户。

    ![Databricks 部署磁贴](./media/quickstart-create-databricks-workspace-portal/databricks-deployment-tile.png "Databricks 部署磁贴")

    当工作区部署失败时，仍然会在失败状态下创建工作区。 删除失败的工作区，并创建一个解决部署错误的新工作区。 删除失败的工作区时，托管资源组和任何成功部署的资源也将被删除。

#### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="prepare-your-environment"></a>准备环境

1. 登录。

   如果使用的是 CLI 的本地安装，请使用 [az login](/cli/azure/reference-index#az-login) 命令登录。

    ```azurecli
    az login
    ```

    遵循终端中显示的步骤完成身份验证过程。

2. 安装 Azure CLI 扩展。

   使用 Azure CLI 的扩展引用时，必须先安装该扩展。  借助 Azure CLI 扩展，可访问尚未在核心 CLI 中提供的试验性和预发布的命令。  若要详细了解包含更新和卸载的扩展，请参阅[使用 Azure CLI 的扩展](/cli/azure/azure-cli-extensions-overview)。

   运行以下命令，安装 [Databricks 扩展](/cli/ext/databricks/databricks)：

    ```azurecli
    az extension add --name databricks
   ```
3. 创建资源组。

   与所有 Azure 资源一样，Azure Databricks 也必须部署到资源组中。 使用资源组可以组织和管理相关的 Azure 资源。

   对于本快速入门，请使用以下 [az group create](/cli/azure/group#az-group-create) 命令在 _westus2_ 位置创建名为 _ databricks-quickstart _ 的资源组：

   ```azurecli
   az group create --name databricks-quickstart --location westus2
   ```

### <a name="create-an-azure-databricks-workspace"></a>创建 Azure Databricks 工作区

使用 [az databricks workspace create](/cli/ext/databricks/databricks/workspace?view=azure-cli-latest) 创建 Azure Databricks 工作区。

```azurecli
az databricks workspace create 
    --resource-group databricks-quickstart \
    --name mydatabricksws  \
    --location westus  \
    --sku standard
```

---

## <a name="create-a-spark-cluster-in-databricks"></a>在 Databricks 中创建 Spark 群集

> [!NOTE]
> 若要使用免费帐户创建 Azure Databricks 群集，请在创建群集前转到你的配置文件并将订阅更改为 **即用即付**。 有关详细信息，请参阅 [Azure 免费帐户](https://azure.microsoft.com/free/)。

1. 在 Azure 门户中，转到所创建的 Databricks 工作区，然后单击“启动工作区”。

2. 随后将会重定向到 Azure Databricks 门户。 在该门户中，单击“新建群集”。

    ![Azure 上的 Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Azure 上的 Databricks")

3. 在“新建群集”页中，提供用于创建群集的值。 

    ![在 Azure 上创建 Databricks Spark 群集](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "在 Azure 上创建 Databricks Spark 群集")

    除以下值外，接受其他所有默认值：

   * 输入群集的名称。
   * 对于本文，请通过（5.X、6.X、7.X）运行时创建群集。  
   * 请务必选中 **在不活动超过 \_\_ 分钟后终止** 复选框。 提供一个持续时间（以分钟为单位），如果群集在这段时间内一直未被使用，则会将其终止。
    
     选择“创建群集”。  群集运行后，可将笔记本附加到该群集，并运行 Spark 作业。

有关创建群集的详细信息，请参阅[在 Azure Databricks 中创建 Spark 群集](/databricks/clusters/create)。

## <a name="run-a-spark-sql-job"></a>运行 Spark SQL 作业

执行以下任务在 Databricks 中创建笔记本、将该笔记本配置为从 Azure 开放数据集读取数据，然后针对这些数据运行 Spark SQL 作业。

1. 在左窗格中选择“Azure Databricks”。 在“常见任务”中，选择“新建笔记本”。 

    ![在 Databricks 中创建笔记本](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "在 Databricks 中创建笔记本")

2. 在“创建笔记本”对话框中输入一个名称，选择“Python”作为语言，并选择前面创建的 Spark 群集。 

    ![在 Databricks 中创建笔记本](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "在 Databricks 中创建笔记本")

    选择“创建”。

3. 在此步骤中，请使用 [Azure 开放数据集](https://azure.microsoft.com/services/open-datasets/catalog/boston-safety-data/#AzureDatabricks)中的波士顿安全数据创建 Spark DataFrame，并使用 SQL 来查询数据。

   以下命令设置 Azure 存储访问信息。 将此 PySpark 代码粘贴到第一个单元格中，然后使用 **Shift+Enter** 来运行代码。

   ```python
   blob_account_name = "azureopendatastorage"
   blob_container_name = "citydatacontainer"
   blob_relative_path = "Safety/Release/city=Boston"
   blob_sas_token = r"?st=2019-02-26T02%3A34%3A32Z&se=2119-02-27T02%3A34%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=XlJVWA7fMXCSxCKqJm8psMOh0W4h7cSYO28coRqF2fs%3D"
   ```

   以下命令允许 Spark 以远程方式从 Blob 存储读取数据。 将此 PySpark 代码粘贴到下一个单元格中，然后使用 **Shift+Enter** 来运行代码。

   ```python
   wasbs_path = 'wasbs://%s@%s.blob.core.chinacloudapi.cn/%s' % (blob_container_name, blob_account_name, blob_relative_path)
   spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)
   print('Remote blob path: ' + wasbs_path)
   ```

   以下命令创建 DataFrame。 将此 PySpark 代码粘贴到下一个单元格中，然后使用 **Shift+Enter** 来运行代码。

   ```python
   df = spark.read.parquet(wasbs_path)
   print('Register the DataFrame as a SQL temporary view: source')
   df.createOrReplaceTempView('source')
   ```

4. 运行一个 SQL 语句，从名为 **source** 的临时视图返回头 10 行数据。 将此 PySpark 代码粘贴到下一个单元格中，然后使用 **Shift+Enter** 来运行代码。

   ```python
   print('Displaying top 10 rows: ')
   display(spark.sql('SELECT * FROM source LIMIT 10'))
   ```

5. 随后将会看到以下屏幕截图中所示的表格输出（此处只显示了一部分列）：

    ![示例数据](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "示例 JSON 数据")

6. 现在创建此数据的视觉表示形式，用于说明使用 Citizens Connect App 和 City Worker App 而非其他源报告了多少安全事件。 在表格输出的底部，选择“条形图”图标，然后单击“绘图选项”。 

    ![创建条形图](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "创建条形图")

8. 在“自定义绘图”中，按屏幕截图中所示拖放值。

    ![自定义饼图](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "自定义条形图")

   * 将“键”设置为“source”。 
   * 将“值”设置为“<\id>”。 
   * 将“聚合”设置为“计数”。 
   * 将“显示类型”设置为“饼图”。 

     单击“应用”。

## <a name="clean-up-resources"></a>清理资源

完成本文后，可以终止群集。 为此，请在 Azure Databricks 工作区的左窗格中选择“群集”  。 针对想要终止的群集，将光标移到“操作”  列下面的省略号上，选择“终止”  图标。

![停止 Databricks 群集](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "停止 Databricks 群集")

如果不手动终止群集，但在创建群集时选中了“在不活动 \_\_ 分钟后终止”  复选框，则该群集会自动停止。 在这种情况下，如果群集保持非活动状态超过指定的时间，则会自动停止。

## <a name="next-steps"></a>后续步骤

在本文中，我们已在 Azure Databricks 中创建一个 Spark 群集，并使用 Azure 开放数据集中的数据运行了一个 Spark 作业。 我们还可以查看 [Spark 数据源](/databricks/data/data-sources/index)，了解如何将其他数据源中的数据导入 Azure Databricks。 请继续学习下一篇文章，了解如何使用 Azure Databricks 执行 ETL（提取、转换和加载数据）操作。

> [!div class="nextstepaction"]
>[使用 Azure Databricks 提取、转换和加载数据](databricks-extract-load-sql-data-warehouse.md)
