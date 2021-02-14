---
title: 入门 - 翻译器
titleSuffix: Azure Cognitive Services
description: 本文将演示如何注册 Azure 认知服务翻译器并获取订阅密钥。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 05/26/2020
ms.date: 02/04/2021
ms.author: v-johya
ms.openlocfilehash: 071f7bbae33d4104646cf8ba795abc0e137232f6
ms.sourcegitcommit: dc0d10e365c7598d25e7939b2c5bb7e09ae2835c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99579509"
---
# <a name="how-to-sign-up-for-translator"></a>如何注册翻译器

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

- 没有帐户？ 可以创建[试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)进行试验，无需支付任何费用。
- 已有帐户？ [登录](https://portal.azure.cn/)

## <a name="create-a-subscription-for-translator"></a>为翻译器创建订阅

登录到门户后，可以创建翻译器的订阅，如下所示：

1. 选择“+ 创建资源”。
1. 在“搜索市场”搜索框中，输入“翻译器”，然后从结果中选择它 。
1. 选择“创建”，定义该订阅的详细信息。
1. 从“定价层”列表中，选择最适合需要的定价层。
    1. 每个订阅都有一个免费层。 免费层具有与付费计划相同的特征和功能，并且不会过期。
    1. 帐户只能有一个免费订阅。
1. 选择“创建”完成创建订阅。

## <a name="authentication-key"></a>身份验证密钥

注册翻译器时，将获得你的订阅所特有的个性化访问密钥。 每次调用翻译器时都需要此密钥。

1. 通过先选择相应的订阅检索身份验证密钥。
1. 在订阅详细信息的“资源管理”部分中选择“密钥和终结点” 。
1. 复制订阅所列出的任一密钥。

## <a name="learn-test-and-get-support"></a>了解、测试和获取支持

- [GitHub 上的代码示例](https://github.com/MicrosoftTranslator)
- [Microsoft Translator 支持论坛](https://www.aka.ms/TranslatorForum)

Microsoft Translator 通常会在验证订阅帐户状态前允许通过前几个请求。 如果前几个翻译器请求成功，然后调用失败，错误响应会指示问题。 请记录 API 响应，以便你能够查看原因。

## <a name="pricing-options"></a>定价选项

- [翻译](https://www.azure.cn/pricing/details/cognitive-services/)

<!-- Custom Translator not support in Azure -->
