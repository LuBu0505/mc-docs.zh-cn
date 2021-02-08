---
title: 使用 Azure CLI 和 IoT 扩展管理 IoT 中心设备预配服务
description: 了解如何使用 Azure CLI 和 IoT 扩展来管理 IoT 中心设备预配服务 (DPS)
author: chrissie926
ms.author: v-tawe
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: iot-dps
ms.custom: devx-track-azurecli
services: iot-dps
ms.openlocfilehash: 215c4eac115a106479831c69898649ed578c75b3
ms.sourcegitcommit: dc0d10e365c7598d25e7939b2c5bb7e09ae2835c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99579372"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>如何使用 Azure CLI 和 IoT 扩展来管理 IoT 中心设备预配服务

[Azure CLI](https://docs.azure.cn/cli) 是一个开源跨平台命令行工具，用于管理 IoT Edge 等 Azure 资源。 Azure CLI 适用于 Windows、Linux 和 macOS。 使用 Azure CLI 可以管理 Azure IoT 中心资源、设备预配服务实例和现成的链接中心。

IoT 扩展丰富了 Azure CLI 的功能，例如设备管理和完整的 IoT Edge 功能。

在本教程中，我们先完成设置 Azure CLI 和 IoT 扩展的步骤。 然后了解如何运行 CLI 命令来执行基本的设备预配服务操作。 

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>先决条件

- 需要 [Python 2.7x 或 Python 3.x](https://www.python.org/downloads/)。

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- 本文需要 Azure CLI 版本 2.0.70 或更高版本。

## <a name="basic-device-provisioning-service-operations"></a>基本的设备预配服务操作

该示例演示如何使用 CLI 命令登录到 Azure 帐户、创建 Azure 资源组（保存 Azure 解决方案相关资源的容器）、创建 IoT 中心、创建设备预配服务、列出现有的设备预配服务，以及如何创建链接的 IoT 中心。 

在开始之前，请完成前面所述的安装步骤。 如果还没有 Azure 帐户，可以立即[创建一个试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。 


### <a name="1-log-in-to-the-azure-account"></a>1.登录到 Azure 帐户
  
```azurecli
az login
```

![屏幕截图显示了一个运行 az login 命令的命令提示符窗口。](./media/how-to-manage-dps-with-cli/login.jpg)

### <a name="2-create-a-resource-group-iothubblogdemo-in-chinanorth"></a>2.在 chinanorth 中创建资源组 IoTHubBlogDemo

```azurecli
az group create -l chinanorth -n IoTHubBlogDemo
```

![创建资源组](./media/how-to-manage-dps-with-cli/create-resource-group.jpg)


### <a name="3-create-two-device-provisioning-services"></a>3.创建两个设备预配服务

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps
```

![创建设备预配服务](./media/how-to-manage-dps-with-cli/create-dps.jpg)

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps2
```

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4.列出此资源组下的所有现有设备预配服务

```azurecli
az iot dps list --resource-group IoTHubBlogDemo
```

![列出设备预配服务](./media/how-to-manage-dps-with-cli/list-dps.jpg)


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5.在新建的资源组下创建 IoT 中心 blogDemoHub

```azurecli
az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo
```

![创建 IoT 中心](./media/how-to-manage-dps-with-cli/create-hub.jpg)

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6.将一个现有的 IoT 中心链接到设备预配服务

```azurecli
az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l chinaeast
```

![链接中心](./media/how-to-manage-dps-with-cli/create-hub.jpg)

## <a name="next-steps"></a>后续步骤
在本教程中，你了解了如何执行以下操作：

> [!div class="checklist"]
> * 注册设备
> * 启动设备
> * 验证设备已注册

前往下一教程，了解如何跨负载均衡的中心预配多台设备。 

> [!div class="nextstepaction"]
> [跨负载均衡的 IoT 中心预配设备](./tutorial-provision-multiple-hubs.md)
