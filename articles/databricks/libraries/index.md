---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/11/2020
title: 库 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用和管理库。
ms.openlocfilehash: ba187f5b45b409e3828f05d75a3dcf5528c0ac90
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060029"
---
# <a name="libraries"></a>库

若要使第三方或自定义代码可供在你的群集上运行的笔记本和作业使用，你可以安装库。 可以用 Python、Java、Scala 和 R 编写库。你可以上传 Java、Scala 和 Python 库，将其指向 PyPI、Maven 和 CRAN 存储库中的外部包。

本文重点介绍了如何在工作区 UI 中执行库任务。 你还可以使用[库 CLI](../dev-tools/cli/libraries-cli.md) 或[库 API](../dev-tools/api/latest/libraries.md) 来管理库。

> [!TIP]
>
> Databricks 包含 Databricks Runtime 中的许多常用库。 若要查看 Databricks Runtime 中包含哪些库，请查看你的 Databricks Runtime 版本的 [Databricks Runtime 发行说明](../release-notes/runtime/releases.md)中的“系统环境”小节。

> [!NOTE]
>
> Microsoft 支持部门帮助隔离和解决 Azure Databricks 所安装和维护的库的相关问题。 对于第三方组件（包括库），Microsoft 提供商业上合理的支持，以便用户进一步排查问题。 Microsoft 支持部门会尽最大努力帮助用户解决问题。 对于托管在 Github 上的开源连接器和项目，建议在 Github 上提出和跟进问题。 标准支持案例提交流程不支持通过 shade 解决 jar 冲突或构建 Python 库等开发工作：需要通过咨询更快解决这些问题。 支持人员可能会邀请你利用其他开源技术渠道，你可以通过这些渠道找到在该技术方面拥有深入专业知识的资源。 有几个社区站点；其中两个是[有关 Azure Databricks 的 Microsoft Q&A 页](https://docs.microsoft.com/answers/topics/azure-databricks.html)和 [Stack Overflow](https://stackoverflow.com/)。

可以采用以下三种模式来安装库：“工作区”、“群集安装”，以及“作用域为笔记本”。

* [工作区库](workspace-libraries.md)充当本地存储库，你可以从中创建群集安装库。 工作区库可能是你的组织创建的自定义代码，也可能是你的组织已经标准化的开源库的特定版本。
* [群集库](cluster-libraries.md)可供群集上运行的所有笔记本使用。 可以直接从公共存储库（例如 PyPI 或 Maven）安装群集库，也可以从以前安装的工作区库中创建一个。
* [作用域为笔记本的 Python 库](notebooks-python-libraries.md)允许你安装 Python 库并创建作用域为笔记本会话的环境。 作用域为笔记本的库不会影响在同一群集上运行的其他笔记本。 这些库不会保留，必须为每个会话重新安装这些库。

  当每个特定笔记本需要一个自定义 Python 环境时，请使用作用域为笔记本的库。 使用作用域为笔记本的库，还可以保存、重用和共享 Python 环境。

  * 在 Databricks Runtime ML 6.4 及更高版本中，可通过 ``%pip`` 和 ``%conda`` magic 命令使用作用域为笔记本的库，在Databricks Runtime 7.1 及更高版本中，可通过 ``%pip`` magic 命令使用这些库。 请参阅[作用域为笔记本的 Python 库](notebooks-python-libraries.md)。
  * 尽管以笔记本为范围的库与 ``%pip`` 不兼容，但也可通过库实用工具使用它们（建议对所有新的工作负荷使用 ``%pip``）。 请参阅[库实用工具](../dev-tools/databricks-utils.md#dbutils-library)。

本部分的内容：

* [工作区库](workspace-libraries.md)
* [群集库](cluster-libraries.md)
* [笔记本范围内的 Python 库](notebooks-python-libraries.md)