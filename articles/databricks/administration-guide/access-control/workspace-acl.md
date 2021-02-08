---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 09/14/2020
title: 启用工作区对象访问控制 - Azure Databricks
description: 了解如何为 Azure Databricks 工作区对象（例如文件夹、笔记本、MLflow 试验和 MLflow 模型）启用和禁用访问控制功能。
keyword: workspace acl
ms.openlocfilehash: 93dbac2eaa7ea8916947d54c1b507f7a0517e1a5
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060158"
---
# <a name="enable-workspace-object-access-control"></a><a id="enable-workspace-object-access-control"> </a><a id="workspace-acl"> </a>启用工作区对象访问控制

> [!NOTE]
>
> 访问控制仅在 [Azure Databricks 高级计划](https://databricks.com/product/azure-pricing)中提供。

默认情况下，除非管理员启用工作区访问控制，否则所有用户均可创建和修改[工作区](../../workspace/index.md)对象（包括文件夹、笔记本、试验和模型）。 在使用工作区访问控制的情况下，用户的操作能力取决于单个权限。 本文介绍如何启用工作区访问控制，以及如何防止用户看到他们无权访问的工作区对象。

有关分配权限和配置工作区对象访问控制的信息，请参阅[工作区对象访问控制](../../security/access-control/workspace-acl.md)。

## <a name="enable-workspace-object-access-control"></a><a id="enable-workspace-acl"> </a><a id="enable-workspace-object-access-control"> </a>启用工作区对象访问控制

1. 转到[管理控制台](../admin-console.md)。
2. 选择“访问控制”选项卡。

   > [!div class="mx-imgBorder"]
   > ![“访问控制”选项卡](../../_static/images/admin-settings/access-control-tab-azure.png)

3. 单击“工作区访问控制”旁边的“启用”按钮。
4. 单击“确认”  。

## <a name="prevent-users-from-seeing-workspace-objects-they-do-not-have-access-to"></a><a id="prevent-users-from-seeing-workspace-objects-they-do-not-have-access-to"> </a><a id="workspace-object-visibility"> </a>防止用户看到他们无权访问的工作区对象

> [!NOTE]
>
> 默认情况下，发布 Azure Databricks 平台 3.34 版（于 2020 年 12 月发布）之后创建的工作区的工作区可见性控制处于启用状态。 如果工作区是在此之前创建的，则管理员必须启用该功能。

工作区访问控制本身不会阻止用户看到 Azure Databricks UI 中显示的工作区对象的文件名，即使用户没有这些工作区对象的权限。 若要防止笔记本文件名和文件夹显示给没有权限的用户，请执行以下操作：

1. 转到[管理控制台](../admin-console.md)。
2. 选择“访问控制”选项卡。
3. 单击“工作区可见性控制”旁边的“启用”按钮 。
4. 单击“确认”  。

若要禁用工作区可见性控制，请使用相同的过程，在第三步中单击“禁用”。

## <a name="library-and-jobs-access-control"></a>库和作业访问控制

![“库”图标](../../_static/images/icons/library.png) 所有用户均可查看库。 若要控制谁可以将库附加到群集，请参阅[群集访问控制](../../security/access-control/cluster-acl.md)。

![“作业”图标](../../_static/images/icons/jobs.png) 若要启用作业访问控制和作业可见性访问控制，请参阅[为工作区启用作业访问控制](jobs-acl.md)。 若要控制谁可以运行作业并查看作业运行结果，请参阅[作业访问控制](../../security/access-control/jobs-acl.md)。