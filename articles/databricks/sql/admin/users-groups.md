---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/14/2021
title: 管理用户和组 - Azure Databricks
description: 了解如何管理 Azure Databricks SQL Analytics 用户和组。
ms.openlocfilehash: 6d1694df06cd43bd26c2f13fad102cc98d838a14
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692329"
---
# <a name="manage-users-and-groups"></a>管理用户和组

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

只有 Azure Databricks 管理员可管理用户和组。 Azure Databricks 管理员可管理 Azure Databricks [工作区](../../workspace-index.md)中的[用户和组](../../administration-guide/users-groups/index.md)。

所有用户都作为 ``users`` 组的成员继承对 SQL Analytics 的访问权限。 工作区管理员可以从 ``users`` 组中删除 SQL Analytics 权利，然后将其单独分配给用户。

## <a name="grant-a-user-access-to-sql-analytics"></a><a id="grant-a-user-access-to-sql-analytics"> </a><a id="sqla-user"> </a>向用户授予对 SQL Analytics 的访问权限

1. 单击 ![应用切换器图标](../../_static/images/icons/app-switcher-icon.png) 图标（位于边栏底部）。
2. 选择 ![应用切换器 - 工作区](../../_static/images/icons/app-workspace.png)
3. 转到[管理控制台](../../administration-guide/admin-console.md)。
4. 转到“用户”[](../../administration-guide/users-groups/users.md)选项卡。
5. 在用户行中，选中“SQL Analytics 访问权限”复选框。

   > [!div class="mx-imgBorder"]
   > ![SQLA 访问权限](../../_static/images/sql/user-sqla-access.png)

6. 单击“确认”  。

## <a name="grant-a-group-access-to-sql-analytics"></a>向组授予对 SQL Analytics 的访问权限

1. 单击 ![应用切换器图标](../../_static/images/icons/app-switcher-icon.png) 图标（位于边栏底部）。
2. 选择 ![应用切换器 - 工作区](../../_static/images/icons/app-workspace.png)
3. 转到[管理控制台](../../administration-guide/admin-console.md)。
4. 转到“组”选项卡。
5. 单击组名。
6. 单击“权利”选项卡。
7. 选中“SQL Analytics 访问权限”复选框。

   > [!div class="mx-imgBorder"]
   > ![SQLA 访问权限](../../_static/images/sql/group-sqla-access.png)

8. 单击 **“启用”** 。