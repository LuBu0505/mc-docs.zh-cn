---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/14/2021
title: 管理用户 - Azure Databricks
description: 了解如何在 Azure Databricks 中管理用户。
ms.openlocfilehash: a1305267a88a8d51d6c8bf9b020d20d64c403b79
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059975"
---
# <a name="manage-users"></a>管理用户

Azure Databricks 管理员是 ``admins`` [组](groups.md)的成员。

管理员可以使用[管理控制台](../admin-console.md)、[SCIM API](../../dev-tools/api/latest/scim/index.md) 或[支持 SCIM 的标识提供者](scim/index.md)（例如 Azure Active Directory）来管理用户帐户。 本文介绍如何使用管理控制台来管理用户。

可以使用管理控制台上的“用户”选项卡执行以下操作：

* 添加和删除用户。
* 授予和撤销工作区和 SQL Analytics 权利。
* 授予和撤销创建群集的权限（如果已为工作区启用了[群集访问控制](../../security/access-control/cluster-acl.md)）。
* 授予和撤销 ``admins`` 组中的成员身份。

还可以在管理控制台的其他部分执行以下用户管理任务，详见其他文章：

* 将用户添加到组。 请参阅[管理组](groups.md)。

> [!NOTE]
>
> 在工作区资源上具有“参与者”或“所有者”角色的用户可以通过 Azure 门户以管理员身份登录。 有关详细信息，请参阅[分配帐户管理员](../account-settings/account.md#assign-initial-account-admins)。

## <a name="add-a-user"></a><a id="add-a-user"> </a><a id="add-user"> </a>添加用户

1. 转到[管理控制台](../admin-console.md)。
2. 在“用户”选项卡上，单击“添加用户”。
3. 输入用户电子邮件地址 ID。 可以添加属于 Azure Databricks 工作区的 Azure Active Directory 租户的任何用户。

   > [!div class="mx-imgBorder"]
   > ![添加用户](../../_static/images/admin-settings/users-email-azure.png)

4. 单击“确定”。

用户已添加到工作区。

> [!div class="mx-imgBorder"]
> ![已添加用户](../../_static/images/admin-settings/add-user.png)

尽管未选中“工作区访问”和“SQL Analytics”复选框，但用户将以 ``users`` 组成员的身份继承这些权利，其中该组具有权利。 工作区管理员可从 ``users`` 组中删除权利，然后在“用户”页面上将其分别分配给用户。 有关 SQL Analytics 访问权利的信息，请参阅[授予用户对 SQL Analytics 的访问权限](../../sql/admin/users-groups.md#sqla-user)。

如果已启用[群集访问控制](../../security/access-control/cluster-acl.md)，则会在没有群集创建权利的情况下添加用户。

如果用户以前存在于工作区中，则将还原用户以前的权利。

## <a name="remove-a-user"></a><a id="remove-a-user"> </a><a id="remove-user"> </a>删除用户

1. 转到[管理控制台](../admin-console.md)。
2. 在“用户”选项卡上找到用户，然后单击用户行最右侧的 ![“删除用户”图标](../../_static/images/admin-settings/remove-user.png)。
3. 单击“删除用户”进行确认。