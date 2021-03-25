---
title: 针对在运行自定义命令应用程序时发生的错误进行调试
titleSuffix: Azure Cognitive Services
description: 本文介绍如何针对在自定义命令应用程序中发生的运行时错误进行调试。
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: v-johya
ms.openlocfilehash: 1492facfd1c0766ddf3f23a48913f9a44622c58a
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767548"
---
# <a name="debug-errors-when-running-a-custom-commands-application"></a>针对在运行自定义命令应用程序时发生的错误进行调试

本文介绍如何针对在运行自定义命令应用程序时出现的错误进行调试。 

## <a name="connection-failed"></a>连接失败

如果从[客户端应用程序（包含语音 SDK）](./how-to-custom-commands-setup-speech-sdk.md)或 [Windows 语音助理客户端](./how-to-custom-commands-developer-flow-test.md)运行自定义命令应用程序，可能会遇到下面列出的连接错误：

| 错误代码 | 详细信息 |
| ------- | -------- |
| [401](#error-401) | AuthenticationFailure: WebSocket 升级失败并出现身份验证错误 |
| [1002](#error-1002)] | 服务器返回了状态代码“404”，而状态代码应为“101”。 |

### <a name="error-401"></a>错误 401
- 客户端应用程序中指定的区域与自定义命令应用程序的区域不匹配

- 语音资源密钥无效
    
    确保语音资源密钥正确。

### <a name="error-1002"></a>错误 1002 
- 自定义命令应用程序尚未发布
    
    在门户中发布你的应用程序。

- 自定义命令 applicationId 无效

    确保自定义命令应用程序 ID 正确。
 自定义命令应用程序在语音资源的外部

    确保在语音资源中创建自定义命令应用程序。

有关排查连接问题的详细信息，请参考 [Windows 语音助理客户端故障排除](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/tree/master/clients/csharp-wpf#troubleshooting)


## <a name="dialog-is-canceled"></a>对话已取消

运行自定义命令应用程序时，发生某些错误会导致对话取消。

- 如果你在门户中测试该应用程序，门户中会直接显示取消说明，并播放错误提示音。 

- 如果使用 [Windows 语音助理客户端](./how-to-custom-commands-developer-flow-test.md)运行该应用程序，该客户端会播放错误提示音。 可以在“活动日志”下找到“事件: CancelledDialog”。 

- 如果你正在学习我们的客户端应用程序示例[客户端应用程序（包含语音 SDK）](./how-to-custom-commands-setup-speech-sdk.md)，其中会播放错误提示音。 可以在“状态”下找到“事件: CancelledDialog”。 

- 如果你要生成自己的客户端应用程序，始终可以设计所需的逻辑来处理 CancelledDialog 事件。

CancelledDialog 事件由取消代码和说明组成，如下面所列：

| 取消代码 | 取消说明 |
| ------- | --------------- | ----------- |
| [MaxTurnThresholdReached](#no-progress-was-made-after-the-max-number-of-turns-allowed) | 达到轮次数目上限后没有任何进展 |
| [RecognizerQuotaExceeded](#recognizer-usage-quota-exceeded) | 超出了识别器的使用配额 |
| [RecognizerConnectionFailed](#connection-to-the-recognizer-failed) | 与识别器连接失败 |
| [RecognizerUnauthorized](#this-application-cannot-be-accessed-with-the-current-subscription) | 无法使用当前订阅访问此应用程序 |
| [RecognizerInputExceededAllowedLength](#input-exceeds-the-maximum-supported-length) | 输入超出了识别器支持的最大长度 |
| [RecognizerNotFound](#recognizer-not-found) | 未找到识别器 |
| [RecognizerInvalidQuery](#invalid-query-for-the-recognizer) | 针对识别器的查询无效 |
| [RecognizerError](#recognizer-return-an-error) | 识别器返回了错误 |

### <a name="no-progress-was-made-after-the-max-number-of-turns-allowed"></a>达到轮次数目上限后没有任何进展
完成特定次数的轮次后，如果未成功更新所需的槽，则会取消对话。 内置的最大次数为 3。

### <a name="recognizer-usage-quota-exceeded"></a>超出了识别器的使用配额
语言理解 (LUIS) 对资源用量施加了限制。 通常，发生“超出了识别器的使用配额”错误的可能原因包括： 
- LUIS 创作超出了限制

    将预测资源添加到自定义命令应用程序： 
    1. 转到“设置”>“LUIS 资源”
    1. 从“预测资源”中选择一个预测资源，或单击“创建新资源”  

- LUIS 预测资源超出限制

    如果使用的是 F0 预测资源，其限制为每月 1 万次查询，每秒 5 次查询。

有关 LUIS 资源限制的更多详细信息，请参阅[语言理解资源用量和限制](../luis/luis-limits.md#resource-usage-and-limits)

### <a name="connection-to-the-recognizer-failed"></a>与识别器连接失败
通常这意味着发生了暂时性的语言理解 (LUIS) 识别器连接失败。 重试连接应可解决问题。

### <a name="this-application-cannot-be-accessed-with-the-current-subscription"></a>无法使用当前订阅访问此应用程序
你的订阅未获授权，无法访问 LUIS 应用程序。 

### <a name="input-exceeds-the-maximum-supported-length"></a>输入超出了支持的最大长度
输入超过了 500 个字符。 对于输入言语，我们只允许最多 500 个字符。

### <a name="invalid-query-for-the-recognizer"></a>针对识别器的查询无效
输入超过了 500 个字符。 对于输入言语，我们只允许最多 500 个字符。

### <a name="recognizer-return-an-error"></a>识别器返回了错误
LUIS 识别器在尝试识别输入时返回了错误。

### <a name="recognizer-not-found"></a>未找到识别器
找不到自定义命令对话模型中指定的识别器类型。 目前，我们仅支持[语言理解 (LUIS) 识别器](https://luis.azure.cn/)。

## <a name="other-common-errors"></a>其他常见错误
### <a name="unexpected-response"></a>意外的响应
意外的响应可能是多种原因导致的。 首先要检查以下几项：
- 示例句子中的“是/否意向”

    由于目前除非是在“是/否意向”与确认功能配合使用的情况下，否则我们不支持“是/否意向”， 因此不会检测示例句子中定义的所有“是/否意向”。

- 在不同的命令中使用了类似的意向和示例句子

    如果两个命令共享类似的意向和示例句子，LUIS 识别准确度可能会受影响。 可以尝试尽量使命令功能和示例句子更为独特。

    有关提高识别准确度的最佳做法，请参阅 [LUIS 最佳做法](../luis/luis-concept-best-practices.md)。

- 对话已取消
    
    查看前面部分所述的取消原因。

### <a name="error-while-rendering-the-template"></a>呈现模板时出错
在语音响应中使用了未定义的参数。 

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>对象引用未设置为对象的实例
在“将活动发送到客户端”操作中定义的 JSON 有效负载内指定了空参数。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [查看 GitHub 上的示例](https://aka.ms/speech/cc-samples)

