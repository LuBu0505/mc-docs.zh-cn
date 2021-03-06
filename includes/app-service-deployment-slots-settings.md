---
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 09/18/2019
ms.date: 01/11/2021
ms.author: v-tawe
ms.openlocfilehash: adc94da379f0a367b84ea8dce4308fe4ae9f79ea
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98024162"
---
从另一个部署槽克隆配置时，可以编辑克隆的配置。 某些配置元素在交换时遵循内容（不特定于槽），而其他配置元素会在交换之后保留在同一个槽（特定于槽）。 以下列表显示交换槽时会更改的设置。

**已交换的设置**：

* 常规设置 - 例如 Framework 版本、32/64 位、Web 套接字
* 应用设置（可以配置为停在槽中）
* 连接字符串（可以配置为停在槽中）
* 处理程序映射
* 公用证书
* WebJobs 内容
* 混合连接 *
* 服务终结点 *
* Azure 内容分发网络 *

标有星号 (*) 的功能计划取消交换。 

**不交换的设置**：

* 发布终结点
* 自定义域名
* 非公共证书和 TLS/SSL 设置
* 缩放设置
* Web 作业计划程序
* IP 限制
* Always On
* 诊断设置
* 跨域资源共享 (CORS)
* 虚拟网络集成

> [!NOTE]
> 若要使这些设置可交换，请在应用的每个槽中添加应用设置 `WEBSITE_OVERRIDE_PRESERVE_DEFAULT_STICKY_SLOT_SETTINGS`，并将其值设置为 `0` 或 `false`。 这些设置要么全部可交换，要么根本不可交换。 不能仅使某些设置可交换，而使其他设置不可交换。

> 应用于不交换的设置的某些应用设置也不交换。 例如，由于诊断设置不会交换，因此相关的应用设置（如 `WEBSITE_HTTPLOGGING_RETENTION_DAYS` 和 `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS`）也不会交换，即使它们未显示为槽设置也是如此。
>
