---
title: 将云服务部署到 Azure 时对 ConstrainedAllocationFailed 进行故障排除 | Microsoft Docs
description: 本文介绍了如何在将云服务部署到 Azure 时解决 ConstrainedAllocationFailed 异常。
services: cloud-services
author: mibufo
ms.author: v-junlch
ms.service: cloud-services
ms.topic: troubleshooting
ms.date: 02/26/2021
ms.openlocfilehash: 41b7e8592d96417fbf95cc38bceb1d6e6aafa1a9
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751885"
---
# <a name="troubleshoot-constrainedallocationfailed-when-deploying-a-cloud-service-to-azure"></a>将云服务部署到 Azure 时对 ConstrainedAllocationFailed 进行故障排除

在本文中，你将解决由于约束而无法部署 Azure 云服务的分配失败。

在以下情况下，Azure 会进行分配：

- 升级云服务实例

- 添加新的 Web 或辅助角色实例

- 将实例部署到云服务

在执行这些操作时，甚至在达到 Azure 订阅限制之前，有时可能会收到错误。

> [!TIP]
> 规划服务的部署时，本信息可能也有用。

## <a name="symptom"></a>症状

在 Azure 门户中，导航到你的云服务，并在侧栏中选择“操作日志(经典)”来查看日志。

检查云服务的日志时，你将看到以下异常：

|异常类型  |错误消息  |
|---------|---------|
|ConstrainedAllocationFailed     |Azure 操作“`{Operation ID}`”失败，代码为 Compute.ConstrainedAllocationFailed。 详细信息：分配失败；无法满足请求中的约束。 请求的新服务部署绑定至地缘组，或以虚拟网络为目标，或此托管服务下已经有部署。 上述任一情况都会将新的部署局限于特定的 Azure 资源。 请稍后重试，或尝试减小 VM 大小或角色实例数目。 或者，可能的话，删除先前提到的约束，或尝试部署到不同的区域。|

## <a name="cause"></a>原因

你正在向其中进行部署的区域或群集存在容量问题。 当所选的资源 SKU 不适用于指定的位置时，会发生此问题。

> [!NOTE]
> 在部署云服务的第一个节点时，该节点被固定到一个资源池。 资源池可以是单个群集，也可以是一组群集。
>
> 随着时间的推移，此资源池中的资源可能会被完全利用。 当固定的资源池中没有足够的资源可用时，如果云服务发出分配请求来请求更多资源，则该请求将导致[分配失败](cloud-services-allocation-failures.md)。

## <a name="solution"></a>解决方案

在这种情况下，你应该选择一个不同的区域或 SKU 来部署云服务。 在部署或升级云服务之前，你可以确定某个区域或可用性区域中有哪些 SKU 可用。 请遵循下面的 [Azure CLI](#list-skus-in-region-using-azure-cli)、[PowerShell](#list-skus-in-region-using-powershell) 或 [REST API](#list-skus-in-region-using-rest-api) 过程。

### <a name="list-skus-in-region-using-azure-cli"></a>使用 Azure CLI 列出区域中的 SKU

你可以使用 [az vm list-skus](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az_vm_list_skus) 命令。

- 使用 `--location` 参数将输出筛选为你正在使用的位置。
- 使用 `--size` 参数按部分大小名称搜索。
- 有关详细信息，请参阅[解决有关 SKU 不可用的错误](../azure-resource-manager/templates/error-sku-not-available.md#solution-2---azure-cli)指南。

    例如：

    ```azurecli
    az vm list-skus --location chinanorth --size Standard_F --output table
    ```

#### <a name="list-skus-in-region-using-powershell"></a>使用 PowerShell 列出区域中的 SKU

你可以使用 [Get-AzComputeResourceSku](https://docs.microsoft.com/powershell/module/az.compute/get-azcomputeresourcesku) 命令。

- 按位置筛选结果。
- 必须拥有最新版本 PowerShell 才能运行此命令。
- 有关详细信息，请参阅[解决有关 SKU 不可用的错误](../azure-resource-manager/templates/error-sku-not-available.md#solution-1---powershell)指南。

例如：

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations -icontains "chinanorth"}
```

**一些有用的其他命令：**

筛选出包含大小 (Standard_DS14_v2) 的位置：

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("chinanorth") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("Standard_DS14_v2")}
```

筛选出包含大小 (V3) 的所有位置：

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("chinanorth") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("v3")} | fc
```

#### <a name="list-skus-in-region-using-rest-api"></a>使用 REST API 列出区域中的 SKU

你可以使用[资源 SKU - 列出](https://docs.microsoft.com/rest/api/compute/resourceskus/list)操作。 它会以下列格式返回可用 SKU 和区域：

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "chinanorth"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "chinanorth"
      ],
      "restrictions": []
    },
    <Rest_of_your_file_is_located_here...>
  ]
}
    
```

## <a name="next-steps"></a>后续步骤

若要了解更多的分配失败解决方案，并更好地了解它们是如何生成的，请执行以下操作：

> [!div class="nextstepaction"]
> [分配失败（云服务）](cloud-services-allocation-failures.md)

如果你有本文未解决的 Azure 问题，请访问 [MSDN 和 Stack Overflow](https://www.azure.cn/support/forums/) 上的 Azure 论坛。 可以在这些论坛上发布问题。 还可提交 Azure 支持请求。 若要提交支持请求，请在 [Azure 支持](https://www.azure.cn/support/contact/)页上，选择“获取支持”。
