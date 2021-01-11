---
title: 使用 REST 管理用户分配的托管标识 - Azure AD
description: 分步说明如何创建、列出和删除用户分配托管标识以进行 REST API 调用。
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/08/2021
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb671a81cecf01df4c3901d1e1d59b57ed985625
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98021738"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>使用 REST API 调用创建、列出或删除用户分配托管标识

Azure 资源托管标识使 Azure 服务能够向支持 Azure AD 身份验证的服务进行身份验证，而无需在代码中输入凭据。 

本文介绍如何使用 CURL 创建、列出和删除用户分配托管标识以进行 REST API 调用。

## <a name="prerequisites"></a>先决条件

- 如果不熟悉 Azure 资源的托管标识，请查阅[概述部分](overview.md)。 请务必了解[系统分配的托管标识与用户分配的托管标识之间的差异](overview.md#managed-identity-types)。
- 如果还没有 Azure 帐户，请先[注册试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)，然后再继续。
- 可在本地运行本文中的所有命令：
    - 若要在本地运行，请安装 [curl](https://curl.haxx.se/download.html) 和 [Azure CLI](/cli/install-azure-cli)。

## <a name="obtain-a-bearer-access-token"></a>获取持有者访问令牌

1. 如果在本地运行，请通过 Azure CLI 登录到 Azure：

    ```
    az login
    ```

1. 使用 [az account get-access-token](/cli/account#az_account_get_access_token) 获取访问令牌

    ```azurecli
    az account get-access-token
    ```

## <a name="create-a-user-assigned-managed-identity"></a>创建用户分配的托管标识 

若要创建用户分配的托管标识，你的帐户需要[托管标识参与者](../../role-based-access-control/built-in-roles.md#managed-identity-contributor)角色分配。

[!INCLUDE [ua-character-limit](../../../includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.chinacloudapi.cn/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
PUT https://management.chinacloudapi.cn/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```

**请求标头**

|请求标头  |说明  |
|---------|---------|
|*Content-Type*     | 必需。 设置为 `application/json`。        |
|*授权*     | 必需。 设置为有效的 `Bearer` 访问令牌。        |

**请求正文**

|名称  |说明  |
|---------|---------|
|location     | 必需。 资源位置。        |

## <a name="list-user-assigned-managed-identities"></a>列出用户分配的托管标识

若要列出/读取用户分配的托管标识，你的帐户需要[托管标识操作员](../../role-based-access-control/built-in-roles.md#managed-identity-operator)或[托管标识参与者](../../role-based-access-control/built-in-roles.md#managed-identity-contributor)角色分配。

```bash
curl 'https://management.chinacloudapi.cn/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
GET https://management.chinacloudapi.cn/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview HTTP/1.1
```

|请求标头  |说明  |
|---------|---------|
|*Content-Type*     | 必需。 设置为 `application/json`。        |
|*授权*     | 必需。 设置为有效的 `Bearer` 访问令牌。        |

## <a name="delete-a-user-assigned-managed-identity"></a>删除用户分配的托管标识

若要删除用户分配的托管标识，你的帐户需要[托管标识参与者](../../role-based-access-control/built-in-roles.md#managed-identity-contributor)角色分配。

> [!NOTE]
> 删除用户分配托管标识不会从将其分配到的任何资源中删除引用。 要使用 CURL 从 VM 中删除用户分配的托管标识，请参阅[从 Azure VM 中删除用户分配的标识](qs-configure-rest-vm.md#remove-a-user-assigned-managed-identity-from-an-azure-vm)。

```bash
curl 'https://management.chinacloudapi.cn/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
DELETE https://management.chinacloudapi.cn/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```
|请求标头  |说明  |
|---------|---------|
|*Content-Type*     | 必需。 设置为 `application/json`。        |
|*授权*     | 必需。 设置为有效的 `Bearer` 访问令牌。        |

## <a name="next-steps"></a>后续步骤

要了解如何使用 CURL 将用户分配托管标识分配给 Azure VM/VMSS，请参阅[使用 REST API 调用在 Azure VM 上配置 Azure 资源托管标识](qs-configure-rest-vm.md#user-assigned-managed-identity)和[使用 REST API 调用在虚拟机规模集上配置 Azure 资源托管标识](qs-configure-rest-vmss.md#user-assigned-managed-identity)。

