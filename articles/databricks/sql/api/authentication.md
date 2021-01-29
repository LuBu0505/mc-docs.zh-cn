---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 使用 Azure Databricks 个人访问令牌进行身份验证 - Azure Databricks
description: 了解如何使用 Azure Databricks 个人访问令牌进行身份验证并访问 Databricks REST API。
ms.openlocfilehash: 010f98b06dcfc11816d3c3a456c3fb9fbd3f3e65
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692330"
---
# <a name="authentication-using-azure-databricks-personal-access-tokens"></a>使用 Azure Databricks 个人访问令牌进行身份验证

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

若要对 Databricks REST API 进行身份验证和访问，可使用 Azure Databricks 个人访问令牌或 Azure Active Directory (Azure AD) 令牌。

本文介绍如何使用 Azure Databricks 个人访问令牌。 有关 Azure AD 令牌，请参阅[使用 Azure Active Directory 令牌进行身份验证](../../dev-tools/api/latest/aad/index.md)。

## <a name="generate-a-personal-access-token"></a><a id="generate-a-personal-access-token"> </a><a id="token-management"> </a>生成个人访问令牌

请参阅[个人访问令牌](../user/security/personal-access-tokens.md)。

## <a name="use-a-personal-access-token-to-access-the-databricks-rest-api"></a><a id="netrc"> </a><a id="use-a-personal-access-token-to-access-the-databricks-rest-api"> </a>使用个人访问令牌访问 Databricks REST API

可在 ``.netrc`` 中存储个人访问令牌并在 ``curl`` 中使用，也可将其传递到 ``Authorization: Bearer`` 标头。

### <a name="store-token-in-netrc-file-and-use-in-curl"></a>在 ``.netrc`` 文件中存储令牌并在 ``curl`` 中使用

使用 ``machine``、``login`` 和 ``password`` 属性创建 [.netrc](https://ec.haxx.se/usingcurl-netrc.html) 文件：

```ini
machine <databricks-instance>
login token
password <personal-access-token>
```

其中：

* ``<databricks-instance>`` 是 Azure Databricks 部署的[工作区 URL](../../workspace/workspace-details.md#workspace-url)。
* ``token`` 是文本字符串 ``token``
* ``<personal-access-token>`` 是个人访问令牌的值。

若要调用 ``.netrc`` 文件，请在 ``curl`` 命令中使用 ``-n``：

```bash
curl -n -X GET https://<databricks-instance>/api/2.0/sql/endpoints/get?id=<endpoint-id>
```

### <a name="pass-token-to-bearer-authentication"></a><a id="bearer"> </a><a id="pass-token-to-bearer-authentication"> </a>将令牌传递到 ``Bearer`` 身份验证

可使用 ``Bearer`` 身份验证将令牌包含在标头中，

```bash
curl -X GET -H 'Authorization: Bearer <personal-access-token>' https://<databricks-instance>/api/2.0/sql/endpoints/list
```