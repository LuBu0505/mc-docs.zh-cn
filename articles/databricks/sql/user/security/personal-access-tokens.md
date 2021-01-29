---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 个人访问令牌 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 个人访问令牌。
ms.openlocfilehash: 1617ed43ff4d4264fc81ac3ee072261bd99228fc
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692278"
---
# <a name="personal-access-tokens"></a>个人访问令牌

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

可使用个人访问令牌对 Databricks [REST API](../../api/authentication.md) 和 [BI 工具](../../../integrations/bi/jdbc-odbc-bi.md#authentication)进行身份验证。

每个用户的个人访问令牌数限制为每个 Azure Databricks [工作区](../../../workspace/index.md) 600 个。

## <a name="generate-a-personal-access-token"></a><a id="generate"> </a><a id="generate-a-personal-access-token"> </a>生成个人访问令牌

1. 单击边栏底部的![用户设置图标](../../../_static/images/icons/user-settings-icon.png)图标，然后选择“设置”。
2. 单击“个人访问令牌”选项卡。
3. 单击“+ 生成新令牌”。
4. 可以选择输入注释并修改令牌生存期。
5. 单击“生成”  。
6. 单击 ![复制图标](../../../_static/images/sql/copy-icon.png) 以复制令牌，然后单击“确定”。