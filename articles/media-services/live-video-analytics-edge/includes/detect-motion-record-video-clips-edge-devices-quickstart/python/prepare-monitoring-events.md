---
ms.openlocfilehash: bc211d7dbfc4ff96499e77259d29e847a260e8d4
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750003"
---
请确保已完成[准备监视事件](../../../detect-motion-emit-events-quickstart.md#prepare-to-monitor-events)的步骤。

![开始监视内置事件终结点](../../../media/quickstarts/start-monitoring-iothub-events.png)

> [!NOTE]
> 系统可能会要求你提供 IoT 中心的内置终结点信息。 若要获取此信息，请在 Azure 门户中导航到 IoT 中心，然后在左侧导航窗格中查找“内置终结点”选项。 单击此处，在“与事件中心兼容的终结点”部分下查找“与事件中心兼容的终结点” 。 复制并使用框中的文本。 终结点将如下所示：  
    ```
    Endpoint=sb://iothub-ns-xxx.servicebus.chinacloudapi.cn/;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX;EntityPath=<IoT Hub name>
    ```
