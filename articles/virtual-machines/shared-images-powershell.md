---
title: 使用 Azure PowerShell 创建共享映像库
description: 了解如何使用 Azure PowerShell 在 Azure 中创建共享映像库
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
origin.date: 05/04/2020
author: rockboyfor
ms.date: 01/04/2021
ms.author: v-yeche
ms.reviewer: akjosh
ms.openlocfilehash: b5147af5a60ce361a985e148f85fb68c0131811f
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055313"
---
# <a name="create-a-shared-image-gallery-with-azure-powershell"></a>使用 Azure PowerShell 创建共享映像库 

[共享映像库](./shared-image-galleries.md)大大简化了整个组织中的自定义映像共享。 自定义映像类似于市场映像，不同的是自定义映像的创建者是自己。 自定义映像可用于启动部署任务，例如预加载应用程序、应用程序配置和其他 OS 配置。 

使用共享映像库，你可以在 AAD 租户内在同一区域或跨区域与组织中的其他用户共享自定义 VM 映像。 选择要共享哪些映像，要在哪些区域中共享，以及希望与谁共享它们。 你可以创建多个库，以便可以按逻辑方式对共享映像进行分组。 

库是顶级资源，它提供完整的 Azure 基于角色的访问控制 (Azure RBAC)。 你可以控制映像的版本，并且可以选择将每个映像版本复制到一组不同的 Azure 区域。 库仅适用于托管映像。

共享映像库功能具有多种资源类型。 

[!INCLUDE [virtual-machines-shared-image-gallery-resources](../../includes/virtual-machines-shared-image-gallery-resources.md)]

[!INCLUDE [virtual-machines-common-shared-images-powershell](../../includes/virtual-machines-common-shared-images-powershell.md)]

## <a name="next-steps"></a>后续步骤

从 [VM](image-version-vm-powershell.md)、[托管映像](image-version-managed-image-powershell.md)或[另一个库中的映像](image-version-another-gallery-powershell.md)创建映像。

<!--NOT AVAILABLE ON [Azure Image Builder (preview)](./image-builder-overview.md)-->
<!--NOT AVAILABLE ON [create a new image version from an existing image version](./windows/image-builder-gallery-update-image-version.md)-->

此外可以使用模板创建共享映像库资源。 提供多个 Azure 快速入门模板： 

- [创建共享映像库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sig-create/)
- [在共享的映像库中创建映像定义](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sig-image-definition-create/)
- [在共享映像库中创建映像版本](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sig-image-version-create/)
- [根据映像版本创建 VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-sig/)

<!--Update_Description: update meta properties, wording update, update link-->