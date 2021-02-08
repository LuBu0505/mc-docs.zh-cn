---
title: include 文件
description: include 文件
services: service-bus-messaging
ms.service: service-bus-messaging
ms.topic: include
origin.date: 11/24/2020
author: rockboyfor
ms.date: 02/01/2021
ms.testscope: yes|no
ms.testdate: 12/07/2020null
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 20e1e70b30a25dad9d05259b1bc7a63b8a167131
ms.sourcegitcommit: 7fc72b8afbdf9ad5e53922f489229e54282214b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99541126"
---
<!--Verified Successfully-->
与 HTTP/REST API 一样，AMQP-over-WebSockets 协议选项通过端口 TCP 443 运行，但在功能上与普通 AMQP 相同。 由于额外的握手往返，此选项的初始连接延迟稍高，并且作为共享 HTTPS 端口的折衷方案，此选项的开销略高。 如果选择此模式，TCP 端口 443 足以进行通信。 以下选项允许选择普通 AMQP 或 AMQP WebSockets 模式：

| 语言 | 选项   |
| -------- | ----- |
| .NET     | 具有 [TransportType.Amqp](https://docs.azure.cn/dotnet/api/microsoft.azure.servicebus.transporttype) 或 [TransportType.AmqpWebSockets](https://docs.azure.cn/dotnet/api/microsoft.azure.servicebus.transporttype) 的 [ServiceBusConnection.TransportType](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.servicebusconnection.transporttype) 属性 |
| Java     | 具有 [com.microsoft.azure.servicebus.primitives.TransportType.AMQP](https://docs.azure.cn/java/api/com.microsoft.azure.servicebus.primitives.transporttype) 或 [com.microsoft.azure.servicebus.primitives.TransportType.AMQP_WEB_SOCKETS](https://docs.azure.cn/java/api/com.microsoft.azure.servicebus.primitives.transporttype) 的 [com.microsoft.azure.servicebus.ClientSettings](https://docs.azure.cn/java/api/com.microsoft.azure.servicebus.clientsettings.clientsettings) |
| 节点  | [ServiceBusClientOptions](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclientoptions) 具有 `webSocket` 构造函数参数。 |
| Python | 具有 [TransportType.Amqp](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) 或 [TransportType.AmqpOverWebSocket](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) 的 [ServiceBusClient.transport_type](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.ServiceBusClient) |

<!-- Update_Description: new article about service bus websockets options -->
<!--NEW.date: 12/07/2020-->