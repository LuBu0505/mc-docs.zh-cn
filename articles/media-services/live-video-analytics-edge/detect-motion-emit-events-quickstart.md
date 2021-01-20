---
title: 检测运动并发出事件 - Azure
description: 本快速入门介绍如何以编程方式调用直接方法，从而使用 IoT Edge 上的实时视频分析来检测运动和发出事件。
ms.topic: quickstart
origin.date: 08/10/2020
ms.date: 01/18/2021
zone_pivot_groups: ams-lva-edge-programming-languages
ms.openlocfilehash: dd833ddd4726bf17f755e6917ad037be9f472325
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98229863"
---
# <a name="quickstart-detect-motion-and-emit-events"></a>快速入门：检测运动并发出事件

本快速入门将引导你完成开始使用 IoT Edge 上的实时视频分析的步骤。 它将 Azure VM 用作 IoT Edge 设备和模拟的实时视频流。 完成设置步骤后，你将能通过媒体图运行模拟实时视频流，该媒体图可检测和报告该流中的任何运动。 下图显示了该媒体图的图形表示形式。

![基于运动检测的实时视频分析](./media/analyze-live-video/motion-detection.svg) 

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [header](includes/detect-motion-emit-events-quickstart/csharp/header.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [header](includes/detect-motion-emit-events-quickstart/python/header.md)]
---

## <a name="prerequisites"></a>先决条件

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [prerequisites](includes/detect-motion-emit-events-quickstart/csharp/prerequisites.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [prerequisites](includes/detect-motion-emit-events-quickstart/python/prerequisites.md)]
---

## <a name="review-the-sample-video"></a>观看示例视频

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [review-sample-video](includes/detect-motion-emit-events-quickstart/csharp/review-sample-video.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [review-sample-video](includes/detect-motion-emit-events-quickstart/python/review-sample-video.md)]

---

## <a name="set-up-azure-resources"></a>设置 Azure 资源

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [setup-azure-resources](includes/detect-motion-emit-events-quickstart/csharp/setup-azure-resources.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [setup-azure-resources](includes/detect-motion-emit-events-quickstart/python/setup-azure-resources.md)]
---

## <a name="set-up-your-development-environment"></a>设置开发环境

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [setup-development-environment](includes/detect-motion-emit-events-quickstart/csharp/setup-development-environment.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [setup-development-environment](includes/detect-motion-emit-events-quickstart/python/setup-development-environment.md)]
---

## <a name="examine-the-sample-files"></a>检查示例文件

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [examine-sample-files](includes/detect-motion-emit-events-quickstart/csharp/examine-sample-files.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [examine-sample-files](includes/detect-motion-emit-events-quickstart/python/examine-sample-files.md)]
---

## <a name="generate-and-deploy-the-deployment-manifest"></a>生成并部署部署清单

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [generate-deploy-deployment-manifest](includes/detect-motion-emit-events-quickstart/csharp/generate-deploy-deployment-manifest.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [generate-deploy-deployment-manifest](includes/detect-motion-emit-events-quickstart/python/generate-deploy-deployment-manifest.md)]
---

## <a name="prepare-to-monitor-events"></a>准备监视事件

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [prepare-monitor-events](includes/detect-motion-emit-events-quickstart/csharp/prepare-monitor-events.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [prepare-monitor-events](includes/detect-motion-emit-events-quickstart/python/prepare-monitor-events.md)]
---

## <a name="run-the-sample-program"></a>运行示例程序

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [run-sample-program](includes/detect-motion-emit-events-quickstart/csharp/run-sample-program.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [run-sample-program](includes/detect-motion-emit-events-quickstart/python/run-sample-program.md)]
---

## <a name="interpret-results"></a>解释结果

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [interpret-results](includes/detect-motion-emit-events-quickstart/csharp/interpret-results.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [interpret-results](includes/detect-motion-emit-events-quickstart/python/interpret-results.md)]
---

## <a name="clean-up-resources"></a>清理资源

如果想学习其他快速入门，则请保留所创建的资源。 否则，请在 Azure 门户中，转到资源组，选择运行本快速入门所用的资源组，然后删除所有资源。

## <a name="next-steps"></a>后续步骤

运行其他快速入门，如检测实时视频源中的对象。        
