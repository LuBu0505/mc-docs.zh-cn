---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/11/2020
title: 笔记本范围内的 Python 库 - Azure Databricks
description: 了解如何在 Azure Databricks 中管理 Python 包和笔记本范围内的库。
ms.openlocfilehash: 8f3a5b877303fe32533cf4b0083a6d20e0469d43
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058497"
---
# <a name="notebook-scoped-python-libraries"></a>笔记本范围内的 Python 库

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../release-notes/release-types.md)提供。

笔记本范围内的库允许你创建、保存、重用和共享特定于笔记本的自定义 Python 环境。 安装笔记本范围内的库时，只有当前笔记本以及与该笔记本关联的任何作业有权访问该库。 附加到同一群集的其他笔记本不受影响。

笔记本范围内的库不会跨会话保留。 必须在每个会话开始时或从群集中分离笔记本时，重新安装笔记本范围内的库。

安装笔记本范围内的库有两种方法：

* 在笔记本中运行 ``%pip`` 或 ``%conda`` magic 命令。 [Databricks Runtime 7.1](../release-notes/runtime/7.1.md) 和更高版本支持 ``%pip`` 命令。 Databricks Runtime 6.4 ML 及更高版本以及用于基因组学的 Databricks Runtime 6.4 及更高版本都支持 ``%pip`` 和 ``%conda``。 本文介绍如何使用这些 magic 命令。
* 调用 Azure Databricks 库实用工具。 库实用工具仅在 Databricks Runtime 上受支持，而在 Databricks Runtime ML 或用于基因组学的 Databricks Runtime 上不受支持。 请参阅[库实用工具](../dev-tools/databricks-utils.md#dbutils-library)。

若要为附加到群集的所有笔记本安装库，请使用[工作区](workspace-libraries.md)库和[群集安装的](cluster-libraries.md)库。

## <a name="requirements"></a>要求

默认情况下会在 Databricks Runtime 7.1 及更高版本、Databricks Runtime 7.1 及更高版本和用于基因组学的 Databricks Runtime 7.1 及更高版本中启用以笔记本为作用域的库。

在 Runtime 6.4 ML 至 7.0 ML 和 Databricks Runtime 6.4 for Genomics 至 Databricks Runtime 7.0 for Genomics 中，还可通过配置设置来使用这些库。 将 [Spark 配置](../clusters/configure.md#spark-config) ``spark.databricks.conda.condaMagic.enabled`` 设置为 ``true``。

在运行 Databricks Runtime 7.4 ML 或用于基因组学的 Databricks Runtime 7.4 或更低版本的高并发群集上，以笔记本为作用域的库与[表访问控制](../security/access-control/table-acls/index.md)或[凭据直通](../security/credential-passthrough/adls-passthrough.md)不兼容。 替代方法是使用 Databricks Runtime 群集上的[库实用工具](../dev-tools/databricks-utils.md#dbutils-library)，或者将群集升级到 Databricks Runtime 7.5 ML 或者用于基因组学的 Databricks Runtime 7.5 或更高版本。

以笔记本为作用域的库与 [Databricks Connect](../dev-tools/databricks-connect.md) 也不兼容。 请使用[库实用工具](../dev-tools/databricks-utils.md#dbutils-library)安装带 Databricks Connect 的以笔记本为作用域的库。

### <a name="driver-node"></a><a id="driver-node"> </a><a id="enable-pip-and-conda-magic-commands"> </a>驱动程序节点

使用笔记本范围内的库可能会导致更多的流量流向驱动程序节点，因为它可使环境在执行程序节点之间保持一致。 当使用包含 10 个或更多个节点的群集时，Databricks 建议驱动程序节点至少满足以下规范：

* 对于 100 节点 CPU 群集，请使用 Standard_DS5_v2。
* 对于 10 节点 GPU 群集，请使用 Standard_NC12。

对于更大的群集，请使用更大的驱动程序节点。

## <a name="use-notebook-scoped-libraries"></a>使用以笔记本为作用域的库

Databricks Runtime 使用 ``%pip`` magic 命令来创建和管理笔记本范围内的库。 在 Databricks Runtime ML 和用于基因组学的 Databricks Runtime 上，还可以使用 ``%conda`` magic 命令。 Databricks 建议使用 ``pip`` 来安装库，除非你要安装的库建议使用 ``conda``。 有关详细信息，请参阅[了解 conda 和 pip](https://www.anaconda.com/understanding-conda-and-pip)。

> [!IMPORTANT]
>
> * 应将所有 ``%pip`` 和 ``%conda`` 命令放在笔记本的开头。 在修改环境的 ``%pip`` 或 ``%conda`` 命令后，会重置笔记本状态。 如果在笔记本中创建 Python 方法或变量，然后在后面的一个单元格中使用 ``%pip`` 或 ``%conda`` 命令，则这些方法或变量会丢失。
> * 如果必须在笔记本中同时使用 ``%pip`` 和 ``%conda`` 命令，请参阅 [ 和 conda 命令之间的交互](#pip-conda-interactions)。
> * 在用于机器学习的 Databricks Runtime 中，使用 ``%pip`` 或 ``%conda`` 卸载或修改核心 Python 包（例如 IPython 或 conda）可能会导致某些功能无法正常工作。 如果遇到问题，可以通过拆离并重新附加笔记本或者通过重启群集来重置环境。

### <a name="manage-libraries-with-pip-commands"></a><a id="manage-libraries-with-pip-commands"> </a><a id="pip-commands"> </a>使用 ``%pip`` 命令管理库

以下部分包含一些示例，说明如何使用 ``%pip`` 命令来管理环境。

#### <a name="use-a-requirements-file-to-install-libraries"></a>使用要求文件安装库

[要求文件](https://pip.pypa.io/en/stable/user_guide/#requirements-files)包含要使用 pip 安装的包的列表。 文件的名称必须以 ``requirements.txt`` 结尾。 以下是使用要求文件的示例：

```
%pip install -r /dbfs/requirements.txt
```

有关 ``requirements.txt`` 文件的详细信息，请参阅[要求文件格式](https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format)。

#### <a name="install-a-library-with-pip"></a>使用 ``pip`` 安装库

```
%pip install matplotlib
```

#### <a name="install-a-wheel-package-with-pip"></a>使用 ``pip`` 安装 wheel 包

```
%pip install /dbfs/my_package.whl
```

#### <a name="uninstall-a-library-with-pip"></a>使用 ``pip`` 卸载库

> [!NOTE]
>
> 你无法卸载 [Databricks Runtime](../runtime/dbr.md) 中包含的库，也无法卸载作为[群集库](cluster-libraries.md)安装的库。 如果你安装的库版本不同于 Databricks Runtime 中包含的版本或在群集上安装的版本，则可以使用 ``%pip uninstall`` 将库还原为 Databricks Runtime 中的默认版本或在群集上安装的版本，但不能使用 ``%pip`` 命令卸载 Databricks Runtime 中包含的库版本或在群集上安装的库版本。

```
%pip uninstall -y matplotlib
```

必须使用 -y 选项。

#### <a name="install-pypi-libraries-from-a-version-control-system-project-url-with-pip"></a>使用 ``%pip`` 从版本控制系统项目 URL 安装 PyPI 库

```
%pip install git+https://github.com/databricks/databricks-cli
```

可以向 URL 添加参数，以指定版本或 Git 子目录等。 有关详细信息以及其他版本控制系统的示例，请参阅 [pip 安装文档](https://pip.pypa.io/en/stable/reference/pip_install/#vcs-support)。

#### <a name="install-non-pypi-libraries-from-a-version-control-system-project-url-with-pip"></a>使用 ``%pip`` 从版本控制系统项目 URL 安装非 PyPI 库

```
%pip install --index-url http://<personal-access-token>@your-package-repository.com/your/file/path <package>==<version>
```

#### <a name="install-a-private-package-with-credentials-managed-by-databricks-secrets-with-pip"></a>使用 ``%pip`` 通过 Databricks 机密管理的凭据安装专用包

Databricks 机密 API 允许存储身份验证令牌和密码。 使用 [DBUtils API](../dev-tools/databricks-connect.md#enabling-dbutilssecretsget) 从笔记本访问机密。

```
%pip install git+https://<token>@gitprovider.com/<user>/<respository>.git/@<version>#egg=<package>-0
```

还可以在 magic 命令中使用 ``$variables``。

```
token = dbutils.secrets.get(scope="scope", key="key")
%pip install git+https://$token@gitprovider.com/<user>/<repository>.git
```

#### <a name="install-a-package-from-dbfs-with-pip"></a>使用 ``%pip`` 从 DBFS 安装包

可以使用 ``pip`` 安装已保存在 DBFS 上的专用包。

将文件上传到 DBFS 时，它将自动重命名该文件，并使用下划线替换空格、句点和连字符。 ``pip`` 要求 wheel 文件的名称在版本（例如 0.1.0）中使用句点和连字符，而不是空格或下划线。 若要使用 ``%pip`` 命令来安装包，必须[重命名文件](../dev-tools/databricks-utils.md#dbutils-fs)以满足这些要求。

```
%pip install /dbfs/mypackage-0.0.1-py3-none-any.whl
```

#### <a name="save-libraries-in-a-requirements-file"></a>在要求文件中保存库

```
%pip freeze > /dbfs/requirements.txt
```

文件路径中的任何子目录都必须已存在。 调用 ``%pip freeze > /dbfs/<new-directory>/requirements.txt`` 时，如果目录 ``/dbfs/<new-directory>`` 尚不存在，则该命令会失败。

### <a name="manage-libraries-with-conda-commands"></a><a id="conda-commands"> </a><a id="manage-libraries-with-conda-commands"> </a>使用 ``%conda`` 命令管理库

``%conda`` magic 命令在 Databricks Runtime 上不可用。 它们在用于机器学习的 Databricks Runtime 和用于基因组学的 Databricks Runtime 上可用。

以下部分包含一些示例，说明如何使用 ``%conda`` 命令来管理环境。

#### <a name="install-a-library-with-conda"></a>使用 ``conda`` 安装库

```
%conda install matplotlib
```

#### <a name="uninstall-a-library-with-conda"></a>使用 ``conda`` 卸载库

```
%conda uninstall matplotlib
```

## <a name="save-and-reuse-or-share-an-environment"></a><a id="copy-environment"> </a><a id="save-and-reuse-or-share-an-environment"> </a>保存并重用或共享环境

将笔记本从群集中分离时，环境不会保存。 若要保存环境以便以后重用或将其与他人共享，请按照以下步骤进行操作。

Databricks 建议仅在运行同一版本的 Databricks Runtime ML 或同一版本的用于基因组学的 Databricks Runtime 的群集之间共享环境。

1. 将环境另存为 conda YAML 规范。

   ```
   %conda env export -f /dbfs/myenv.yml
   ```

2. 使用 ``conda env update`` 将文件导入到另一个笔记本。

   ```
   %conda env update -f /dbfs/myenv.yml
   ```

## <a name="list-the-python-environment-of-a-notebook"></a>列出笔记本的 Python 环境

若要显示与笔记本关联的 Python 环境，请使用 ``%conda list``：

```
%conda list
```

## <a name="interactions-between-pip-and-conda-commands"></a><a id="interactions-between-pip-and-conda-commands"> </a><a id="pip-conda-interactions"> </a>``pip`` 和 ``conda`` 命令之间的交互

若要避免冲突，请在使用 ``pip`` 或 ``conda`` 安装 Python 包和库时遵循这些准则。

* 通过 [API](../dev-tools/api/latest/libraries.md) 或[群集 UI](cluster-libraries.md) 安装的库是使用 pip 安装的。 如果已从 API 或群集 UI 安装了任何库，则在安装笔记本范围内的库时，应仅使用 ``%pip`` 命令。
* 如果在群集上使用以笔记本为作用域的库，则在该群集上运行的初始化脚本可以使用 ``conda`` 或 ``pip`` 命令来安装库。 但是，如果初始化脚本包含 ``pip`` 命令，则仅使用笔记本中的 ``%pip`` 命令（而不是 ``%conda``）。
* 最好是专门使用 ``pip`` 命令，或专门使用 ``conda`` 命令。 如果必须使用 ``conda`` 安装一些包，并使用 ``pip`` 安装另一些包，请先运行 ``conda`` 命令，然后再运行 ``pip`` 命令。 有关详细信息，请参阅[在 Conda 环境中使用 Pip](https://www.anaconda.com/using-pip-in-a-conda-environment/)。

## <a name="frequently-asked-questions-faq"></a>常见问题解答 (FAQ)

* [从群集 UI/API 安装的库如何与笔记本范围内的库交互？](#how-do-libraries-installed-from-the-cluster-uiapi-interact-with-notebook-scoped-libraries)
* [使用初始化脚本安装的库如何与以笔记本为作用域的库交互？](#how-do-libraries-installed-using-an-init-script-interact-with-notebook-scoped-libraries)
* [能否在作业笔记本中使用 ``%pip`` 和 ``%conda`` 命令？](#can-i-use-pip-and-conda-commands-in-job-notebooks)
* [能否在 R 或 Scala 笔记本中使用 ``%pip`` 和 ``%conda`` 命令？](#can-i-use-pip-and-conda-commands-in-r-or-scala-notebooks)
* [能否使用 ``%sh pip`` 或 ``%sh conda``？](#can-i-use-sh-pip-or-sh-conda)
* [能否使用 ``%conda`` 命令更新 R 包？](#can-i-update-r-packages-using-conda-commands)

### <a name="how-do-libraries-installed-from-the-cluster-uiapi-interact-with-notebook-scoped-libraries"></a>从群集 UI/API 安装的库如何与笔记本范围内的库交互？

从群集 UI 或 API 安装的库可用于群集上的所有笔记本。 这些库是使用 ``pip`` 安装的；因此，如果库是使用群集 UI 安装的，则在笔记本中只能使用 ``%pip`` 命令。

### <a name="how-do-libraries-installed-using-an-init-script-interact-with-notebook-scoped-libraries"></a>使用初始化脚本安装的库如何与以笔记本为作用域的库交互？

使用初始化脚本安装的库可用于群集上的所有笔记本。

如果在运行 Databricks Runtime ML 或用于基因组学的 Databricks Runtime 的群集上使用笔记本范围内的库，则群集上运行的初始化脚本可以使用 ``conda`` 或 ``pip`` 命令来安装库。 但是，如果初始化脚本包含 ``pip`` 命令，则仅使用笔记本中的 ``%pip`` 命令。

例如，此笔记本代码片段会生成一个脚本，该脚本将在所有群集节点上安装 fast.ai 包。

```python
dbutils.fs.put("dbfs:/home/myScripts/fast.ai", "conda install -c pytorch -c fastai fastai -y", True)
```

### <a name="can-i-use-pip-and-conda-commands-in-job-notebooks"></a>能否在作业笔记本中使用 ``%pip`` 和 ``%conda`` 命令？

是的。

### <a name="can-i-use-pip-and-conda-commands-in-r-or-scala-notebooks"></a>是否可以在 R 或 Scala 笔记本中使用 ``%pip`` 和 ``%conda`` 命令？

是的，可以在 [Python magic 单元](../notebooks/notebooks-use.md#mix-languages)中使用。

### <a name="can-i-use-sh-pip-or-sh-conda"></a>能否使用 ``%sh pip`` 或 ``%sh conda``？

Databricks 不建议使用 ``%sh pip``，因为它与 ``%pip`` 用法不兼容。

### <a name="can-i-update-r-packages-using-conda-commands"></a>能否使用 ``%conda`` 命令更新 R 包？

错误。

## <a name="limitations"></a>限制

* 如果在高并发群集上启用了[表访问控制](../security/access-control/table-acls/index.md)或[凭据传递](../security/credential-passthrough/adls-passthrough.md)，则不能使用 `%pip` 和 `%conda` 命令。 一种替代方法是使用 Databricks Runtime 上的[库实用工具](../dev-tools/databricks-utils.md#dbutils-library)。

* 不支持以下 `conda` 命令：
  * `activate`
  * `create`
  * `init`
  * `run`
  * `env create`
  * `env remove`

## <a name="known-issues"></a>已知问题

* 在 Databricks Runtime 7.0 ML 及更低版本和用于基因组学的 Databricks Runtime 7.0 及更低版本上，如果注册的 UDF 依赖于使用 ``%pip`` 或 ``%conda`` 安装的 Python 包，则它不会在 ``%sql`` 单元格中工作。 请改用 Python 命令行界面中的 ``spark.sql``。
* 在 Databricks Runtime 7.2 ML 及更低版本和用于基因组学的 Databricks Runtime 7.2 及更低版本上，使用 ``%conda`` 更新笔记本环境时，不会在 Python 工作进程上激活新环境。 如果 PySpark UDF 函数调用第三方函数（该函数使用在 Conda 环境中安装的资源），这可能会导致问题。
* 使用 ``%conda env update`` 更新笔记本环境时，不保证包的安装顺序。 这可能会导致 ``horovod`` 包出现问题，在这种情况下，需要在 ``horovod`` 之前安装 ``tensorflow`` 和 ``torch``，以便分别使用 ``horovod.tensorflow`` 或 ``horovod.torch``。 如果发生这种情况，请卸载 ``horovod`` 包，并在确保安装依赖项后重新安装它。