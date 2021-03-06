---
title: 对象检测 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 了解与计算机视觉 API 的对象检测功能相关的概念 - 使用情况和限制。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
origin.date: 04/17/2019
ms.date: 12/28/2020
ms.author: v-johya
ms.custom: seodec18
ms.openlocfilehash: 585e5c7cec2840a9137e9661f24d2568de20a37e
ms.sourcegitcommit: b4fd26098461cb779b973c7592f951aad77351f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2021
ms.locfileid: "97856895"
---
# <a name="detect-common-objects-in-images"></a>检测图像中的常见对象

对象检测类似于[标记](concept-tagging-images.md)，但是 API 返回找到的每个对象的边框坐标（以像素为单位）。 例如，如果图像包含狗、猫和人，检测操作将列出这些对象及其在图像中的坐标。 可以使用此功能处理图像中对象之间的关系。 还可以确定图像中是否有多个相同标记的实例。

检测 API 根据图像中识别到的对象或生物应用标记。 目前，标记分类与对象检测分类之间没有正式关系。 从概念上讲，检测 API 仅查找对象和生物，而标记 API 还可以包含诸如“室内”等上下文术语，这些术语不能使用边界框进行本地化。

## <a name="object-detection-example"></a>对象检测示例

以下 JSON 响应表明计算机视觉在示例图像中检测对象时所返回的内容。

![一位正在厨房中使用 Microsoft Surface 设备的女士](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="limitations"></a>限制

请务必注意对象检测的限制，以便可以避免或缓解误报（缺少对象）和有限详细信息的影响。

* 如果对象小（小于图像的 5%），则通常不删除对象。
* 如果对象紧密地排列在一起（例如一摞盘子），则通常不会检测到对象。
* 对象不区分品牌或产品名称（例如，商店货架上不同类型的苏打饮料）。 但是可以使用[品牌检测](concept-brand-detection.md)功能从图像中获取品牌信息。

## <a name="use-the-api"></a>使用 API

对象检测功能属于[分析图像](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b) API。 可以通过本机 SDK 或 REST 调用来调用此 API。 将 `Objects` 包括在 **visualFeatures** 查询参数中。 然后，在获取完整 JSON 响应时，就只需分析 `"objects"` 部分内容的字符串。

* [快速入门：计算机视觉 REST API 或客户端库](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)

