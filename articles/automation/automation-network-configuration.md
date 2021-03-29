---
title: Azure 自动化网络配置详细信息
description: 本文详细说明了 Azure Automation State Configuration、Azure 自动化混合 Runbook 辅助角色、更新管理以及更改跟踪和清单所需的网络信息
ms.author: v-jay
ms.topic: conceptual
origin.date: 01/26/2021
ms.date: 03/15/2021
ms.openlocfilehash: 2e491fe7ab97cc2f89e2b56682611c49ca2a4036
ms.sourcegitcommit: 62410a4f24e5412edd9e8a06e897658b89036b16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102589926"
---
# <a name="azure-automation-network-configuration-details"></a>Azure 自动化网络配置详细信息

本页提供[混合 Runbook 辅助角色和 State Configuration](#hybrid-runbook-worker-and-state-configuration) 以及[更新管理](#update-management)所需的网络详细信息。

## <a name="hybrid-runbook-worker-and-state-configuration"></a>混合 Runbook 辅助角色和 State Configuration

混合 Runbook 辅助角色和 [Automation State Configuration](automation-dsc-overview.md) 需要以下端口和 URL 才能与 Azure 自动化通信。

* 端口：只需使用 443 即可进行出站 Internet 访问
* 全局 URL：`*.azure-automation.cn`
* 代理服务：`https://<workspaceId>.agentsvc.azure-automation.cn`

### <a name="network-planning-for-hybrid-runbook-worker"></a>混合 Runbook 辅助角色的网络规划

要使系统或用户混合 Runbook 辅助角色连接并注册 Azure 自动化，必须让其有权访问此部分所述的端口号和 URL。 辅助角色还必须有权访问 [Log Analytics 代理所需的端口和 URL](../azure-monitor/agents/agent-windows.md)，以便能够连接到 Azure Monitor Log Analytics 工作区。

### <a name="configuration-of-private-networks-for-state-configuration"></a>配置 State Configuration 的专用网络

如果节点位于专用网络中，则需要上文中定义的端口和 URL。 这些资源为托管节点提供网络连接，并允许 DSC 与 Azure 自动化功能通信。

如果使用在节点之间传递的 DSC 资源（如 [WaitFor* 资源](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)），还需要允许节点之间产生流量。 请参阅每个 DSC 资源的文档以了解这些网络要求。

若要了解 TLS 1.2 的客户端要求，请参阅[强制 Azure 自动化执行 TLS 1.2](automation-managing-data.md#tls-12-enforcement-for-azure-automation)。

## <a name="update-management"></a>更新管理

更新管理需要此表中的地址。 该表后面的段落同样适用于这两者。

与这些地址的通信使用端口 443。

|Azure 中国云  |
|---------|---------|
|\*.ods.opinsights.azure.cn|
|\*.oms.opinsights.azure.cn|
|\*.blob.core.chinacloudapi.cn|
|\*.azure-automation.cn|

创建网络组安全规则或配置 Azure 防火墙以允许流量流向自动化服务和 Log Analytics 工作区时，请使用[服务标记](../virtual-network/service-tags-overview.md#available-service-tags) GuestAndHybridManagement 和 AzureMonitor 。 这样可简化网络安全规则的日常管理。 若要获取当前服务标记和范围信息，并将其包含为本地防火墙配置的一部分，请参阅[可下载的 JSON 文件](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)。

## <a name="next-steps"></a>后续步骤

* 了解[自动化更新管理概述](update-management\overview.md)。
* 了解[混合 Runbook 辅助角色](automation-hybrid-runbook-worker.md)。
* 了解 [Automation State Configuration](automation-dsc-overview.md)。