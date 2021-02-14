---
title: Azure Stack Hub 帮助和支持
description: 获取 Microsoft Azure Stack Hub 的支持。
author: WenJason
ms.topic: article
origin.date: 01/19/2021
ms.date: 02/08/2021
ms.author: v-jay
ms.reviewer: shisab
ms.lastreviewed: 01/19/2021
ms.openlocfilehash: a9fff0a19fcb05595faac5887dc4aaa4f9fc9ea5
ms.sourcegitcommit: 20bc732a6d267b44aafd953516fb2f5edb619454
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99503997"
---
# <a name="azure-stack-hub-help-and-support"></a>Azure Stack Hub 帮助和支持

Azure Stack Hub 操作员可以使用“帮助 + 支持”收集诊断日志并将其发送到 Microsoft 进行故障排除。 可以从管理员门户访问 Azure Stack Hub 门户中的“帮助 + 支持”。 它提供一些资源，可帮助操作员了解有关 Azure Stack 的详细信息、查看其支持选项并获得专家帮助。  

![如何在 Azure Stack Hub 管理员门户中访问帮助和支持](media/azure-stack-help-and-support/help-and-support.png)

## <a name="help-resources"></a>帮助资源

操作员可以通过“帮助 + 支持”详细了解 Azure Stack Hub、查看其支持选项，以及获得专家帮助。

### <a name="things-to-try-first"></a>首先要尝试的操作

“帮助 + 支持”的顶部是应首先尝试的操作，例如阅读新概念、了解计费的工作原理或查看有哪些可用的支持选项。

![Azure Stack Hub 中的自助服务支持](media/azure-stack-help-and-support/get-support-tiles.png)

- **文档** [Azure Stack Hub 操作员文档](index.yml)包含介绍如何提供 Azure Stack Hub 服务的概念、操作方法说明和教程。 这些服务包括虚拟机、SQL 数据库、Web 应用等。

- **了解计费**。 获取有关[用量和计费](azure-stack-billing-and-chargeback.md)的提示。

- **支持选项**。 Azure Stack Hub 操作员有多种可供选择的 [Azure 支持选项](./azure-stack-manage-basics.md)，用于应对各种企业的需求。

### <a name="get-expert-help"></a>获取专家帮助

对于集成系统，Azure 和我们的原始设备制造商 (OEM) 硬件合作伙伴之间已经建立了协作的问题升级和解决流程。

如果存在云服务问题，请通过 Azure 支持寻求支持。 可以选择管理员门户右上角的“帮助”（问号），然后选择“帮助 + 支持”，打开“帮助 + 支持”概述，并提交新的支持请求  。 创建支持请求时，系统会预先选择 Azure Stack Hub 服务。 我们强烈建议客户使用此体验来提交票证，而不是使用 Azure 门户。

如果存在部署问题、修补和更新问题、硬件（包括现场可更换部件）问题，以及任何硬件品牌软件（例如在硬件生命周期主机上运行的软件）问题，请首先联系 OEM 硬件供应商。 至于其他问题，请联系 Azure 支持。

![获取集成系统的专家帮助](media/azure-stack-help-and-support/get-support-integrated.png)

对于 Azure Stack 开发工具包 (ASDK)，可以在 [Azure Stack Hub MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home?sort=relevancedesc&brandIgnore=True&searchTerm=Azure+Stack)中咨询与支持相关的问题。 

选择管理员门户右上角的“帮助”（问号），然后选择“帮助 + 支持”，打开“帮助 + 支持”概述，其中包含指向论坛的链接  。 我们会定期监视 MSDN 论坛。 由于 ASDK 是一个评估环境，因此我们不会通过 Azure 支持提供官方支持。

还可以转到 MSDN 论坛来探讨问题，或参加在线培训以提升自己的技能。

![获取有关 Azure Stack Hub 的专家帮助](media/azure-stack-help-and-support/get-support-cards.png)

### <a name="get-up-to-speed-with-azure-stack-hub"></a>快速熟悉 Azure Stack Hub

这套教程是根据运行的是 ASDK 或集成系统而自定义的，可帮助你快速熟悉环境。

![获取有关 Azure Stack Hub 的支持教程](media/azure-stack-help-and-support/get-support-tutorials.png)

## <a name="diagnostic-log-collection"></a>诊断日志收集

可以通过两种方式将诊断日志发送到 Microsoft：

- [主动发送日志](./diagnostic-log-collection.md#send-logs-proactively)：如果启用，日志收集将由特定的运行状况警报触发。
- [立即发送日志](./diagnostic-log-collection.md#send-logs-now)：可以手动选择特定滑动窗口作为日志收集的时间范围。

![此屏幕截图显示了如何开始收集诊断日志。](media/azure-stack-help-and-support/banner-enable-automatic-log-collection.png)

## <a name="diagnostic-log-collection"></a>诊断日志收集

从 1907 版开始，在“帮助和支持”中提供了两种新方法来收集日志：

- **自动收集**：如果启用，日志收集将由特定的运行状况警报触发。
- **立即收集日志**：可以从过去七天选择 1-4 小时滑动窗口。

![诊断日志收集选项](media/azure-stack-automatic-log-collection/azure-stack-log-collection-overview.png)

集成系统可以与 Azure 支持共享诊断日志。 由于 Azure Stack 开发工具包 (ASDK) 是一个评估环境，因此 Azure 支持不为它提供支持。 有关详细信息，请参阅 [Azure Stack 诊断日志收集概述](./diagnostic-log-collection.md)。

## <a name="help-and-support-for-earlier-releases-azure-stack-hub-pre-1905"></a>旧版 Azure Stack Hub（1905 之前）的帮助和支持

旧版 Azure Stack Hub 也有“帮助 + 支持”链接，此链接重定向到 [Azure Stack Hub 操作员文档](./index.yml)。

![获取支持教程](media/azure-stack-help-and-support/get-support-previous.png)

如果存在云服务问题，请通过 Azure 支持寻求支持。 可以选择管理员门户右上角的“帮助”（问号），选择“帮助和支持”，然后选择“新建支持请求”，直接向 Azure 支持提交新的支持请求  。

对于集成系统，Azure 和我们的 OEM 合作伙伴之间已经建立了协作的问题升级和解决流程。 如果存在云服务问题，请通过 Azure 支持寻求支持。

如果存在部署问题、修补和更新问题、硬件（包括现场可更换部件）问题，以及任何硬件品牌软件（例如在硬件生命周期主机上运行的软件）问题，请首先联系 OEM 硬件供应商。 至于其他问题，请联系 Azure 支持。

对于 ASDK，可以在 [Azure Stack Hub MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home?sort=relevancedesc&brandIgnore=True&searchTerm=Azure+Stack)中提出与支持相关的问题。

选择管理员门户右上角的“帮助”（问号），然后选择“新建支持请求”，从 Azure Stack Hub 社区的专家处获取帮助 。 由于 ASDK 是一个评估环境，因此我们不会通过 Azure 支持提供官方支持。

## <a name="next-steps"></a>后续步骤

- 了解[诊断日志收集](./diagnostic-log-collection.md)。
- 了解如何[查找云 ID](azure-stack-find-cloud-id.md)。
- 了解如何[排查 Azure Stack Hub 问题](azure-stack-troubleshooting.md)。
