---
title: 将 Internet 域指向流量管理器 - Azure 流量管理器
description: 本文将帮助将公司域名指向流量管理器域名。
services: traffic-manager
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 10/11/2016
author: rockboyfor
ms.date: 02/22/2021
ms.testscope: no
ms.testdate: 09/28/2020
ms.author: v-yeche
ms.openlocfilehash: dcedc0429020de9d2b8e58956d19acc12f4e045b
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054049"
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>将公司 Internet 域指向 Azure 流量管理器域

创建流量管理器配置文件时，Azure 会自动为该配置文件分配一个 DNS 名称。 若要使用 DNS 区域中的某个名称，请创建一条映射到流量管理器配置文件域名的 CNAME DNS 记录。 可以在流量管理器配置文件的“配置”页上的“常规”部分中找到流量管理器域名。

例如，若要将名称 `www.contoso.com` 指向流量管理器 DNS 名称 `contoso.trafficmanager.cn`，请创建以下 DNS 资源记录：

`www.contoso.com IN CNAME contoso.trafficmanager.cn.`

对 www\.contoso.com 发出的所有流量请求都会定向到 contoso.trafficmanager.cn。

> [!IMPORTANT]
> 无法将第二级域（例如 *contoso.com*）指向流量管理器域。 DNS 协议标准不允许对二级域名使用 CNAME 记录。

## <a name="next-steps"></a>后续步骤

* [流量管理器路由方法](traffic-manager-routing-methods.md)
* [流量管理器 - 禁用、启用或删除配置文件](./traffic-manager-manage-profiles.md)
* [流量管理器 - 禁用或启用终结点](./traffic-manager-manage-endpoints.md)

<!--Update_Description: update meta properties, wording update, update link-->