---
title: 什么是 Azure 防火墙管理器安全合作伙伴提供程序？
description: 了解 Azure 防火墙管理器安全合作伙伴提供程序
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
origin.date: 06/30/2020
ms.date: 12/28/2020
ms.author: victorh
ms.openlocfilehash: 7496a18f4c9d411577f5273de60304c8083af60f
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023247"
---
# <a name="what-are-security-partner-providers"></a>什么是安全合作伙伴提供程序？

通过 Azure 防火墙管理器中的安全合作伙伴提供程序，你可使用熟悉的同类最佳第三方安全即服务 (SECaaS) 产品/服务来保护用户的 Internet 访问。

通过快速配置，你可以使用受支持的安全合作伙伴保护中心的安全，并从虚拟网络 (VNet) 或区域中的分支位置路由和筛选 Internet 流量。 可使用自动化路由管理来做到这一点，无需设置和管理用户定义的路由 (UDR)。

可以在多个 Azure 区域中部署配置有所选安全合作伙伴的安全中心，使全球任何地方的用户可以在这些区域中进行连接并获得安全性。 通过将安全合作伙伴的产品/服务用于 Internet/SaaS 应用程序流量并将 Azure 防火墙用于安全中心中的专用流量，现在，你可以开始在 Azure 上构建与全球分布的用户和应用程序非常接近的安全边缘。

受支持的安全合作伙伴有 Zscaler、[Check Point](check-point-overview.md) 和 iboss   。

![安全合作伙伴提供程序](media/trusted-security-partners/trusted-security-partners.png)

## <a name="key-scenarios"></a>关键方案

可以在以下方案中使用安全合作伙伴来筛选 Internet 流量：

- 虚拟网络 (VNet) 到 Internet

   为在 Azure 上运行的云工作负荷提供高级用户感知型 Internet 保护。

- 分支到 Internet

   利用 Azure 连接和全球分布，轻松为分支到 Internet 方案添加第三方 NSaaS 筛选。 可以使用 Azure 虚拟 WAN 构建全球传输网络和安全边缘。

支持以下方案：
- 通过安全合作伙伴提供程序实现 VNet/分支到 Internet 以及通过 Azure 防火墙到其他流量（辐射到辐射、辐射到分支、分支到辐射）。
- 通过安全合作伙伴提供程序实现 VNet/分支到 Internet

## <a name="best-practices-for-internet-traffic-filtering-in-secured-virtual-hubs"></a>安全虚拟中心中筛选 Internet 流量的最佳做法

Internet 流量通常包括 Web 流量。 但它也包括传到 Microsoft 365 等 SaaS 应用程序以及 Azure 公共 PaaS 服务（例如 Azure 存储和 Azure Sql 等）的流量。 以下是处理传到这些服务的流量的最佳做法建议：

### <a name="handling-azure-paas-traffic"></a>处理 Azure PaaS 流量
 
- 如果流量主要包含 Azure PaaS，并且可以使用 IP 地址、FQDN、服务标记或 FQDN 标记来筛选针对应用程序的资源访问，请使用 Azure 防火墙进行保护。

- 如果流量包含对 SaaS 应用程序的访问，或者你需要用户感知的筛选（例如，对于你的虚拟桌面基础结构 (VDI) 工作负荷），或者你需要高级 Internet 筛选功能，请在你的中心中使用第三方合作伙伴解决方案。

![适用于 Azure 防火墙管理器的所有方案](media/trusted-security-partners/all-scenarios.png)

## <a name="handling-microsoft-365-traffic"></a>处理 Microsoft 365 流量

在全球分布的分支位置方案中，你应该直接在分支处重定向 Microsoft 365 流量，然后再将剩余的 Internet 流量发送到 Azure 安全中心。

对于 Microsoft 365 而言，网络延迟和性能对于成功的用户体验至关重要。 若要围绕最佳性能和用户体验实现这些目标，客户必须首先实现 Microsoft 365 直接和本地转义，然后再考虑通过 Azure 路由剩余的 Internet 流量。

<!--[Microsoft 365 network connectivity principles](https://docs.azure.cn/microsoft-365/enterprise/microsoft-365-network-connectivity-principles)-->

此外，Microsoft 365 连接经过了加密以保护隐私，并且使用有效的专用协议来保障性能。 这使得将这些连接受制于传统的网络级别安全解决方案不切实际并且会造成影响。 出于这些原因，我们强烈建议客户首先直接从分支发送 Microsoft 365 流量，然后再通过 Azure 发送剩余流量。 Microsoft 与多个 SD-WAN 解决方案提供商建立了合作伙伴关系，这些提供商与 Azure 和 Microsoft 365 集成，让客户能够轻松启用 Microsoft 365 直接和本地 Internet 突破。 请参阅[什么是 Azure 虚拟 WAN？](../virtual-wan/virtual-wan-about.md)了解详情

## <a name="next-steps"></a>后续步骤

[使用 Azure 防火墙管理器在安全中心中部署安全合作伙伴产品/服务](deploy-trusted-security-partner.md)。
