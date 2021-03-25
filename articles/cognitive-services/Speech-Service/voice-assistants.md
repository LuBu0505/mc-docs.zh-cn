---
title: 语音助理 - 语音服务
titleSuffix: Azure Cognitive Services
description: 概述使用语音软件开发工具包 (SDK) 的语音助理的特性、功能和限制。
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: v-johya
ms.openlocfilehash: c5a1395f696d67eab148a900acb3b662a2b96a27
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104768108"
---
# <a name="what-is-a-voice-assistant"></a>什么是语音助手？

使用语音服务的语音助理能够支持开发人员为其应用程序和体验创建自然的、类似于人类的对话界面。

<!--Not available in MC: Azure Bot Service-->
语音助理服务在设备与使用自定义命令解决语音操控方案需求的助理实现之间提供快速可靠的交互。

对于直截了当式的语音操控方案，可以利用[自定义命令](custom-commands.md)应用创作功能的简便性。

| 如果你想要... | 那么请考虑... | 例如… |
|-------------------|------------------|----------------|
|通过简化的创作和托管实现语音操控或简单的任务导向型对话 | [自定义命令](custom-commands.md) | <ul><li>“打开吸顶灯”</li><li>“让温度再暖 5 度”</li><li>[此处](https://speech.azure.cn/customcommands)提供了其他示例</li></ul>

使用“自定义命令”，可以轻松构建针对语音优先的交互体验进行了优化的丰富语音命令应用。 它提供统一的创作体验、自动托管模型和相对较低的复杂性，帮助你集中精力为“语音命令”场景构建最佳解决方案。


## <a name="reference-architecture-for-building-a-voice-assistant-using-the-speech-sdk"></a>使用语音 SDK 生成语音助理的参考体系结构

   ![语音助理业务流程服务流的概念图](./media/voice-assistants/overview.png "语音助理流")

## <a name="core-features"></a>核心功能

<!--Customized in MC-->
选择[自定义命令](custom-commands.md)创建助理交互时，可以使用丰富的一组自定义功能，根据品牌、产品和个性来自定义助理。

| 类别 | 功能 |
|----------|----------|
|[语音转文本](speech-to-text.md) | 语音助理使用语音服务中的[语音转文本](speech-to-text.md)将实时音频转换为识别的文本。 此文本是听录而成的，因此可供助理实现和客户端应用程序使用。
|[文本转语音](text-to-speech.md) | 使用语音服务中的[文本转语音](text-to-speech.md)来合成助理做出的文本响应。 然后，这些合成内容可作为音频流提供给客户端应用程序使用。 Microsoft 提供生成你自己的自定义优质神经 TTS 语音（根据品牌发出语音）的功能。

## <a name="getting-started-with-voice-assistants"></a>语音助理入门

我们专门提供了快速入门来帮助你在 10 分钟内运行代码。 下表提供了按语言组织的语音助理快速入门列表。

* [快速入门：使用自定义命令生成语音操控应用](quickstart-custom-commands-application.md)

## <a name="sample-code-and-tutorials"></a>示例代码和教程

GitHub 上提供了用于创建语音助理的示例代码。 这些示例涵盖了以多种流行编程语言编写的、用于连接到助理的客户端应用程序。

* [GitHub 上的语音助理示例](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant)
* [教程：创建使用简单语音命令的自定义命令应用程序](./how-to-develop-custom-commands-application.md)

## <a name="customization"></a>自定义

通过 Azure 语音服务生成的语音助理可以使用各种自定义选项。

* [自定义语音识别](./custom-speech-overview.md)
<!--Customized in MC-->

## <a name="next-steps"></a>后续步骤

* [免费获取语音服务订阅密钥](overview.md#try-the-speech-service-for-free)
* [详细了解自定义命令](custom-commands.md)
* [获取语音 SDK](speech-sdk.md)

