---
title: 快速入门：计算机视觉 REST API
titleSuffix: Azure Cognitive Services
description: 通过本快速入门实现计算机视觉 REST API 入门。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 12/28/2020
ms.author: v-johya
ms.custom: seodec18
ms.openlocfilehash: f1b90d1dde40b7fab97edff6ea1bc34643f06f72
ms.sourcegitcommit: b4fd26098461cb779b973c7592f951aad77351f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2021
ms.locfileid: "97856833"
---
使用计算机视觉 REST API：

* 分析图像中的标记、文本说明、人脸、成人内容，等等。
* 使用智能裁剪生成缩略图

> [!NOTE]
> 此快速入门使用 cURL 命令来调用 REST API。 也可以使用编程语言调用 REST API。 请参阅 GitHub 示例，查看 [C#](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/ComputerVision/REST)、[Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/ComputerVision/REST)、[Java](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/java/ComputerVision/REST)、[JavaScript](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/javascript/ComputerVision/REST) 和 [Go](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/go/ComputerVision/REST) 的相关示例。

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn) 
* 拥有 Azure 订阅后，在 Azure 门户中<a href="https://portal.azure.cn/#create/Microsoft.CognitiveServicesComputerVision"  title="创建计算机视觉资源"  target="_blank">创建计算机视觉资源 <span class="docon docon-navigate-external x-hidden-focus"></span></a>，获取密钥和终结点。 部署后，单击“转到资源”。
  * 需要从创建的资源获取密钥和终结点，以便将应用程序连接到计算机视觉服务。 你稍后会在快速入门中将密钥和终结点粘贴到下方的代码中。
  * 可以使用免费定价层 (`F0`) 试用该服务，然后再升级到付费层进行生产。
* 已安装 [cURL](https://curl.haxx.se/)

## <a name="analyze-an-image"></a>分析图像

若要分析图像的各种视觉特征，请执行以下步骤：

1. 将以下命令复制到文本编辑器中。
1. 必要时在命令中进行如下更改：
    1. 将 `<subscriptionKey>` 的值替换为你的订阅密钥。
    1. 将请求 URL 的第一部分 (`chinaeast2`) 替换为你自己的终结点 URL 中的文本。
        [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
    1. （可选）将请求正文中的图像 URL (`http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg\`) 更改为要分析的其他图像的 URL。
1. 打开命令提示符窗口。
1. 将文本编辑器中的命令粘贴到命令提示符窗口，然后运行命令。

```bash
curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -H "Content-Type: application/json" "https://chinaeast2.api.cognitive.azure.cn/vision/v3.1/analyze?visualFeatures=Categories,Description&details=Landmarks" -d "{\"url\":\"http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg\"}"
```

### <a name="examine-the-response"></a>检查响应

成功的响应以 JSON 格式返回。 示例应用程序会在命令提示符窗口中分析和显示成功响应，如下例所示：

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "requestId": "b6e33879-abb2-43a0-a96e-02cb5ae0b795",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

<!--Not available in MC: Read printed and handwritten text-->
## <a name="generate-a-thumbnail"></a>生成缩略图

可以通过计算机视觉，使用智能裁剪生成缩略图。 可以指定所需的高度和宽度，可以与输入图像的纵横比不同。 计算机视觉使用智能裁剪来智能识别感兴趣的区域并围绕该区域生成裁剪坐标。
 
要创建和运行示例，请执行以下步骤：

1. 将以下命令复制到文本编辑器中。
1. 必要时在命令中进行如下更改：
    1. 将 `<subscriptionKey>` 的值替换为你的订阅密钥。
    1. 将 `<thumbnailFile>` 的值替换为用于保存返回的缩略图的路径和名称。
    1. 将请求 URL 的第一部分 (`chinaeast2`) 替换为你自己的终结点 URL 中的文本。
        [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
    1. （可选）将请求正文 (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) 中的图像 URL 更改为要从中生成缩略图的其他图像的 URL。
1. 打开命令提示符窗口。
1. 将文本编辑器中的命令粘贴到命令提示符窗口。
1. 按 Enter 运行此程序。

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://chinaeast2.api.cognitive.azure.cn/vision/v3.1/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

### <a name="examine-the-response"></a>检查响应

成功的响应会将缩略图写入 `<thumbnailFile>` 中指定的文件。 如果请求失败，则响应包含错误代码和消息，以帮助确定出错的地方。 如果请求似乎是成功的，但创建的缩略图不是有效的图像文件，则可能是因为订阅密钥无效。


## <a name="next-steps"></a>后续步骤

深入了解计算机视觉 API。 要快速体验计算机视觉 API，请尝试使用 [Open API 测试控制台](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b/console)。

> [!div class="nextstepaction"]
> [探索计算机视觉 API](https://dev.cognitive.azure.cn/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b)

