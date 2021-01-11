---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/04/2021
ms.author: v-junlch
ms.custom: fasttrack-edit
ms.openlocfilehash: 3f518356b4c934015fcd035ae5305598cd64d336
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98021444"
---
下表显示了 Azure Functions 运行时的主版本支持的绑定：


| 类型 | 1.x | 2.x 及更高版本<sup>1</sup> | 触发器 | 输入 | 输出 |
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob 存储](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|
| [Azure Cosmos DB](../articles/azure-functions/functions-bindings-cosmosdb-v2.md)               |✔|✔|✔|✔|✔|
| [Dapr](https://github.com/dapr/azure-functions-extension)<sup>3</sup>             | |✔|✔|✔|✔|
| [事件网格](../articles/azure-functions/functions-bindings-event-grid.md)              |✔|✔|✔| |✔|
| [事件中心](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|
| [HTTP 和 Webhook](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [IoT 中心](../articles/azure-functions/functions-bindings-event-iot.md)             |✔|✔|✔| |✔|
| [Kafka](https://github.com/azure/azure-functions-kafka-extension)<sup>2</sup>             | |✔|✔| |✔|
| [移动应用](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔| | |✔|✔|
| [通知中心](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [队列存储](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|
| [RabbitMQ](../articles/azure-functions/functions-bindings-rabbitmq.md)<sup>2</sup>             | |✔|✔| |✔|
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔| | |✔|
| [服务总线](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔|✔| |✔|
| [SignalR](../articles/azure-functions/functions-bindings-signalr-service.md)             | |✔| |✔|✔|
| [表存储](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|
| [计时器](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |

<sup>1</sup> 从版本 2.x 运行时开始，除了 HTTP 和 Timer 以外，所有绑定都必须注册。 请参阅[注册绑定扩展](../articles/azure-functions/functions-bindings-register.md)。

<sup>2</sup> 消耗计划中不支持触发器。 需要[运行时驱动的触发器](../articles/azure-functions/functions-networking-options.md#premium-plan-with-virtual-network-triggers)。

<sup>3</sup> 仅支持 Kubernetes、IoT Edge 和其他自托管模式。

