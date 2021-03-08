---
title: 如何使用 CLI 标记 Azure 虚拟机
description: 了解如何使用 Azure CLI 标记虚拟机。
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure-services
origin.date: 11/11/2020
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 01/04/2021
ms.author: v-yeche
ms.custom: devx-track-azurecli
ms.openlocfilehash: efef4b73312537b6bac4b318dd3c28cdf72fe2f1
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054371"
---
<!--Verified successfully-->
# <a name="how-to-tag-a-vm-using-the-cli"></a>如何使用 CLI 来标记 VM

本文介绍如何使用 Azure CLI 来标记 VM。 标记是用户定义的键/值对，可直接放置在资源或资源组中。 针对每个资源和资源组，Azure 当前支持最多 50 个标记。 标记可以在创建时放置在资源中或添加到现有资源中。 还可以使用 Azure [PowerShell](tag-powershell.md) 来标记虚拟机。

可以使用 `az vm show` 查看给定 VM 的所有属性，包括标记。

```azurecli
az vm show --resource-group myResourceGroup --name myVM --query tags
```

若要通过 Azure CLI 添加新的 VM 标记，可以使用 `azure vm update` 命令以及标记参数 `--set`：

```azurecli
az vm update \
    --resource-group myResourceGroup \
    --name myVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

若要删除标记，可以在 `azure vm update` 命令中使用 `--remove` 参数。

```azurecli
az vm update \
   --resource-group myResourceGroup \
   --name myVM \
   --remove tags.myNewTagName1
```

既然我们已通过 Azure CLI 和门户将标记应用到资源中，那就让我们看一看使用情况详细信息，以在计费门户中的查看标记。

### <a name="next-steps"></a>后续步骤

- 若要详细了解如何标记 Azure 资源，请参阅 [Azure 资源管理器概述](../azure-resource-manager/management/overview.md)和 [使用标记来组织 Azure 资源](../azure-resource-manager/management/tag-resources.md)。

<!--NOT AVAILABLE ON [Understanding your Azure Bill](../cost-management-billing/understand/review-individual-bill.md)-->
<!--Update_Description: update meta properties, wording update, update link-->