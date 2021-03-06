---
title: 排查 Azure IoT 中心错误 400027 ConnectionForcefullyClosedOnNewConnection
description: 了解如何修复错误 400027 ConnectionForcefullyClosedOnNewConnection
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
origin.date: 01/30/2020
ms.date: 02/17/2020
ms.author: v-yiso
ms.openlocfilehash: c9e9ff700f92a1bab3004c9450fc720fa92673ab
ms.sourcegitcommit: b6fead1466f486289333952e6fa0c6f9c82a804a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2020
ms.locfileid: "96300795"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

本文介绍 **400027 ConnectionForcefullyClosedOnNewConnection** 错误的原因和解决方案。

## <a name="symptoms"></a>症状

你的设备在使用 .NET SDK 和 MQTT 传输类型时因为 ConnectionStatusChangeReason 断开连接，并显示 Communication_Error。

设备到云的孪生操作（如读取或修补报告的属性）或直接方法调用失败，错误代码为 **400027**。

## <a name="cause"></a>原因

另一个客户端使用相同凭据创建了与 IoT 中心的新连接，因此 IoT 中心关闭了以前的连接。 IoT 中心不允许多个客户端使用同一组凭据进行连接。

## <a name="solution"></a>解决方案

请确保每个客户端使用其自己的标识连接到 IoT 中心。