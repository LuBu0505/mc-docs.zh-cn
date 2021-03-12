---
title: include 文件
description: include 文件
author: rockboyfor
ms.service: governance
ms.topic: include
origin.date: 03/26/2020
ms.date: 03/01/2021
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 0b4d191a54a1d206ab53e4b42a1f37645967b022
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102053329"
---
| 资源 | 限制 |
| --- | --- |
| 每个 Azure AD 租户的管理组数 | 10,000 |
| 每个管理组的订阅数 | 不受限制。 |
| 管理组层次结构的级别数 | 根级别加上 6 个级别<sup>1</sup> |
| 每个管理组的直接父管理组数 | 一种 |
| 每个位置的[管理组级别部署数](../articles/azure-resource-manager/templates/deploy-to-management-group.md) | 800<sup>2</sup> |

<sup>1</sup>6 个级别不包括订阅级别。

<sup>2</sup>如果达到 800 个部署的限制，则会从历史记录中删除不再需要的部署。 若要删除管理组级别部署，请使用 [Remove-AzManagementGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzManagementGroupDeployment) 或 [az deployment mg delete](https://docs.azure.cn/cli/deployment/mg#az_deployment_mg_delete)。
