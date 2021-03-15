---
title: 重置帐户凭据 - CLI
description: 使用 Azure CLI 脚本重置帐户凭据和恢复 app.config 设置。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 08/31/2020
ms.date: 03/15/2021
ms.author: v-jay
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8024cb43fd98d7318bf6f171cdd43cc1155a1cbd
ms.sourcegitcommit: 5f85f27bd5d62ffb4913b9b9bd86cc41b3dfbf06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103211792"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLI 示例：重置帐户凭据

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

本文中的 Azure CLI 脚本演示如何重置帐户凭据和恢复 app.config 设置。

## <a name="prerequisites"></a>先决条件

[创建媒体服务帐户](./create-account-howto.md)。

## <a name="example-script"></a>示例脚本

```azurecli
# Update the following variables for your own settings:
$resourceGroup="amsResourceGroup"
$amsAccountName="amsmediaaccountname"

az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --resource-group $resourceGroup
```

## <a name="next-steps"></a>后续步骤

* [az ams](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [重置凭据](https://docs.microsoft.com/cli/azure/ams/account/sp#az-ams-account-sp-reset-credentials)
