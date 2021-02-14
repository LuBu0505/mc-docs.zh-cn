---
title: Azure Stack Hub MySQL 资源提供程序 1.1.93.0 发行说明
description: 查看发行说明以了解 Azure Stack Hub MySQL 资源提供程序 1.1.93.0 更新中的新增功能。
author: WenJason
ms.topic: article
origin.date: 09/22/2020
ms.date: 02/08/2021
ms.author: v-jay
ms.reviewer: xiaofmao
ms.lastreviewed: 09/22/2020
ms.openlocfilehash: 5ecdcbbf8ebef07f1ef3a56a197951140cc89902
ms.sourcegitcommit: 20bc732a6d267b44aafd953516fb2f5edb619454
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99503994"
---
# <a name="mysql-resource-provider-1193x-release-notes"></a>MySQL 资源提供程序 1.1.93.x 发行说明

本发行说明描述 MySQL 资源提供程序 1.1.93.x 版中的改进和已知问题。

## <a name="build-reference"></a>内部版本参考
下载 MySQL 资源提供程序二进制文件，然后运行自解压程序，将内容解压缩到一个临时目录。 资源提供程序具有相应的最低 Azure Stack Hub 版本。 下面列出了安装此 MySQL 资源提供程序版本所需的最低 Azure Stack Hub 发行版：

> |支持的 Azure Stack Hub 版本|MySQL 资源提供程序版本|
> |-----|-----|
> |版本 2008、2005|[MySQL RP 版本 1.1.93.1](https://aka.ms/azshmysqlrp11931)|  
> |     |     |

> [!IMPORTANT]
> 在部署最新版本的 MySQL 资源提供程序之前，请先将支持的最低 Azure Stack Hub 更新应用到 Azure Stack Hub 集成系统。

## <a name="new-features-and-fixes"></a>新功能和修复

此 Azure Stack Hub MySQL 资源提供程序版本包含以下改进和修复：

- 将基础 VM 更新到专用 Windows Server。 此 Windows Server 版本专用于 Azure Stack Hub Add-On RP Infrastructure，对租户市场不可见。 在部署或升级到此版本的 MySQL 资源提供程序之前，请确保先下载 Microsoft AzureStack 附加 RP Windows Server 映像。
- 支持删除孤立的数据库元数据和宿主服务器元数据。 如果无法再连接宿主服务器，租户可以选择从门户中删除孤立的数据库元数据。 当没有链接到宿主服务器的孤立数据库元数据时，操作员将能够从管理门户中删除孤立的宿主服务器元数据。
- 使 Make KeyVaultPfxPassword 成为执行机密轮换时可选的参数。 有关详细信息，请参阅[此文档](azure-stack-sql-resource-provider-maintain.md#secrets-rotation)。
- 其他 bug 修复。

建议在将 Azure Stack Hub 升级到 2005 版后应用 MySQL 资源提供程序 1.1.93.1。

## <a name="known-issues"></a>已知问题
如果使用了错误的 AzureRmContext，则部署 1.1.93.0 版本可能会失败。 建议直接升级到 1.1.93.1 版本。 如果已成功升级到 1.1.93.0，则可以安全地跳过 1.1.93.1 版本。

在已部署了相同版本的情况下重新部署 MySQL 资源提供程序时（例如，当已部署了 MySQL 资源提供程序 1.1.93.1 并再次部署同一版本时），托管 MySQL 资源提供程序的 VM 将停止。 若要解决此问题，请转到管理门户，在名为“system.\<region\>.mysqladapter”的资源组中找到并重启名为“mysqlvm\<version\>”的 VM。

## <a name="next-steps"></a>后续步骤

- [详细了解 MySQL 资源提供程序](azure-stack-mysql-resource-provider.md)。
- [准备部署 MySQL 资源提供程序](azure-stack-mysql-resource-provider-deploy.md#prerequisites)。
- [从旧版升级 MySQL 资源提供程序](azure-stack-mysql-resource-provider-update.md)。
