---
ms.openlocfilehash: 36659d4454c7809ad76f07cc39cbd445e4be89c4
ms.sourcegitcommit: a978c5f2c6b53494d67e7c3c5a44b2aa648219a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2021
ms.locfileid: "98633495"
---
通过使用 OUTPUT_VIDEO_FOLDER_ON_DEVICE 密钥，将 MP4 文件写入在 .env 文件中配置的边缘设备上的目录中。 如果使用了默认值，则结果应位于 /var/media/ 文件夹中。

播放 MP4 文件剪辑：

1. 转到资源组，找到 VM，然后进行连接。

    ![资源组](../../../media/quickstarts/resource-group.png)
    
    ![VM](../../../media/quickstarts/virtual-machine.png)
1. 使用[设置 Azure 资源](../../../detect-motion-emit-events-quickstart.md#set-up-azure-resources)时生成的凭据登录。 
1. 在命令提示符下，转到相关目录。 默认位置为 /var/media。 应会在目录中看到 MP4 文件。

    ![输出](../../../media/quickstarts/samples-output.png) 

1. 使用[安全复制 (SCP)](../../../../../virtual-machines/linux/copy-files-to-linux-vm-using-scp.md) 将文件复制到本地计算机。 
1. 使用 [VLC 媒体播放器](https://www.videolan.org/vlc/)或任何其他 MP4 文件播放器播放这些文件。
