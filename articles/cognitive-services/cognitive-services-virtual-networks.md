---
title: 虚拟网络
titleSuffix: Azure Cognitive Services
description: 为认知服务资源配置分层网络安全。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 01/19/2021
ms.author: v-johya
ms.openlocfilehash: 8580fe98f3a4072cbf5ad38b6d4a13f733f706de
ms.sourcegitcommit: 102a21dc30622e4827cc005bdf71ade772c1b8de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98751376"
---
# <a name="configure-azure-cognitive-services-virtual-networks"></a>配置 Azure 认知服务虚拟网络

Azure 认知服务提供了分层的安全模型。 借助此模型，可保护认知服务帐户，使其可供网络的特定子集访问。 配置网络规则后，仅通过指定网络集请求数据的应用程序才能访问帐户。 可以使用请求筛选来限制对资源的访问。 只允许来自指定的 IP 地址、IP 范围或 [Azure 虚拟网络](../virtual-network/virtual-networks-overview.md)中一系列子网的请求。

当网络规则生效时访问认知服务资源的应用程序需要授权。 使用 [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) 凭据或使用有效的 API 密钥支持授权。

> [!IMPORTANT]
> 默认情况下，为认知服务帐户启用防火墙规则会阻止传入的数据请求。 为了允许请求通过，需要满足以下条件之一：

> * 该请求应来自目标认知服务帐户的允许子网列表上的 Azure 虚拟网络 (VNet) 中运行的服务。 来自 VNet 的请求中的终结点需要设置为认知服务帐户的[自定义子域](cognitive-services-custom-subdomains.md)。
> * 或者请求应来自一系列允许的 IP 地址。
>
> 被阻止的请求包括来自其他 Azure 服务、来自 Azure 门户、来自日志记录和指标服务等的请求。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenarios"></a>方案

若要保护认知服务资源，应该先配置一个规则，以便在默认情况下拒绝来自所有网络的流量（包括 Internet 流量）进行访问。 然后，应配置允许访问特定 vnet 流量的规则。 借助此配置，可为应用程序生成安全网络边界。 还可以配置规则以授予对来自所选公共 internet IP 地址范围的流量的访问权限，从而支持来自特定 internet 或本地客户端的连接。

对于面向 Azure 认知服务的所有网络协议（包括 REST 和 WebSocket），将强制实施网络规则。 若要使用 Azure 测试控制台等工具访问数据，必须配置显式网络规则。 可以将网络规则应用于现有的认知服务资源，也可以在创建新的认知服务资源时应用。 一旦应用网络规则，就会对所有请求强制实施这些规则。

## <a name="supported-regions-and-service-offerings"></a>支持的区域和服务产品

虚拟网络 (VNET) 在[认知服务可用的区域](https://azure.microsoft.com/global-infrastructure/services/)中受支持。 认知服务支持网络规则配置的服务标记。 下面列出的服务包含在 CognitiveServicesManagement 服务标记中。

> [!div class="checklist"]
> * 计算机视觉
> * 内容审查器
> * 自定义视觉
> * 人脸
> * 语言理解 (LUIS)
> * 文本分析
> * 文本翻译


> [!NOTE]
> 如果使用的是 LUIS，CognitiveServicesManagement 标记只允许通过 SDK 或 REST API 使用服务。 若要从虚拟网络访问和使用 LUIS 门户，需要使用以下标记：  
> * **AzureActiveDirectory**
> * **AzureFrontDoor.Frontend**
> * **AzureResourceManager** 
> * **CognitiveServicesManagement**



## <a name="change-the-default-network-access-rule"></a>更改默认网络访问规则

默认情况下，认知服务资源接受来自任何网络上客户端的连接。 若要限制为仅允许选定网络访问，必须先更改默认操作。

> [!WARNING]
> 更改网络规则可能会使应用程序无法正常连接到 Azure 认知服务。 除非还应用了 **授予** 访问权限的特定网络规则，否则将默认网络规则设置为“拒绝”会阻止对数据的所有访问。 在将默认规则更改为拒绝访问之前，务必先使用网络规则对所有许可网络授予访问权限。 如果允许列出本地网络的 IP 地址，请确保添加本地网络中所有可能的传出公共 IP 地址。

### <a name="managing-default-network-access-rules"></a>管理默认网络访问规则

可以通过 Azure 门户、PowerShell 或 Azure CLI 管理认知服务资源的默认网络访问规则。

# <a name="azure-portal"></a>[Azure 门户](#tab/portal)

1. 转到要保护的认知服务资源。

1. 选择名为“虚拟网络”的“资源管理”菜单 。

   ![虚拟网络选项](./media/vnet/virtual-network-blade.png)

1. 若要默认拒绝访问，请选择允许从“所选网络”进行访问。 只有“所选网络”设置，而没有配置的“虚拟网络”或“地址范围”，则所有访问都将被有效拒绝  。 当所有访问都被拒绝时，就不会允许试图使用认知服务资源的请求。 Azure 门户、Azure PowerShell 或 Azure CLI 仍可用于配置认知服务资源。
1. 若要允许来自所有网络的流量，请选择允许从“所有网络”进行访问。

   ![虚拟网络拒绝](./media/vnet/virtual-network-deny.png)

1. 单击“保存”应用所做的更改。

# <a name="powershell"></a>[PowerShell](#tab/powershell)

1. 安装 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) 并[登录](https://docs.microsoft.com/powershell/azure/authenticate-azureps)，或选择“试用”。

1. 显示认知服务资源默认规则的状态。

    ```azurepowershell
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
    }
    (Get-AzCognitiveServicesAccountNetworkRuleSet @parameters).DefaultAction
    ```

1. 将默认规则设置为默认拒绝网络访问。

    ```azurepowershell
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -DefaultAction Deny
    }
    Update-AzCognitiveServicesAccountNetworkRuleSet @parameters
    ```

1. 将默认规则设置为默认允许网络访问。

    ```azurepowershell
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -DefaultAction Allow
    }
    Update-AzCognitiveServicesAccountNetworkRuleSet @parameters
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. 安装 [Azure CLI](/cli/install-azure-cli) 并[登录](/cli/authenticate-azure-cli)，或选择“试用”。

1. 显示认知服务资源默认规则的状态。

    ```azurecli
    az cognitiveservices account show \
        -g "myresourcegroup" -n "myaccount" \
        --query networkRuleSet.defaultAction
    ```

1. 将默认规则设置为默认拒绝网络访问。

    ```azurecli
    az cognitiveservices account update \
        -g "myresourcegroup" -n "myaccount" \
        --default-action Deny
    ```

1. 将默认规则设置为默认允许网络访问。

    ```azurecli
    az cognitiveservices account update \
        -g "myresourcegroup" -n "myaccount" \
        --default-action Allow
    ```


## <a name="next-steps"></a>后续步骤

* 探索各种 [Azure 认知服务](./what-are-cognitive-services.md)
* 详细了解 [Azure 虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)

