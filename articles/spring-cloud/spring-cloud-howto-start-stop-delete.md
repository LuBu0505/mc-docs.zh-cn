---
title: 启动、停止和删除 Azure Spring Cloud 应用程序 | Microsoft Docs
description: 如何启动、停止和删除 Azure Spring Cloud 应用程序
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 03/23/2021
ms.author: v-junlch
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 44af469a4c8ccee3048b66fb5b7f1cc00081eeef
ms.sourcegitcommit: bed93097171aab01e1b61eb8e1cec8adf9394873
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105602636"
---
# <a name="start-stop-and-delete-your-azure-spring-cloud-application"></a>启动、停止和删除 Azure Spring Cloud 应用程序



本指南说明如何使用 Azure 门户或 Azure CLI 在 Azure Spring Cloud 中更改应用程序的状态。

## <a name="using-the-azure-portal"></a>使用 Azure 门户

部署应用程序后，可以使用 Azure 门户启动、停止和删除应用程序。

1. 转到 Azure 门户中的 Azure Spring Cloud 服务实例。
1. 选择“应用程序仪表板”选项卡。
1. 选择要更改其状态的应用程序。
1. 在该应用程序的“概述”页上，选择“启动/停止”、“重启”或“删除”。

## <a name="using-the-azure-cli"></a>使用 Azure CLI

> [!NOTE]
> 可以使用可选参数，并使用 Azure CLI 配置默认值。 通过阅读[我们的参考文档](/cli/ext/spring-cloud/spring-cloud)详细了解 Azure CLI。  

首先，安装用于 Azure CLI 的 Azure Spring Cloud 扩展，如下所示：

```azurecli
az extension add --name spring-cloud
```

接下来，选择以下任一 Azure CLI 操作：

* 若要启动应用程序：

    ```azurecli
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* 若要停止应用程序：

    ```azurecli
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* 若要重启应用程序：

    ```azurecli
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* 若要删除应用程序：

    ```azurecli
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```
