---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 09/09/2020
title: 为工作区启用池访问控制 - Azure Databricks
description: 了解如何启用和禁用 Azure Databricks 池的访问控制功能，这些池是一组空闲的现成可用实例，可用于启动群集。
ms.openlocfilehash: f1674e3be86611f312f58bb94277c65244858d06
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060159"
---
# <a name="enable-pool-access-control-for-your-workspace"></a>为工作区启用池访问控制

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。

> [!NOTE]
>
> 访问控制仅在 [Azure Databricks Premium 计划](https://databricks.com/product/azure-pricing)中可用。

默认情况下，除非管理员启用池访问控制，否则所有用户均可创建和修改[池](../../clusters/instance-pools/index.md)。 使用池访问控制，用户的操作能力取决于权限。 本文介绍如何启用池访问控制。

有关分配权限和配置池访问控制的信息，请参阅[池访问控制](../../security/access-control/pool-acl.md)。

## <a name="enable-pool-access-control"></a>启用池访问控制

1. 转到[管理控制台](../admin-console.md)。
2. 选择“访问控制”选项卡。

   > [!div class="mx-imgBorder"]
   > ![“访问控制”选项卡](../../_static/images/admin-settings/access-control-tab-azure.png)

3. 单击“群集、池和作业访问控制”旁边的“启用”按钮 。

   > [!div class="mx-imgBorder"]
   > ![启用访问控制](../../_static/images/admin-settings/cluster-and-jobs-acls.png)

   > [!NOTE]
   >
   > 尽管该字段标记为“群集和作业访问控制”，但它也可以用于启用池访问控制。

4. 单击“确认”  。