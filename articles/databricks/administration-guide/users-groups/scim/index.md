---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/03/2020
title: 使用 SCIM 预配用户和组 - Azure Databricks
description: 了解如何使用支持 SCIM 的 IdP 将用户预配到 Azure Databricks。
ms.openlocfilehash: 9819d6dc394dd72dde5f2f1b9dd4358ffac3b652
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059976"
---
# <a name="provision-users-and-groups-using-scim"></a>使用 SCIM 预配用户和组

Azure Databricks 支持 [SCIM](http://www.simplecloud.info/)（跨域身份管理系统，一种可用于将用户预配过程自动化的开放标准）。 借助 SCIM，你可使用标识提供者 (IdP) 在 Azure Databricks 中创建用户，并为他们提供适当的访问级别，当他们离开你的组织或不再需要访问 Azure Databricks 时，你还可删除他们的访问权限（对其进行取消预配）。 此外，还可以直接调用 [SCIM API](../../../dev-tools/api/latest/scim/index.md) 来管理预配。 仅可使用 SCIM API 执行某些用户管理（例如[临时停用或重新激活](../../../dev-tools/api/latest/scim/scim-users.md#activate-and-deactivate-user-by-id)）。

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。

> [!NOTE]
>
> * 只有 Azure Databricks [管理员](../users.md)才能配置标识提供者，以将用户预配到 Azure Databricks 或直接调用 Azure Databricks SCIM API。
> * 使用 SCIM 预配时，存储在 IdP 中的用户和组属性可能会替代你使用 Azure Databricks [管理控制台](../../admin-console.md) 和[组 API](../../../dev-tools/api/latest/groups.md) 做出的更改。 例如，如果在 IdP 中为某个用户分配了“允许创建群集”权限，而你在 Azure Databricks [管理控制台](../../admin-console.md)中使用“用户”选项卡删除了该权限，那么，在 IdP 下一次与 Azure Databricks 同步时，如果 IdP 配置为预配该权限，则会重新授予该权限。 相同的行为也适用于组。

以下文章介绍了如何进行设置以使用启用了 SCIM 的受支持 IdP 来预配用户：

* [为 Microsoft Azure Active Directory 配置 SCIM 预配](aad.md)

若要了解如何使用 Azure Databricks SCIM API，请参阅 [SCIM API](../../../dev-tools/api/latest/scim/index.md)。