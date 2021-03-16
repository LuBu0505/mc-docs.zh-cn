---
title: 排查事件网格问题
description: 本文提供了各种方式来解决 Azure 事件网格问题
ms.topic: conceptual
ms.date: 03/05/2021
ms.author: v-johya
ms.openlocfilehash: 21a566eda9350f7640323d7ec0797083f4146151
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102204975"
---
# <a name="troubleshoot-azure-event-grid-issues"></a>解决 Azure 事件网格问题
本文提供的信息有助于解决 Azure 事件网格问题。 

## <a name="diagnostic-logs"></a>诊断日志
为事件网格主题或域启用诊断设置，以捕获和查看发布与传送失败日志。 有关详细信息，请参阅[诊断日志](enable-diagnostic-logs-topic.md)。

## <a name="metrics"></a>指标
你可以查看有关事件网格主题和订阅的指标，并创建关于它们的警报。 有关详细信息，请参阅[事件网格指标](monitor-event-delivery.md)。

## <a name="alerts"></a>警报
创建有关 Azure 事件网格指标和活动日志操作的警报。 有关详细信息，请参阅[有关事件网格指标和活动日志的警报](set-alerts.md)。

## <a name="subscription-validation-issues"></a>订阅验证问题
在创建事件订阅的过程中，你可能会收到一条错误消息，指出所提供的终结点的验证失败。 若要解决订阅验证问题，请参阅[事件网格订阅验证故障排除](troubleshoot-subscription-validation.md)。 

## <a name="error-codes"></a>错误代码
如果你收到错误代码为 400、409 和 403 等等的错误消息，请参阅[解决事件网格错误](troubleshoot-errors.md)。 

### <a name="sample"></a>示例
请参阅[行计数器示例](https://docs.microsoft.com/samples/azure/azure-sdk-for-net/line-counter/)。 此示例应用展示了如何将存储、事件中心和事件网格客户端与 ASP.NET Core 集成、分布式跟踪和托管服务配合使用。 它允许用户将文件上传到 blob，这会触发包含文件名的事件中心事件。 事件中心处理器接收事件，然后，应用下载 blob 并对文件中的行数进行计数。 应用将显示一个指向某个页面的链接，其中包含行计数。 单击此链接后，将使用事件网格发布包含该文件名称的 CloudEvent。

## <a name="next-steps"></a>后续步骤
如需更多帮助，请在 [Stack Overflow 论坛](https://stackoverflow.com/questions/tagged/azure-eventgrid)中发布问题，或开具[支持票证](https://www.azure.cn/support/contact/)。 

