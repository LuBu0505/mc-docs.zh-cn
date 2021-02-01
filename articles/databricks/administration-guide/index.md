---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/08/2020
title: 管理指南 - Azure Databricks
description: 了解如何管理 Azure Databricks。
ms.openlocfilehash: 28ee34bc017dfad86332fe1e7b155cc0c6cc5145
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059979"
---
# <a name="administration-guide"></a><a id="administration"> </a><a id="administration-guide"> </a>管理指南

若要管理 Azure Databricks 服务，需要分配多种不同身份的管理员：

* 具有 Azure“参与者”或“所有者”角色的用户，该用户可以查看和更改 Azure Databricks 服务、Azure 订阅以及诊断日志配置。  注册或创建了 Azure Databricks 服务的人员通常具有这些角色中的一个。
* **Azure Databricks 管理员**，他们可以管理用户和组（包括单一登录、预配和访问控制）以及工作区存储。 可以在帐户中分配任意数量的管理员，而管理员可将某些管理任务委托给非管理员用户（例如，为了进行群集管理）。 大多数 Azure Databricks 管理任务都是使用[管理控制台](admin-console.md)执行的。

  Azure Databricks 管理员是 `admin` 组的成员。 若要为用户授予管理员权限，请使用[管理控制台](admin-console.md)、[组 API](../dev-tools/api/latest/groups.md)、[SCIM API](../dev-tools/api/latest/scim/index.md) 或[启用了 SCIM 的标识提供者](users-groups/scim/index.md)将用户添加到 `admin` 组。

* **Azure Active Directory 管理员**，他们有权启用 Azure Active Directory [条件访问](access-control/conditional-access.md)。

本指南的内容：

* [管理 Azure Databricks 帐户](account-settings/index.md)
* [访问管理控制台](admin-console.md)
* [管理用户和组](users-groups/index.md)
* [启用访问控制](access-control/index.md)
* [管理工作区对象和行为](workspace/index.md)
* [管理群集配置选项](clusters/index.md)
* [管理虚拟网络](cloud-configurations/azure/index.md)
* [灾难恢复](disaster-recovery.md)