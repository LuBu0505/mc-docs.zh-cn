---
title: include 文件
description: include 文件
author: rockboyfor
ms.service: governance
ms.topic: include
origin.date: 03/26/2020
ms.date: 03/22/2021
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: aa5588383bc4e4bb1568e71ff2526c31bc4be1bf
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767109"
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
