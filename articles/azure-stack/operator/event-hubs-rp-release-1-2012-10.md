---
title: Azure Stack Hub 上的事件中心 1.2012.1.0 发行说明
description: 了解 Azure Stack Hub 上的事件中心的 1.2012.1.0 版本，包括 bug 修复、功能以及如何安装更新。
author: WenJason
ms.author: v-jay
ms.service: azure-stack
ms.topic: article
origin.date: 02/15/2021
ms.date: 03/01/2021
ms.reviewer: kalkeea
ms.lastreviewed: 02/15/2021
ms.openlocfilehash: ffadbc2fa5b55d8942b2b959ee3928aaea77124e
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751882"
---
# <a name="event-hubs-on-azure-stack-hub-1201210-release-notes"></a>Azure Stack Hub 上的事件中心 1.2012.1.0 发行说明

这些发行说明介绍了 Azure Stack Hub 上的事件中心版本 1.2012.1.0 中的改进和修复，以及任何已知问题。 

[!INCLUDE [Azure Stack Hub update reminder](../includes/event-hubs-hub-update-banner.md)]

## <a name="updates-in-this-release"></a>此版本中的更新

此版本包括以下更新：

- 对于 Azure 门户 SDK 开发人员，现在支持门户版本 5.0.303.7361。
- 事件中心群集的内部日志记录改进。

## <a name="issues-fixed-in-this-release"></a>此版本中已修复的问题

此版本包括以下修复：

- 修复了事件中心群集的升级顺序，以解决一个升级问题。
- 当群集处于“正在升级”或“升级失败”状态时，针对事件中心群集的群集运行状况和备份运行状况检查不运行。 此版本中已修复了此问题。
- 修复了导致使用情况记录包含错误数量的一个 bug。 过去，我们显示的是容量单位 (CU) 而不是核心数。 以前，1CU 群集会在每小时使用量中显示 1 个核心。 现在，对于 1CU 群集，用户在其每小时使用量中会看到正确的数量 - 10 个核心。

## <a name="known-issues"></a>已知问题 

此版本没有已知问题。

## <a name="next-steps"></a>后续步骤

- 若要了解详细信息，请先阅读 [Azure Stack Hub 上的事件中心操作员概述](event-hubs-rp-overview.md)。

