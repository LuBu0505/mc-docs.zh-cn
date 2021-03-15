---
title: Azure CLI 脚本示例 - 创建和提交作业
description: 本主题中的 Azure CLI 脚本演示如何使用 HTTPs URL 将作业提交到简单编码的转换。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 08/31/2020
ms.date: 03/15/2021
ms.author: v-jay
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7a82afafe1afdaee253360037fc6883503fb145c
ms.sourcegitcommit: 5f85f27bd5d62ffb4913b9b9bd86cc41b3dfbf06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103211795"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI 示例：创建并提交作业

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

在媒体服务 v3 中提交作业来处理视频时，必须告知媒体服务查找输入视频的位置。 其中一个选项是指定 HTTPS URL 作为作业输入（如本文中所示）。 

## <a name="prerequisites"></a>必备条件 

[创建媒体服务帐户](./create-account-howto.md)。

## <a name="example-script"></a>示例脚本

运行 `az ams job start` 时，可以在作业的输出上设置一个标签。 稍后可以使用此标签来标识该输出资产的用途。 

- 如果为标签分配一个值，请将 ‘--output-assets’ 设置为 “assetname=label”
- 如果不为标签分配值，请将 ‘--output-assets’ 设置为 “assetname=”。
  请注意，你向 `output-assets` 添加了“=”。 

```azurecli
az ams job start \
  --name testJob001 \
  --transform-name testEncodingTransform \
  --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' \
  --files 'Ignite-short.mp4' \
  --output-assets testOutputAssetName= \
  -a amsaccount \
  -g amsResourceGroup 
```

获取如下所示的响应：

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

## <a name="next-steps"></a>后续步骤

[az ams job (CLI)](https://docs.microsoft.com/cli/azure/ams/job)
