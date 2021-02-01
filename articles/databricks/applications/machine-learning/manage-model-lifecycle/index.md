---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/18/2020
title: 管理模型 - Azure Databricks
description: 了解如何在模型注册表中管理 MLflow 模型的生命周期。
ms.openlocfilehash: 8d4ec177378f99321216ca58e8bccde89f08bfbf
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061107"
---
# <a name="manage-models"></a>管理模型

Azure Databricks 提供 MLflow 模型注册表的托管版本，用于帮助用户管理 MLflow 模型的完整生命周期。 模型注册表提供按时间顺序记录的模型世系（MLflow 试验和运行在给定时间生成模型）、模型版本控制、阶段转换（例如从“暂存”到“生产”或“已存档”）以及模型、模型版本注释和说明。

本文介绍如何在机器学习工作流中使用模型注册表，并提供有关模型注册表 UI 和模型注册表 API 的说明。

有关模型注册表概念的概述性介绍，请参阅 [MLflow 指南](../../mlflow/index.md)。

## <a name="in-this-section"></a>本节内容：

* [惠?](#requirements)
* [创建或注册模型](#create-or-register-a-model)
* [控制对模型的访问](#control-access-to-models)
* [转换模型阶段](#transition-a-model-stage)
* [对模型或模型版本进行批注](#annotate-a-model-or-model-version)
* [重命名模型（仅 API）](#rename-a-model-api-only)
* [搜索模型](#search-for-a-model)
* [删除模型或模型版本](#delete-a-model-or-model-version)
* [跨工作区共享模型](#share-models-across-workspaces)
* [示例](#example)

## <a name="requirements"></a>要求

使用模型注册表 UI：

* Databricks Runtime 6.4 或更高版本，并且已安装 MLflow 1.7.0 或更高版本
* Databricks Runtime 6.4 ML，并且已安装 MLflow 1.7.0 或更高版本
* Databricks Runtime 6.5 ML 或更高版本

使用模型注册表 API：

* Databricks Runtime 6.5 或更高版本
* Databricks Runtime 6.5 ML 或更高版本
* Databricks Runtime 6.4 或更低版本，并且已安装 MLflow 1.7.0 或更高版本
* Databricks Runtime 6.4 ML，并且已安装 MLflow 1.7.0 或更高版本

## <a name="create-or-register-a-model"></a>创建或注册模型

### <a name="in-this-section"></a>本节内容：

* [使用 UI 创建或注册模型](#create-or-register-a-model-using-the-ui)
* [使用 API 注册模型](#register-a-model-using-the-api)

### <a name="create-or-register-a-model-using-the-ui"></a>使用 UI 创建或注册模型

可以通过两种方式在模型注册表中注册模型。 可以注册已记录到 MLflow 的现有模型，也可以创建并注册一个新的空模型，然后将某个已记录的模型分配给它。

#### <a name="register-an-existing-logged-model-from-a-notebook"></a>从笔记本中注册现有的已记录模型

1. 在工作区中，标识包含要注册的模型的 MLflow 运行。
   1. 单击笔记本工具栏中的“试验”图标![试验图标](../../../_static/images/icons/experiment.png)。

      > [!div class="mx-imgBorder"]
      > ![笔记本工具栏](../../../_static/images/mlflow/notebook-toolbar.png)

   1. 在“试验运行”边栏中单击 ![外部链接](../../../_static/images/icons/external-link.png) 图标，该图标位于运行日期旁。 MLflow 运行页面随即显示。 此页面显示运行详细信息，包括参数、指标、标记和项目列表。
2. 在“项目”部分，单击名为“xxx-model”的目录。

   > [!div class="mx-imgBorder"]
   > ![注册模型](../../../_static/images/mlflow/register-model.png)

3. 单击最右侧的“注册模型”按钮。
4. 在“注册模型”对话框中，从“模型”下拉菜单中选择“新建模型”，然后在“模型名称”字段中输入模型名称，例如 ``scikit-learn-power-forecasting``。

   > [!div class="mx-imgBorder"]
   > ![创建模型](../../../_static/images/mlflow/create-model.png)

5. 单击“注册”。 这将注册一个名为 ``scikit-learn-power-forecasting`` 的模型，将该模型复制到由 MLflow 模型注册表管理的安全位置，并创建该模型的新版本。

   几分钟后，MLflow 运行 UI 会将“注册模型”按钮替换为指向新注册的模型版本的链接。

   > [!div class="mx-imgBorder"]
   > ![创建模型](../../../_static/images/mlflow/registered-model-version.png)

6. 单击此链接可在模型注册表 UI 中打开新的模型版本。 还可以单击 ![左侧导航栏中的](../../../_static/images/icons/models-icon.png) “模型图标”图标，在“模型注册表”中找到该模型。

#### <a name="create-a-new-registered-model-and-assign-a-logged-model-to-it"></a>创建新注册的模型并向其分配已记录的模型

可以使用已注册的模型页上的“创建模型”按钮创建一个新的空模型，然后为其分配一个已记录的模型。 请执行下列步骤：

1. 在“已注册的模型”页上，单击“创建模型”。 为该模型输入一个名称，然后单击“创建”。
2. 请按[从笔记本中注册现有的已记录模型](#register-an-existing-logged-model-from-a-notebook)中的步骤 1 到 3 操作。
3. 在“注册模型”对话框中，选择在步骤 1 中创建的模型的名称，然后单击“注册”。 该操作的结果是采用之前创建的名称注册一个模型，将该模型复制到由 MLflow 模型注册表管理的安全位置，并创建模型版本：``Version 1``。

   几分钟后，MLflow 运行 UI 会将“注册模型”按钮替换为指向新注册的模型版本的链接。 现可从“试验运行”页上的“注册模型”对话框中的“模型”下拉列表中选择模型 。 还可以通过在 API 命令（如 [Create ModelVersion](https://mlflow.org/docs/latest/rest-api.html#create-modelversion)）中指定模型名称来注册模型的新版本。

### <a name="register-a-model-using-the-api"></a>使用 API 注册模型

可以通过三种编程式方法在模型注册表中注册模型。 所有方法都将模型复制到由 MLflow 模型注册表管理的安全位置。

* 若要记录模型并在 MLflow 试验期间将其注册为指定名称，请使用 ``mlflow.<model-flavor>.log_model(...)`` 方法。 如果还没有模型注册为该名称，该方法将注册一个新模型，创建版本 1，并返回 ``ModelVersion`` MLflow 对象。 如果已有模型注册为该名称，该方法将创建一个新的模型版本并返回版本对象。

  ```python
  with mlflow.start_run(run_name=<run-name>) as run:
    ...
    mlflow.<model-flavor>.log_model(<model-flavor>=<model>,
      artifact_path="<model-path>",
      registered_model_name="<model-name>"
    )
  ```

* 若要在所有试验运行完成后使用指定名称注册模型，并且已确定最适合添加到注册表的模型，请使用 ``mlflow.register_model()`` 方法。 采用此方法时，需要 ``mlruns:URI`` 参数的运行 ID。 如果还没有模型注册为该名称，该方法将注册一个新模型，创建版本 1，并返回 ``ModelVersion`` MLflow 对象。 如果已有模型注册为该名称，该方法将创建一个新的模型版本并返回版本对象。

  ```python
  result=mlflow.register_model("runs:<model-path>", "<model-name>")
  ```

* 若要创建具有指定名称的新的注册模型，请使用 MLflow 客户端 API ``create_registered_model()`` 方法。 如果模型名称存在，此方法将引发 ``MLflowException``。

  ```python
  client = MlflowClient()
  result = client.create_registered_model("<model-name>")
  ```

## <a name="control-access-to-models"></a>控制对模型的访问

若要了解如何控制对模型注册表中模型的访问，请参阅 [MLflow 模型权限](../../../security/access-control/workspace-acl.md#mlflow-model-permissions)。

## <a name="transition-a-model-stage"></a>转换模型阶段

模型版本具有以下阶段之一：“无”、“暂存”、“生产”或“已存档”   。 “暂存”阶段指模型处于测试和验证阶段，“生产”阶段指模型版本已完成测试或审核流程，并已部署到应用程序，正在获取实时评分 。 “已存档”阶段的模型版本视为非活动状态，可以考虑[将其删除](#delete-a-model-or-model-version)。 不同的模型版本可以处于不同的阶段。

具有适当[权限](../../../security/access-control/workspace-acl.md#mlflow-model-permissions)的用户可以在不同阶段之间转换模型版本。 如果你有权将模型版本转换到某个特定阶段，可以直接进行转换。 如果你没有相应权限，可以请求阶段转换，有权转换模型版本的用户可以[批准、拒绝或取消该请求](#str)。

### <a name="in-this-section"></a>本节内容：

* [使用 UI 转换模型阶段](#transition-a-model-stage-using-the-ui)
* [使用 API 转换模型阶段](#transition-a-model-stage-using-the-api)

### <a name="transition-a-model-stage-using-the-ui"></a>使用 UI 转换模型阶段

请按照这些说明转换模型的阶段。

1. 若要显示可用模型阶段和可用选项的列表，请在“模型版本”页中单击“阶段: <Stage>”按钮，然后请求或选择转换到另一阶段。

   > [!div class="mx-imgBorder"]
   > ![阶段转换选项](../../../_static/images/mlflow/stage-options.png)

2. 输入可选的注释并单击“确定”。

#### <a name="transition-a-model-version-to-the-production-stage"></a>将模型版本转换为“生产”阶段

完成测试和验证流程后，可以转换或者请求转换至“生产”阶段。

模型注册表允许每个阶段中存在已注册模型的多个版本。 若希望“生产”阶段中只有一个版本，请单击“将现有‘生产’模型版本转换至‘已存档’阶段”，这样即可将当前处于“生产”阶段的所有模型版本都转换至“已存档”阶段。

#### <a name="approve-reject-or-cancel-a-model-version-stage-transition-request"></a><a id="approve-reject-or-cancel-a-model-version-stage-transition-request"> </a><a id="str"> </a>批准、拒绝或取消模型版本阶段转换请求

无权转换阶段的用户可以发起阶段转换请求。 请求显示在模型版本页的“待定的请求”部分中：

> [!div class="mx-imgBorder"]
> ![转换至“生产”](../../../_static/images/mlflow/handle-transition-request.png)

若要批准、拒绝或取消阶段转换请求，请单击“批准”、“拒绝”或“取消”链接  。

转换请求的创建者也可以取消请求。

#### <a name="view-model-version-activities"></a>查看模型版本活动

若要查看请求、批准、待定和应用于模型版本的所有转换，请转到“活动”部分。 此处的活动记录包含模型生命周期的世系，可供审核或检查。

### <a name="transition-a-model-stage-using-the-api"></a>使用 API 转换模型阶段

具有适当[权限](../../../security/access-control/workspace-acl.md#mlflow-model-permissions)的用户可以将模型版本转换至新的阶段。

若要将模型版本阶段更新至新阶段，请使用 MLflow 客户端 API ``transition_model_version_stage()`` 方法：

```python
  client = MlflowClient()
  client.transition_model_version_stage(
    name="<model-name>",
    version=<model-version>,
    stage="<stage>",
    description="<description>"
  )
```

``<stage>`` 可以为以下值：``"Staging"|"staging"``、``"Archived"|"archived"``、``"Production"|"production"`` 和 ``"None"|"none"``。

## <a name="annotate-a-model-or-model-version"></a>对模型或模型版本进行批注

可以通过对模型或模型版本进行批注来提供与其相关的信息。 例如，你可能想提供有关所用方法和算法的相关问题或信息的概述性介绍。

### <a name="annotate-a-model-or-model-version-using-the-ui"></a>使用 UI 对模型或模型版本进行批注

添加或更新模型或模型版本说明：

1. 在[已注册模型](../../mlflow/model-registry.md#registered-model-page)或[模型版本](../../mlflow/model-registry.md#model-version-page)页中单击“说明”![编辑图标](../../../_static/images/icons/edit-icon.png)图标。 随即会显示编辑窗口。
2. 在编辑窗口中输入或编辑说明。
3. 单击“保存”  。

   输入的模型版本说明将显示在[已注册模型页](../../mlflow/model-registry.md#registered-model-page)上的表的“说明”列中。 该列最多可显示 32 个字符或一行文本，以较短者为准。

### <a name="annotate-a-model-version-using-the-api"></a>使用 API 对模型版本进行批注

若要更新模型版本说明，请使用 MLflow 客户端 API ``update_model_version()`` 方法：

```python
client = MlflowClient()
client.update_model_version(
  name="<model-name>",
  version=<model-version>,
  description="<description>"
)
```

## <a name="rename-a-model-api-only"></a>重命名模型（仅 API）

若要重命名已注册模型，请使用 MLflow 客户端 API ``rename_registered_model()`` 方法：

```python
client=MlflowClient()
client.rename_registered_model("<model-name>", "<new-model-name>")
```

> [!NOTE]
>
> 仅当已注册模型没有任何版本或所有版本都处于“无”或“已存档”阶段时，才能重命名该模型。

## <a name="search-for-a-model"></a>搜索模型

MLflow 模型注册表中包含了所有已注册的模型。 可以使用 UI 或 API 搜索模型。

### <a name="search-for-a-model-using-the-ui"></a>使用 UI 搜索模型

若要显示所有已注册的模型，请单击 ![侧栏中的](../../../_static/images/icons/models-icon.png) “模型”图标。

若要搜索特定模型，请在搜索框中键入模型名称。

> [!div class="mx-imgBorder"]
> ![搜索已注册模型](../../../_static/images/mlflow/registered-models-search.png)

### <a name="search-for-a-model-using-the-api"></a>使用 API 搜索模型

若要检索所有已注册模型的列表，请使用 MLflow 客户端 API ``list_model_versions()`` 方法：

```python
from pprint import pprint

client = MlflowClient()
for rm in client.list_registered_models():
  pprint(dict(rm), indent=4)
```

输出：

```console
{   'creation_timestamp': 1582671933216,
    'description': None,
    'last_updated_timestamp': 1582671960712,
    'latest_versions': [<ModelVersion: creation_timestamp=1582671933246, current_stage='Production', description='A random forest model containing 100 decision trees trained in scikit-learn', last_updated_timestamp=1582671960712, name='sk-learn-random-forest-reg-model', run_id='ae2cc01346de45f79a44a320aab1797b', source='./mlruns/0/ae2cc01346de45f79a44a320aab1797b/artifacts/sklearn-model', status='READY', status_message=None, user_id=None, version=1>,
    <ModelVersion: creation_timestamp=1582671960628, current_stage='None', description=None, last_updated_timestamp=1582671960628, name='sk-learn-random-forest-reg-model', run_id='d994f18d09c64c148e62a785052e6723', source='./mlruns/0/d994f18d09c64c148e62a785052e6723/artifacts/sklearn-model', status='READY', status_message=None, user_id=None, version=2>],
    'name': 'sk-learn-random-forest-reg-model'}
```

还可以使用 MLflow 客户端 API ``search_model_versions()`` 方法搜索特定的模型名称并列出其版本详细信息：

```python
from pprint import pprint

client=MlflowClient()
[pprint(mv) for mv in client.search_model_versions("name=<model-name>")]
```

输出：

```console
{   'creation_timestamp': 1582671933246,
    'current_stage': 'Production',
    'description': 'A random forest model containing 100 decision trees '
                   'trained in scikit-learn',
    'last_updated_timestamp': 1582671960712,
    'name': 'sk-learn-random-forest-reg-model',
    'run_id': 'ae2cc01346de45f79a44a320aab1797b',
    'source': './mlruns/0/ae2cc01346de45f79a44a320aab1797b/artifacts/sklearn-model',
    'status': 'READY',
    'status_message': None,
    'user_id': None,
    'version': 1 }

{   'creation_timestamp': 1582671960628,
    'current_stage': 'None',
    'description': None,
    'last_updated_timestamp': 1582671960628,
    'name': 'sk-learn-random-forest-reg-model',
    'run_id': 'd994f18d09c64c148e62a785052e6723',
    'source': './mlruns/0/d994f18d09c64c148e62a785052e6723/artifacts/sklearn-model',
    'status': 'READY',
    'status_message': None,
    'user_id': None,
    'version': 2 }
```

## <a name="delete-a-model-or-model-version"></a>删除模型或模型版本

可以使用 UI 或 API 删除模型。

### <a name="delete-a-model-version-or-model-using-the-ui"></a>使用 UI 删除模型版本或模型

> [!WARNING]
>
> 不能撤消此操作。 可以将模型版本转换到“已存档”阶段，而无需在注册表中删除它。 删除某一模型时，将删除模型注册表存储的所有模型项目以及与该已注册模型关联的所有元数据。

> [!NOTE]
>
> 只能删除处于“无”或“已存档”阶段的模型和模型版本。 如果某个已注册模型具有处于“暂存”或“生产”阶段的版本，需要将这些版本转换为“无”或“已存档”阶段，然后才能删除该模型。

删除模型版本：

1. 单击 ![侧栏中的](../../../_static/images/icons/models-icon.png) “模型”图标。
2. 单击模型名称。
3. 单击模型版本。
4. 选择 ![下拉按钮](../../../_static/images/icons/button-down.png) （位于版本号旁）。

   > [!div class="mx-imgBorder"]
   > ![删除模型版本](../../../_static/images/mlflow/delete-model-version.png)

5. 单击“删除”按钮。

删除模型：

1. 单击 ![侧栏中的](../../../_static/images/icons/models-icon.png) “模型”图标。
2. 单击模型名称。
3. 选择 ![下拉按钮](../../../_static/images/icons/button-down.png) （位于模型旁）。
4. 单击“删除”按钮。

### <a name="delete-a-model-version-or-model-using-the-api"></a>使用 API 删除模型版本或模型

> [!WARNING]
>
> 不能撤消此操作。 可以将模型版本转换到“已存档”阶段，而无需在注册表中删除它。 删除某一模型时，将删除模型注册表存储的所有模型项目以及与该已注册模型关联的所有元数据。

> [!NOTE]
>
> 只能删除处于“无”或“已存档”阶段的模型和模型版本。 如果某个已注册模型具有处于“暂存”或“生产”阶段的版本，需要将这些版本转换为“无”或“已存档”阶段，然后才能删除该模型。

#### <a name="delete-a-model-version"></a>删除模型版本

若要删除模型版本，请使用 MLflow 客户端 API ``delete_model_version()`` 方法：

```python
# Delete versions 1,2, and 3 of the model
client = MlflowClient()
versions=[1, 2, 3]
for version in versions:
  client.delete_model_version(name="<model-name>", version=version)
```

#### <a name="delete-a-model"></a>删除模型

若要删除模型，请使用 MLflow 客户端 API ``delete_registered_model()`` 方法：

```python
client = MlflowClient()
client.delete_registered_model(name="<model-name>")
```

## <a name="share-models-across-workspaces"></a>跨工作区共享模型

Azure Databricks 支持在多个工作区之间共享模型。 例如，你可以在自己的工作区中开发和记录模型，然后将其注册到集中式模型注册表。 此特性非常适合多个团队共享模型访问权限的情况。 可以创建多个工作区，并在这些环境中使用和管理模型。

* [跨工作区共享模型](multiple-workspaces.md)

## <a name="example"></a>示例

此示例演示如何使用模型注册表生成机器学习应用程序。

[MLflow 模型注册表示例](../../mlflow/model-registry-example.md)