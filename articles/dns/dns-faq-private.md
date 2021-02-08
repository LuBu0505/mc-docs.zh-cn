---
title: Azure 专用 DNS 常见问题解答
description: 本文介绍 Azure 专用 DNS 的常见问题
services: dns
author: WenJason
ms.service: dns
ms.topic: article
origin.date: 01/15/2021
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: 66201f8b9d5c6db1ab21312b46d962bd257edf89
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060056"
---
# <a name="azure-private-dns-faq"></a>Azure 专用 DNS 常见问题解答

下面是有关 Azure 专用 DNS 的常见问题。

## <a name="does-azure-dns-support-private-domains"></a>Azure DNS 是否支持专用域？

使用 Azure 专用 DNS 区域功能支持专用域。 专用 DNS 区域只能从指定的虚拟网络中进行解析。 有关详细信息，请参阅[概述](private-dns-overview.md)。

有关 Azure 中其他内部 DNS 选项的信息，请参阅 [VM 和角色实例的名称解析](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)。

## <a name="will-azure-private-dns-zones-work-across-azure-regions"></a>Azure 专用 DNS 区域是否可跨 Azure 区域工作？

是的。 专用区域支持在跨 Azure 区域的虚拟网络之间进行 DNS 解析。 即使未显式建立虚拟网络对等互连，专用区域也能正常工作。 所有虚拟网络都必须链接到专用 DNS 区域。

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>专用区域是否需要在虚拟网络与 Internet 之间建立连接？

不是。 专用区域配合虚拟网络工作。 你可在虚拟网络内部或跨虚拟网络管理虚拟机的域或其他资源。 无需建立 Internet 连接即可进行名称解析。

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>是否可将同一专用区域用于解析多个虚拟网络？

是的。 可以将专用 DNS 区域链接到成千上万个虚拟网络。 有关详细信息，请参阅 [Azure DNS 限制](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-dns-limits)

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>删除虚拟机后，是否会自动删除在专用区域中自动注册的虚拟机 DNS 记录？

是的。 如果在已启用自动注册的情况下删除链接的虚拟网络中的虚拟机，则会自动删除已注册的记录。

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>是否可以手动删除从链接的虚拟网络的专用区域中自动注册的虚拟机记录？

是的。 可以在区域中使用手动创建的 DNS 记录来覆盖此类自动注册的 DNS 记录。 以下问答部分解答了此主题。

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>如果尝试将新的 DNS 记录手动创建到专用区域，且该专有区域与链接的虚拟网络中的自动注册的现有虚拟机具有相同主机名时，会发生什么情况？

如果尝试将新的 DNS 记录手动创建到专用区域，且该专有区域与链接的虚拟网络中的自动注册的现有虚拟机具有相同主机名， 则新的 DNS 记录会覆盖自动注册的虚拟机记录。 如果再次尝试从区域中删除这条手动创建的 DNS 记录，则删除操作将会成功。 只要虚拟机仍然存在并且其上已附加专用 IP，就会再次发生自动注册。 DNS 记录将在区域中自动重新创建。

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>从专用区域中取消已链接虚拟网络的链接时会发生什么情况？ 虚拟网络中自动注册的虚拟机记录是否也会从区域中删除？

是的。 若要从专用区域中取消已链接虚拟网络的链接，请更新 DNS 区域以删除关联的虚拟网络链接。 在此过程中，自动注册的虚拟机记录将从区域中删除。

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>删除链接到专用区域的链接虚拟网络时会发生什么情况？ 是否必须手动更新专用区域，才能从该区域中取消已链接虚拟网络的链接？

不是。 如果删除链接的虚拟网络而不先将其从专用区域中取消链接，则删除操作将会成功，并且会自动清除对 DNS 区域的链接。

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalchinacloudappcn-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>是否即使专用区域（例如 private.contoso.com）已链接到虚拟网络，也仍可使用默认 FQDN (internal.chinacloudapp.cn) 进行 DNS 解析？

是的。 专用区域不会替换默认的 Azure 提供的 internal.chinacloudapp.cn 区域。 无论是依赖于 Azure 提供的 internal.chinacloudapp.cn 还是依赖于自己的专用区域，都请使用要解析的区域的 FQDN。

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>链接的虚拟网络中虚拟机上的 DNS 后缀是否会更改为专用区域的 DNS 后缀？

不是。 链接的虚拟网络中虚拟机的 DNS 后缀将保留为 Azure 提供的默认后缀（“*.internal.chinacloudapp.cn”）。 可以手动将虚拟机上的此 DNS 后缀更改为专用区域的 DNS 后缀。
有关如何更改此后缀的指导，请参阅 [使用动态 DNS 在自己的 DNS 服务器中注册主机名](../virtual-network/virtual-networks-name-resolution-ddns.md#windows-clients)

## <a name="what-are-the-usage-limits-for-azure-dns-private-zones"></a>Azure DNS 专用区域有哪些使用限制？

请参阅 [Azure DNS 限制](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-dns-limits)，详细了解 Azure DNS 专用区域的使用限制。

### <a name="does-azure-dns-private-zones-store-any-customer-content"></a>Azure DNS 专用区域是否存储一些客户内容？

否，Azure DNS 专用区域不存储任何客户内容。

## <a name="next-steps"></a>后续步骤

- [详细了解 Azure 专用 DNS](private-dns-overview.md)
