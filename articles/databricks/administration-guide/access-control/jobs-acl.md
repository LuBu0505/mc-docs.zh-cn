---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/02/2020
title: 为工作区启用作业访问控制 - Azure Databricks
description: 为 Azure Databricks 作业启用和禁用访问控制功能。
ms.openlocfilehash: ef27cefd4975343cbad37a3fdc0d25f5390cecf5
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060160"
---
# <a name="enable-jobs-access-control-for-your-workspace"></a>为工作区启用作业访问控制

> [!NOTE]
>
> 访问控制仅在 [Azure Databricks Premium 计划](https://databricks.com/product/azure-pricing)中可用。

为[作业](../../jobs.md)启用访问控制可以使作业所有者控制哪些用户可以查看作业结果或管理作业的运行。 本文介绍如何启用作业访问控制，以及如何防止用户看到他们无权访问的作业。

有关分配权限和配置作业访问控制的信息，请参阅[作业访问控制](../../security/access-control/jobs-acl.md)。

## <a name="enable-jobs-access-control"></a>启用作业访问控制

1. 转到[管理控制台](../admin-console.md)。
2. 选择“访问控制”选项卡。

   > [!div class="mx-imgBorder"]
   > ![“访问控制”选项卡](../../_static/images/admin-settings/access-control-tab-azure.png)

3. 单击“群集、池和作业访问控制”旁边的“启用”按钮 。

   > [!div class="mx-imgBorder"]
   > ![启用访问控制](../../_static/images/admin-settings/cluster-and-jobs-acls.png)

4. 单击“确认”  。

## <a name="prevent-users-from-seeing-jobs-they-do-not-have-access-to"></a><a id="jobs-visibility"> </a><a id="prevent-users-from-seeing-jobs-they-do-not-have-access-to"> </a>防止用户看到他们无权访问的作业

> [!NOTE]
>
> 默认情况下，发布 Azure Databricks 平台 3.34 版（于 2020 年 12 月发布）之后创建的工作区的作业可见性控制处于启用状态。 如果工作区是在此之前创建的，则管理员必须启用该功能。

作业访问控制本身不会阻止用户看到 Azure Databricks UI 中显示的作业，即使用户没有这些作业的权限也是如此。 若要防止用户看到这些作业，请执行以下操作：

1. 转到[管理控制台](../admin-console.md)。
2. 选择“访问控制”选项卡。
3. 单击“作业可见性控制”旁边的“启用”按钮 。
4. 单击“确认”  。

若要禁用作业可见性控制，请使用相同的过程，在第三步中单击“禁用”。