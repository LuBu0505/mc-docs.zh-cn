---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 03/18/2021
ms.author: v-chazhou
ms.openlocfilehash: 1c1ea953d667b3b9ef395bf97caf3a54f2cac600
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766563"
---
让我们创建一个名为 mySecret 的机密，其值为 Success!。 机密可以是密码、SQL 连接字符串，或者需要安全保存的、可供应用程序使用的其他任何信息。 

若要将机密添加到新创建的 Key Vault，请使用 Azure CLI [az keyvault secret set](/cli/keyvault/secret#az-keyvault-secret-set) 命令：

```azurecli
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "mySecret" --value "Success!"
```
