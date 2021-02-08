---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/16/2020
title: 2020 年 12 月 - Azure Databricks
description: Azure Databricks 新功能和改进的 2020 年 12 月发行说明。
ms.openlocfilehash: a3d20e3761dcd519d9bcfde8d02ff7875bb849e0
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060764"
---
# <a name="december-2020"></a>2020 年 12 月

这些功能和 Azure Databricks 平台改进已于 2020 年 12 月发布。

> [!NOTE]
>
> 发布分阶段进行。 Azure Databricks 帐户可能要等到初始发布日期后的一周或更长时间才会更新。

## <a name="databricks-runtime-75-ga"></a>Databricks Runtime 7.5 正式版

**2020 年 12 月 16 日**

Databricks Runtime 7.5、Databricks Runtime 7.5 ML 和用于基因组学的 Databricks Runtime 7.5 现已正式发布。

有关信息，请参阅 [Databricks Runtime 7.5](../../runtime/7.5.md)、[用于机器学习的 Databricks Runtime 7.5](../../runtime/7.5ml.md) 和[用于基因组学的 Databricks Runtime 7.5](../../runtime/7.5genomics.md) 提供的完整发行说明。

## <a name="jobs-api-now-supports-updating-existing-jobs"></a>作业 API 现在支持更新现有作业

**2020 年 12 月 14 日**

你现在可以使用作业 API [更新](../../../dev-tools/api/latest/jobs.md#update)终结点来更新作业的部分字段。 “更新”终结点补充了现有的[重置](../../../dev-tools/api/latest/jobs.md#reset)终结点，它允许你仅指定应添加、更改或删除的字段（而不是覆盖所有作业设置）。 当只需更改某些作业设置时（特别是对于批量更新），使用“更新”终结点速度更快、更简单且更安全。

## <a name="new-global-init-script-framework-is-ga"></a>新的全局初始化脚本框架现已发布

**2020 年 12 月 14 日**

在 7 月份作为公共预览版发布的新的全局初始化脚本框架现在已正式发布。 新框架相对于旧的全局初始化脚本进行了重大改进：

* Init 脚本更安全，需要管理员权限才能执行创建、查看和删除操作。
* 记录与脚本相关的启动失败。
* 可以设置多个 init 脚本的执行顺序。
* Init 脚本可以引用与群集相关的环境变量。
* 可以使用管理控制台或新的全局 Init 脚本 REST API 创建和管理 Init 脚本。

Databricks 建议[将现有的旧版全局 init 脚本迁移到新框架](../../../clusters/init-scripts.md#migrate-legacy-scripts)，以利用这些改进。

有关详细信息，请参阅[全局初始化脚本](../../../clusters/init-scripts.md#global-init-script)。

## <a name="databricks-runtime-75-beta"></a>Databricks Runtime 7.5（beta 版本）

**2020 年 12 月 3 日**

Databricks Runtime 7.5、Databricks Runtime 7.5 ML 和用于基因组学的 Databricks Runtime 7.5 现已作为 Beta 版本发布。

有关信息，请参阅 [Databricks Runtime 7.5](../../runtime/7.5.md)、[用于机器学习的 Databricks Runtime 7.5](../../runtime/7.5ml.md) 和[用于基因组学的 Databricks Runtime 7.5](../../runtime/7.5genomics.md) 提供的完整发行说明。

## <a name="jobs-api-end_time-field-now-uses-epoch-time"></a>作业 API“end_time”字段现在使用纪元时间

2020 年 12 月 2-8 日：版本 3.34

作业 API 为[运行获取](../../../dev-tools/api/latest/jobs.md#runs-get)和[运行列出](../../../dev-tools/api/latest/jobs.md#runs-list)终结点返回新的数据。 新的 ``end_time`` 字段返回运行结束时的 Epoch 时间（自 1970/1/1 UTC 以来的毫秒数），例如 ``1605266150681``。

## <a name="find-dbfs-files-using-new-visual-browser"></a>使用新的可视化浏览器查找 DBFS 文件

2020 年 12 月 2-8 日：版本 3.34

你现在可以使用新的 [DBFS 文件浏览器](../../../data/databricks-file-system.md#browse-dbfs-using-the-ui)在 Azure Databricks 工作区 UI 中浏览和搜索 DBFS 对象。

> [!div class="mx-imgBorder"]
> ![浏览 DBFS](../../../_static/images/dbfs/browse.png)

你还可以使用浏览器中的[“新建上传”对话框](../../../data/databricks-file-system.md#upload-data-to-dbfs-from-the-file-browser)将文件上传到 DBFS。

默认情况下，该功能被禁用。 管理员用户必须使用管理控制台来启用它。 请参阅[管理 DBFS 文件浏览器](../../../administration-guide/workspace/dbfs-browser.md)。

## <a name="visibility-controls-for-jobs-clusters-notebooks-and-other-workspace-objects-are-now-enabled-by-default-on-new-workspaces"></a>现在，新的工作区默认启用针对作业、群集、笔记本和其他工作区对象的可见性控件

2020 年 12 月 1-8 日：版本 3.34

可见性控制（9 月份引入）确保用户只能查看通过工作区、群集或作业访问控制为其授予了访问权限的那些笔记本、文件夹、群集和作业。 在此版本中，默认情况下会为新工作区启用可见性控制。 如果工作区是在此版本之前创建的，则管理员必须启用可见性控制。 请参阅：

* [防止用户看到他们无权访问的工作区对象](../../../administration-guide/access-control/workspace-acl.md#workspace-object-visibility)
* [防止用户看到他们无权访问的群集](../../../administration-guide/access-control/cluster-acl.md#cluster-visibility)
* [防止用户看到他们无权访问的作业](../../../administration-guide/access-control/jobs-acl.md#jobs-visibility)

## <a name="improved-display-of-nested-runs-in-mlflow"></a>在 MLflow 中改进了嵌套运行的显示

2020 年 12 月 2-8 日：版本 3.34

我们改进了 MLflow 运行表中的嵌套运行的显示。 子运行现在成组显示在根运行下。

## <a name="admins-can-now-lock-user-accounts-public-preview"></a>管理员现可锁定用户帐户（公共预览版）

2020 年 12 月 2-8 日：版本 3.34

管理员现在可以将用户的状态设置为非活动。 非活动用户无法访问工作区，但其权限以及与对象的关联（例如运行群集或对笔记本的访问权限）保持不变。 你可以通过 SCIM API 手动停用用户，也可以让系统在用户处于非活动状态一段时间后自动停用用户。 请参阅[按 ID 激活和停用用户](../../../dev-tools/api/latest/scim/scim-users.md#activate-and-deactivate-user-by-id)。

## <a name="updated-nvidia-driver"></a>更新了 NVIDIA 驱动程序

2020 年 12 月 2-8 日：版本 3.34

启用了 GPU 的群集现在使用 NVIDIA 驱动程序版本 450.80.02。