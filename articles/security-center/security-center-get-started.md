---
title: 升级到 Azure Defender - Azure 安全中心
description: 本快速入门介绍如何升级到安全中心的 Azure Defender，以提高安全性。
services: security-center
documentationcenter: na
author: Johnnytechn
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 12/3/2018
ms.date: 02/25/2021
ms.author: v-johya
ms.openlocfilehash: b4b4d481a2a4affaa532dc720380282031d87498
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197134"
---
# <a name="quickstart-set-up-azure-security-center"></a>快速入门：设置 Azure 安全中心

Azure 安全中心跨混合云工作负荷提供统一的安全管理和威胁防护。 免费功能只能为 Azure 资源提供有限的安全性，而启用 Azure Defende 能将这些功能扩展到本地和其他云。 借助 Azure Defende，可以查找和修复安全漏洞、应用访问和应用程序控制来阻止恶意活动、使用分析和智能检测威胁，以及在受到攻击时迅速做出响应。 可以免费试用 Azure Defender。 若要了解详细信息，请参阅[定价页](https://www.azure.cn/pricing/details/security-center/)。

本快速入门介绍如何启用 Azure Defende 以提高安全性，以及如何在计算机上安装 Log Analytics 代理来监视安全漏洞和威胁。

执行以下步骤：

> [!div class="checklist"]
> * 在 Azure 订阅中启用安全中心
> * 在 Azure 订阅上启用 Azure Defender
> * 启用自动数据收集

## <a name="prerequisites"></a>先决条件
若要开始使用安全中心，必须订阅 Azure。 如果你没有订阅，则可以注册[试用版](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。

若要在订阅中启用 Azure Defende，必须拥有“订阅所有者”、“订阅参与者”或“安全管理员”角色。


## <a name="enable-security-center-on-your-azure-subscription"></a>在 Azure 订阅中启用安全中心

1. 登录到 [Azure 门户](https://azure.microsoft.com/features/azure-portal/)。

1. 从门户的菜单中，选择“安全中心”。 

    此时会打开安全中心的“概述”页面。

    :::image type="content" source="./media/security-center-get-started/overview.png" alt-text="安全中心的“概述”仪表板" lightbox="./media/security-center-get-started/overview.png":::

“安全中心 - 概述”提供了统一的视图，用于查看混合云工作负荷的安全态势，可让你发现和评估工作负荷的安全性，以及识别和缓解风险。 安全中心会自动免费启用以前尚未由你或其他订阅用户加入的所有 Azure 订阅。

可以通过选择“订阅”菜单项来查看和筛选订阅列表。 安全中心会调整显示信息，以反映所选订阅的安全状况。 

在首次启动安全中心后的几分钟内，可以看到：

- 有关如何改善已连接资源的安全性的“建议”。
- 当前正在由安全中心评估的资源的清单，以及每个资源的安全状况。

若要充分利用安全中心，需要按以下步骤启用 Azure Defender，并安装 Log Analytics 代理。

> [!TIP]
> 若要在管理组中的所有订阅上启用安全中心，请参阅[在多个 Azure 订阅上启用安全中心](onboard-management-group.md)。

## <a name="enable-azure-defender"></a>启用 Azure Defender

若要学习安全中心快速入门和教程，需要启用 Azure Defender。 可免费试用 30 天。 若要了解详细信息，请参阅[定价页](https://www.azure.cn/pricing/details/security-center/)。 

1. 从安全中心的边栏选择“开始使用”。

    :::image type="content" source="./media/security-center-get-started/get-started-upgrade-tab.png" alt-text="“入门”页的“升级”选项卡"::: 

    “升级”选项卡列出了符合加入条件的订阅和工作区。

1. 从“选择要对其启用 Azure Defender 的工作区”列表中，选择要升级的工作区。
   - 如果选择的订阅和工作区不符合试用条件，会在下一步骤中对其进行升级，并开始收费。
   - 如果你选择了一个符合试用条件的工作区，下一步将开始试用。
1. 选择“升级”以启用 Azure Defender。

## <a name="enable-automatic-data-collection"></a>启用自动数据收集
安全中心从计算机收集数据，以监视安全漏洞和威胁。 数据是使用 Log Analytics 代理收集的，该代理从计算机中读取各种与安全相关的配置和事件日志，然后将数据复制到工作区进行分析。 默认情况下，安全中心会自动创建新工作区。

启用自动预配后，安全中心可在所有受支持的计算机以及任何新建的计算机中安装 Log Analytics 代理。 我们强烈建议启用自动预配。

若要启用对 Log Analytics 代理的自动预配，请执行以下操作：

1. 从安全中心的菜单中，选择“定价和设置”。
1. 选择相关订阅。
1. 在“自动预配”页中，对于“用于 Azure VM 的 Log Analytics 代理”，将状态设置为“开”。
1. 选择“保存”。

    :::image type="content" source="./media/security-center-enable-data-collection/enable-automatic-provisioning.png" alt-text="启用 Log Analytics 代理的自动预配":::

>[!TIP]
> 如果需要设置工作区，那么代理安装过程可能需要最多 25 分钟的时间。

将代理部署到计算机后，安全中心可以提供与系统更新状态、OS 安全配置、终结点保护相关的其他建议，并生成其他安全警报。

>[!NOTE]
> 将自动预配设置为“关闭”不会从已预配 Log Analytics 代理的 Azure VM 中删除该代理。 禁用自动设置会限制对资源的安全监视。

<!--Not available in MC: cost-management-billing/costs/quick-acm-cost-analysis -->
<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png

