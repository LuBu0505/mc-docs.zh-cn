---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
ms.date: 12/16/2020
title: 无法应用更新的群集策略 - Azure Databricks
description: 对现有群集策略进行更新时，除非在删除策略后再重新添加它，否则不会应用更新。
category: Clusters
author: nadroj94
db-author: jordan.hicks@databricks.com
ms.openlocfilehash: 8fc10feca56524b3ad5d1b768eadc3886317b9a6
ms.sourcegitcommit: 136164cd330eb9323fe21fd1856d5671b2f001de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197011"
---
# <a name="cannot-apply-updated-cluster-policy"></a>无法应用更新的群集策略

## <a name="problem"></a>问题

你在尝试更新现有的[群集策略](/databricks/clusters/configure#cluster-policy)，但该更新未应用到与策略关联的群集。 如果尝试编辑由策略管理的群集，则不会应用或保存所做的更改。

## <a name="cause"></a>原因

这是一个已知问题，我们正在解决它。

## <a name="solution"></a>解决方案

在永久性修补程序可用之前，你可以使用暂时性的解决方法。

1. 编辑群集策略。
2. 将策略的属性重新设置为“自由格式”。
3. 将编辑后的策略添加回群集。

如果要编辑与策略关联的群集，请执行以下操作：

1. 终止群集。
2. 将另一策略关联到群集。
3. 编辑群集。
4. 将原始策略重新关联到群集。