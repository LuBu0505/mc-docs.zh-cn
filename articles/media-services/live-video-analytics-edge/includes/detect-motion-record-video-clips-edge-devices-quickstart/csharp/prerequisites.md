---
ms.openlocfilehash: adc2826147aa150a3643b9ee8aa48a183f1ecd9c
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99213995"
---
* 具有活动订阅的 Azure 帐户。 如果没有帐户，可[创建试用帐户](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
  > [!NOTE]
  > 你将需要具有创建服务主体权限的 Azure 订阅（所有者角色提供了此权限）。 如果你没有正确的权限，请联系帐户管理员，让其授予你适当的权限。 
* 包含以下扩展的 [Visual Studio Code](https://code.visualstudio.com/)：
    * [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
        > [!TIP]
        > 在安装 Azure IoT Tools 时，系统可能会提示安装 Docker。 可以忽略此提示。
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。
* 如果尚未完成[检测运动并发出事件](../../../detect-motion-emit-events-quickstart.md)快速入门，请按照以下步骤操作：
     1. [设置 Azure 资源](../../../detect-motion-emit-events-quickstart.md#set-up-azure-resources)
     1. [设置开发环境](../../../detect-motion-emit-events-quickstart.md#set-up-your-development-environment)
     1. [生成并部署 IoT Edge 部署清单](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest)
     1. [准备监视事件](../../../detect-motion-emit-events-quickstart.md#prepare-to-monitor-events)

> [!TIP]
> 如果在创建 Azure 资源时遇到问题，请查看[故障排除指南](../../../troubleshoot-how-to.md#common-error-resolutions)来解决一些常见问题。