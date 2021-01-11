---
title: 检测运动并在边缘设备上录制视频 - Azure
description: 本快速入门演示了如何使用 IoT Edge 上的实时视频分析来分析（模拟）IP 相机的实时视频源，检测是否存在任何运动，如果存在，则将 MP4 视频剪辑录制到边缘设备上的本地文件系统中。
ms.topic: quickstart
origin.date: 04/27/2020
ms.date: 01/11/2021
zone_pivot_groups: ams-lva-edge-programming-languages
ms.openlocfilehash: fd080d65aad00193402112e6d70a8da9bee5d382
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98021315"
---
# <a name="quickstart-detect-motion-and-record-video-on-edge-devices"></a>快速入门：检测运动并在边缘设备上录制视频
 
本快速入门介绍了如何在 IoT Edge 上使用实时视频分析来分析（模拟）IP 相机中的实时视频源。 本快速入门介绍如何检测是否存在任何运动，如果存在，则将 MP4 视频剪辑录制到边缘设备上的本地文件系统中。 本快速入门将 Azure VM 用作 IoT Edge 设备，并且还使用模拟的实时视频流。 

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [header](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/header.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [header](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/header.md)]
---

## <a name="prerequisites"></a>先决条件

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [prerequisites](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/prerequisites.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [prerequisites](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/prerequisites.md)]
---

## <a name="review-the-sample-video"></a>观看示例视频

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [review-sample-video](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/review-sample-video.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [review-sample-video](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/review-sample-video.md)]
---

## <a name="overview"></a>概述

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [overview](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/overview.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [overview](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/overview.md)]
---

## <a name="examine-and-edit-the-sample-files"></a>检查和编辑示例文件

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [examine-edit-sample-files](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/examine-edit-sample-files.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [examine-edit-sample-files](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/examine-edit-sample-files.md)]
---

## <a name="review---check-the-modules-status"></a>查看 - 检查模块的状态

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [check-modules-status](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/check-modules-status.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [check-modules-status](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/check-modules-status.md)]
---

## <a name="review---prepare-for-monitoring-events"></a>查看 - 准备监视事件

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [prepare-monitoring-events](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/prepare-monitoring-events.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [prepare-monitoring-events](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/prepare-monitoring-events.md)]
---

## <a name="run-the-sample-program"></a>运行示例程序

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [run-sample-program](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/run-sample-program.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [run-sample-program](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/run-sample-program.md)]
---

## <a name="interpret-results"></a>解释结果 

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [interpret-results](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/interpret-results.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [interpret-results](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/interpret-results.md)]
---

## <a name="play-the-mp4-clip"></a>播放 MP4 文件剪辑

# <a name="c"></a><a name="programming-language-csharp"></a>[C#](#tab/programming-language-csharp)
[!INCLUDE [play-mp4-clip](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/play-mp4-clip.md)]

# <a name="python"></a><a name="programming-language-python"></a>[Python](#tab/programming-language-python)
[!INCLUDE [play-mp4-clip](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/play-mp4-clip.md)]
---

## <a name="clean-up-resources"></a>清理资源

如果想学习其他快速入门，请保留所创建的资源。 否则，请在 Azure 门户中，转到资源组，选择运行本快速入门所用的资源组，然后删除所有资源。

## <a name="next-steps"></a>后续步骤

* 按照[对自己的模型运行实时视频分析](use-your-model-quickstart.md)快速入门操作，将 AI 应用于实时视频源。
* 查看高级用户面临的其他挑战：

    * 使用支持 RTSP 的 [IP 相机](https://en.wikipedia.org/wiki/IP_camera)，而不是使用 RTSP 模拟器。 可以在 [ONVIF 一致性](https://www.onvif.org/conformant-products)产品页上找到支持 RTSP 的 IP 相机。 查找符合配置文件 G、S 或 T 的设备。
    * 使用 AMD64 或 x64 Linux 设备，而不使用 Azure 中的 Linux VM。 此设备必须与 IP 相机位于同一网络中。 按照[在 Linux 上安装 Azure IoT Edge 运行时](../../iot-edge/how-to-install-iot-edge.md)中的说明进行操作。 然后按照[将首个 IoT Edge 模块部署到虚拟 Linux 设备](../../iot-edge/quickstart-linux.md)中的说明进行操作，将设备注册到 Azure IoT 中心。
