---
title: Azure Cache for Redis 事件网格概述
description: 使用 Azure 事件网格发布 Azure Cache for Redis 事件。
author: curib
ms.author: v-junlch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cache
ms.openlocfilehash: f061252e437bf1a99f3302b3d216ec58f37be7e8
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751835"
---
# <a name="azure-cache-for-redis-event-grid-overview"></a>Azure Cache for Redis 事件网格概述 

Azure Cache for Redis 事件（例如修补、缩放、导入/导出 (RDB) 事件）使用 [Azure 事件网格](https://www.azure.cn/home/features/event-grid/)推送到订阅方（例如 Azure Functions、Azure 逻辑应用），甚至推送到你自己的 http 侦听器。 事件网格通过丰富的重试策略和死信向应用程序提供可靠的事件传递。

若要试用 Azure Cache for Redis 事件，请参阅以下任意快速入门：

|若要使用此工具：    |请参阅此快速入门： |
|--|-|
|Azure 门户    |[快速入门：使用 Azure 门户将 Azure Cache for Redis 事件路由到 Web 终结点](cache-event-grid-quickstart-portal.md)|
|PowerShell    |[快速入门：使用 PowerShell 将 Azure Cache for Redis 事件路由到 Web 终结点](cache-event-grid-quickstart-powershell.md)|
|Azure CLI    |[快速入门：使用 Azure CLI 将 Azure Cache for Redis 事件路由到 Web 终结点](cache-event-grid-quickstart-cli.md)|

## <a name="the-event-model"></a>事件模型

事件网格使用[事件订阅](../event-grid/concepts.md#event-subscriptions)将事件消息路由到订阅方。 此图展示了事件发布者、事件订阅和事件处理程序之间的关系。

:::image type="content" source="media/cache-event-grid/event-grid-model.png" alt-text="事件网格模型。":::

首先，将终结点订阅到事件。 然后，在触发某个事件时，事件网格服务会将有关该事件的数据发送到终结点。



## <a name="best-practices-for-consuming-events"></a>使用事件的最佳做法

处理 Azure Cache for Redis 事件的应用程序应当遵循一些建议的做法：
> [!div class="checklist"]
> * 由于可将多个订阅配置为将事件路由至相同的事件处理程序，因此请勿假定事件来自特定的源，而是应检查消息的主题，确保它来自所期望的 Azure Cache for Redis 实例。
> * 同样，检查 eventType 是否为准备处理的项，并且不假定所接收的全部事件都是期望的类型。
> * Azure Cache for Redis 事件保证至少向订阅方传递一次，以确保输出所有消息。 但是，由于订阅的重试或可用性，偶尔可能会出现重复消息。 若要详细了解消息传递和重试，请参阅[事件网格消息传递和重试](../event-grid/delivery-and-retry.md)。


## <a name="next-steps"></a>后续步骤

详细了解事件网格并尝试使用 Azure Cache for Redis 事件：

- [关于事件网格](../event-grid/overview.md)
- [使用 Azure CLI 将 Azure Cache for Redis 事件路由到 Web 终结点](cache-event-grid-quickstart-cli.md)
- [使用 Azure 门户将 Azure Cache for Redis 事件路由到 Web 终结点](cache-event-grid-quickstart-portal.md)
- [使用 PowerShell 将 Azure Cache for Redis 事件路由到 Web 终结点](cache-event-grid-quickstart-powershell.md)
