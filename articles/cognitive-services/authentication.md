---
title: 身份验证
titleSuffix: Azure Cognitive Services
description: 有三种方法可以对 Azure 认知服务资源的请求进行身份验证：订阅密钥、持有者令牌或多服务订阅。 本文介绍每种方法以及如何发出请求。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
origin.date: 11/22/2019
ms.date: 11/23/2020
ms.author: v-johya
ms.openlocfilehash: a0445846112a9bc6d23b43acb29930436aa18bef
ms.sourcegitcommit: f1d0f81918b8c6fca25a125c17ddb80c3a7eda7e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2020
ms.locfileid: "96306266"
---
# <a name="authenticate-requests-to-azure-cognitive-services"></a>对 Azure 认知服务的请求进行身份验证

对 Azure 认知服务的每个请求都必须包含身份验证标头。 此标头传递订阅密钥或访问令牌，用于验证服务或服务组订阅。 本文介绍三种对请求进行身份验证的方法以及每种方法的要求。

* 使用[单服务](#authenticate-with-a-single-service-subscription-key)或[多服务](#authenticate-with-a-multi-service-subscription-key)订阅密钥进行身份验证
* 使用[令牌](#authenticate-with-an-authentication-token)进行身份验证
* 使用 [Azure Active Directory (AAD)](#authenticate-with-azure-active-directory) 进行身份验证

## <a name="prerequisites"></a>先决条件

在发出请求之前，需要具有 Azure 帐户和 Azure 认知服务订阅。 如果已有帐户，请继续并跳到下一节。 如果还没有帐户，我们会提供指南，可在几分钟内完成设置：[创建 Azure 认知服务帐户](cognitive-services-apis-create-account.md)。

[创建帐户](https://www.azure.cn/pricing/details/cognitive-services/)后，可以从 [Azure 门户](cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)获取订阅密钥。

## <a name="authentication-headers"></a>身份验证标头

让我们快速查看可用于 Azure 认知服务的身份验证标头。

| 标头 | 说明 |
|--------|-------------|
| Ocp-Apim-Subscription-Key | 使用此标头通过特定服务订阅密钥或多服务订阅密钥进行身份验证。 |
| Ocp-Apim-Subscription-Region | 只有在使用具有 [Translator 服务](./Translator/reference/v3-0-reference.md)的多服务订阅密钥时才需要此标头。 使用此标头指定订阅区域。 |
| 授权 | 如果使用的是身份验证令牌，则使用此标头。 以下各节详细介绍了执行令牌交换的步骤。 提供的值遵循以下格式：`Bearer <TOKEN>`。 |

## <a name="authenticate-with-a-single-service-subscription-key"></a>使用单服务订阅密钥进行身份验证

第一个选项是使用特定服务（如 Translator）的订阅密钥对请求进行身份验证。 Azure 门户中的密钥可用于已创建的每个资源。 要使用订阅密钥对请求进行身份验证，必须将其作为 `Ocp-Apim-Subscription-Key` 标头传递。

这些示例请求演示了如何使用 `Ocp-Apim-Subscription-Key` 标头。 请记住，使用此示例时，需要包括有效的订阅密钥。

这是对 Translator 服务的示例调用：
```cURL
curl -X POST 'https://api.translator.azure.cn/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-a-multi-service-subscription-key"></a>使用多服务订阅密钥进行身份验证

>[!WARNING]
> 目前，以下服务不支持多服务密钥：QnA Maker、语音服务、自定义视觉和异常检测器。

此选项仍使用订阅密钥对请求进行身份验证。 主要区别在于订阅密钥未绑定到特定服务，而单个密钥可用于对多个认知服务的请求进行身份验证。 有关区域可用性、支持的功能和定价的信息，请参阅[认知服务定价](https://www.azure.cn/pricing/details/cognitive-services/)。

订阅密钥在每个请求中作为 `Ocp-Apim-Subscription-Key` 标头提供。

<!--Video-->
### <a name="supported-regions"></a>支持的区域

使用多服务订阅密钥向 `api.cognitive.azure.cn` 发出请求时，必须在 URL 中包含该区域。 例如：`chinaeast2.api.cognitive.azure.cn`。

将多服务订阅密钥与 Translator 服务配合使用时，必须使用 `Ocp-Apim-Subscription-Region` 标头指定订阅区域。

以下区域支持多服务身份验证：

- `chinaeast2`
- `chinanorth`

### <a name="sample-requests"></a>示例请求

这是对 Translator 服务的示例调用：

```cURL
curl -X POST 'https://api.translator.azure.cn/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Ocp-Apim-Subscription-Region: YOUR_SUBSCRIPTION_REGION' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-an-authentication-token"></a>使用身份验证令牌进行身份验证

某些 Azure 认知服务接受并在某些情况下需要身份验证令牌。 目前，以下服务支持身份验证令牌：

* 文本翻译 API
* 语音服务：语音转文本 REST API
* 语音服务：文本转语音 REST API

>[!WARNING]
> 支持身份验证令牌的服务可能会随时间而变化，请在使用此身份验证方法之前检查服务的 API 参考。

单服务订阅密钥和多服务订阅密钥均可用于交换认证令牌。 身份验证令牌有效期为 10 分钟。

身份验证令牌作为 `Authorization` 标头包含在请求中。 提供的令牌值必须以 `Bearer` 开头，例如：`Bearer YOUR_AUTH_TOKEN`。

### <a name="sample-requests"></a>示例请求

使用此 URL 交换身份验证令牌的订阅密钥：`https://YOUR-REGION.api.cognitive.azure.cn/sts/v1.0/issueToken`。

```cURL
curl -v -X POST \
"https://YOUR-REGION.api.cognitive.azure.cn/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Content-length: 0" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

以下多服务区域支持令牌交换：

- `chinaeast2`
- `chinanorth`

获得身份验证令牌后，需要在每个请求中将其作为 `Authorization` 标头传递。 这是对 Translator 服务的示例调用：

```cURL
curl -X POST 'https://api.translator.azure.cn/translate?api-version=3.0&from=en&to=de' \
-H 'Authorization: Bearer YOUR_AUTH_TOKEN' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

[!INCLUDE [](../../includes/cognitive-services-azure-active-directory-authentication.md)]

## <a name="see-also"></a>另请参阅

* [什么是认知服务？](./what-are-cognitive-services.md)
* [认知服务定价](https://www.azure.cn/pricing/details/cognitive-services/)
* [自定义子域](cognitive-services-custom-subdomains.md)

