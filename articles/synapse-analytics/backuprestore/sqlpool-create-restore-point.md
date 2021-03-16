---
title: 为专用 SQL 池创建用户定义的还原点
description: 了解如何使用 Azure 门户在 Azure Synapse Analytics 中为专用 SQL 池创建用户定义的还原点。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
origin.date: 10/29/2020
ms.date: 03/08/2021
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 0358052adfc3743d4494d3159186a1eb1048ea68
ms.sourcegitcommit: 5707919d0754df9dd9543a6d8e6525774af738a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102207615"
---
# <a name="user-defined-restore-points"></a>用户定义的还原点

本文介绍了如何使用 Azure 门户在 Azure Synapse Analytics 中为专用 SQL 池新建用户定义的还原点。

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>通过 Azure 门户创建用户定义的还原点

还可以通过 Azure 门户创建用户定义的还原点。

1. 登录到 [Azure 门户](https://portal.azure.cn/)帐户。

2. 导航到要为其创建还原点的专用 SQL 池。

3. 从左窗格中选择“概述”，选择“+ 新建还原点”。 如果“新建还原点”按钮未启用，请确保专用 SQL 池未暂停。

    ![新建还原点](../media/sql-pools/create-sqlpool-restore-point-01.png)

4. 为用户定义的还原点指定名称，然后单击“应用”  。 用户定义的还原点的默认保留期为七天。

    ![还原点的名称](../media/sql-pools/create-sqlpool-restore-point-02.png)

## <a name="next-steps"></a>后续步骤

- [还原现有专用 SQL 池](restore-sql-pool.md)

