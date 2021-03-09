---
title: Azure CLI 脚本示例 - 创建转换
description: 转换描述了处理视频或音频文件的任务的简单工作流（通常称为“工作程序”）。 本文中的 Azure CLI 脚本演示如何创建转换。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 11/18/2020
ms.date: 03/08/2021
ms.author: v-jay
ms.custom: devx-track-azurecli
ms.openlocfilehash: 559441d2b5725ee7ccd82b03e7225b881debcb8f
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697307"
---
# <a name="create-a-transform"></a>创建转换

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

本文中的 Azure CLI 脚本演示如何创建转换。 转换描述了处理视频或音频文件的任务的简单工作流（通常称为“工作程序”）。 应始终检查具有所需名称和“工作程序”的转换是否已存在。 如果已存在，应再次使用该转换。

## <a name="prerequisites"></a>先决条件

[创建媒体服务帐户](./create-account-howto.md)。

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

> [!NOTE]
> 只能为 [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) 指定自定义标准编码器预设 JSON 文件的路径，请参阅[使用自定义转换进行编码](custom-preset-cli-howto.md)示例。
>
> 使用 [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) 时，不能传递文件名。

## <a name="example-script"></a>示例脚本

```azurecli
#!/bin/bash

# Update the following variables for your own settings:
$resourceGroup=amsResourceGroup
$amsAccountName=amsmediaaccountname

# Create a simple Transform for Adaptive Bitrate Encoding
az ams transform create \
 --name myFirstTransform \
 --preset AdaptiveStreaming \
 --description 'a simple Transform for Adaptive Bitrate Encoding' \
 -g $resourceGroup \
 -a $amsAccountName \

 # Create a Transform for Video Analyer Preset
az ams transform create \
 --name videoAnalyzerTransform \
 --preset  VideoAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

 # Create a Transform for Audio Analzyer Preset
az ams transform create \
 --name audioAnalyzerTransform \
 --preset  AudioAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

# List all the Transforms in an account
az ams transform list -a $amsAccountName -g $resourceGroup

echo "press  [ENTER]  to continue."
read continue
```

## <a name="rest"></a>[REST](#tab/rest/)

[!INCLUDE [task general transform creation](./includes/task-create-basic-audio-rest.md)]

---

## <a name="next-steps"></a>后续步骤

[详细了解转换和作业](transforms-jobs-concept.md)
