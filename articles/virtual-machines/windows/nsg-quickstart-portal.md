---
title: 使用 Azure 门户打开 VM 的端口
description: 了解如何使用 Azure 门户为 VM 打开端口/创建终结点
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
origin.date: 05/27/2020
author: rockboyfor
ms.date: 03/29/2021
ms.testscope: yes
ms.testdate: 11/02/2020
ms.author: v-yeche
ms.openlocfilehash: b02c2f06c578cae48aaa91393ff15b0468496f05
ms.sourcegitcommit: 1a64114f25dd71acba843bd7f1cd00c4df737ba4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105603794"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>如何使用 Azure 门户打开虚拟机端口
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="sign-in-to-azure"></a>登录 Azure
通过 https://portal.azure.cn 登录到 Azure 门户。

## <a name="create-a-network-security-group"></a>创建网络安全组

1. 搜索并选择 VM 的资源组，选择“添加”，然后搜索并选择“网络安全组”。

1. 选择“创建”。

    此时将打开“创建网络安全组”窗口。

    :::image type="content" source="./media/nsg-quickstart-portal/create-nsg.png" alt-text="创建网络安全组":::

1. 输入网络安全组的名称。 

1. 选择或创建一个资源组，然后选择位置。

1. 选择“创建”以创建网络安全组。

## <a name="create-an-inbound-security-rule"></a>创建入站安全规则

1. 选择新的网络安全组。 

1. 从左侧菜单中选择“入站安全规则”，并选择“添加”。

    :::image type="content" source="./media/nsg-quickstart-portal/advanced.png" alt-text="切换到“高级”页":::

1. 在“添加入站安全规则”页中，切换到页面顶部“基本”中的“高级”。   

1. 从下拉菜单中选择常见的“服务”，如 HTTP 。 如果要提供要使用的特定端口，也可以选择“自定义”。 

1. （可选）更改 **优先级** 或 **名称**。 优先级会影响应用规则的顺序：数值越小，越先应用规则。

1. 选择“添加”以创建规则。

## <a name="associate-your-network-security-group-with-a-subnet"></a>将网络安全组与子网相关联

最后一步是将网络安全组与子网或特定网络接口相关联。 对于此示例，我们将网络安全组与子网相关联。 

1. 从左侧菜单选择“子网”，然后选择“关联”。

1. 选择虚拟网络，并选择相应的子网。

    :::image type="content" source="./media/nsg-quickstart-portal/select-vnet-subnet.png" alt-text="将网络安全组与虚拟网络相关联":::

1. 完成后，请选择“确定”。

## <a name="additional-information"></a>其他信息

也可以[使用 Azure PowerShell 执行本文中的步骤](nsg-quickstart-powershell.md)。

使用本文中介绍的命令可以快速获取流向 VM 的流量。 网络安全组提供许多出色的功能和粒度来控制对资源的访问。 有关详细信息，请参阅[使用网络安全组筛选网络流量](../../virtual-network/tutorial-filter-network-traffic.md)。

对于高可用性 Web 应用程序，请考虑将 VM 放在 Azure 负载均衡器后面。 当负载均衡器向 VM 分配流量时，网络安全组可以筛选流量。 有关详细信息，请参阅[在 Azure 中均衡 Windows 虚拟机负载以创建高可用性应用程序](tutorial-load-balancer.md)。

## <a name="next-steps"></a>后续步骤
在本文中，已经创建了网络安全组、创建了允许端口 80 上的 HTTP 流量的入站规则，并将该规则与子网进行了关联。 

下列文章更介绍了有关创建更详细环境的信息：
- [Azure Resource Manager 概述](../../azure-resource-manager/management/overview.md)
- [安全组](../../virtual-network/network-security-groups-overview.md)

<!--Update_Description: update meta properties, wording update, update link-->