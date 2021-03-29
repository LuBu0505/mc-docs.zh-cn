---
title: 快速入门 - 预配 Azure Spring Cloud 服务
description: 介绍如何创建 Azure Spring Cloud 服务实例以实现应用部署。
author: MikeDodaro
ms.author: v-junlch
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 03/23/2021
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 9bea18beac9ee81a276b38b2d8492dbccf305cfe
ms.sourcegitcommit: bed93097171aab01e1b61eb8e1cec8adf9394873
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105602637"
---
# <a name="quickstart-provision-azure-spring-cloud-service"></a>快速入门：预配 Azure Spring Cloud 服务

可以使用 Azure 门户或 Azure CLI 实例化 Azure Spring Cloud。  以下过程介绍了这两种方法。
## <a name="prerequisites"></a>先决条件

* [安装 JDK 8](https://docs.microsoft.com/java/azure/jdk/)
* [注册 Azure 订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn/)
* （可选）[安装 Azure CLI 版本 2.0.67 或更高版本](/cli/install-azure-cli)，并使用以下命令安装 Azure Spring Cloud 扩展：`az extension add --name spring-cloud`
* （可选）[安装 Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) 并[登录](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in)

## <a name="provision-an-instance-of-azure-spring-cloud"></a>预配 Azure Spring Cloud 的实例

#### <a name="portal"></a>[门户](#tab/Azure-portal)

以下步骤使用 Azure 门户创建 Azure Spring Cloud 的实例。

1. 在新选项卡中，打开 [Azure 门户](https://portal.azure.cn/)。 

2. 在顶部搜索框中，搜索“Azure Spring Cloud”。

3. 从结果中选择“Azure Spring Cloud”。

    ![ASC 图标 - 启动](./media/spring-cloud-quickstart-launch-app-portal/find-spring-cloud-start.png)

4. 在“Azure Spring Cloud”页上，单击“+ 添加”。

    ![ASC 图标 - 添加](./media/spring-cloud-quickstart-launch-app-portal/spring-cloud-add.png)

5. 在 Azure Spring Cloud“创建”页中填写表单。  遵循以下指南：
    - 订阅：选择要在其中收取此资源费用的订阅。
    - 资源组：最佳做法是为新资源创建新的资源组。 请注意，后面的步骤会将其用作 \<resource group name\>。
    - **服务详细信息/名称**：指定 \<service instance name\>。  该名称必须为 4 到 32 个字符，只能包含小写字母、数字及连字符。  服务名称的第一个字符必须是字母，最后一个字符必须是字母或数字。
    - 位置：选择服务实例的位置。

    ![ASC 门户启动](./media/spring-cloud-quickstart-launch-app-portal/portal-start.png)

6. 单击“审阅并创建”。

> [!div class="nextstepaction"]
> [我遇到了问题](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=public-endpoint)

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

以下过程使用 Azure CLI 扩展来预配 Azure Spring Cloud 的实例。

1. 登录到 Azure CLI 并选择你的有效订阅。

    ```azurecli
    az login
    az account list -o table
    az account set --subscription <Name or ID of subscription, skip if you only have 1 subscription>
    ```

1. 准备 Azure Spring Cloud 服务的名称。  该名称必须为 4 到 32 个字符，只能包含小写字母、数字及连字符。  服务名称的第一个字符必须是字母，最后一个字符必须是字母或数字。

1. 创建用于包含 Azure Spring Cloud 服务的资源组。

    ```azurecli
    az group create --location chinaeast2 --name <resource group name>
    ```

    详细了解 [Azure 资源组](../azure-resource-manager/management/overview.md)。

1. 打开 Azure CLI 窗口，运行以下命令预配 Azure Spring Cloud 的实例。

    ```azurecli
    az spring-cloud create -n <service instance name> -g <resource group name>
    ```

    部署服务实例需要大约五分钟时间。
---

## <a name="next-steps"></a>后续步骤

在此快速入门中，你创建了 Azure 资源，如果这些资源保留在订阅中，将继续产生费用。 如果不打算继续学习下一个快速入门，请参阅[清理资源](spring-cloud-quickstart-logs-metrics-tracing.md#clean-up-resources)。 否则，请继续学习下一个快速入门：

> [!div class="nextstepaction"]
> [设置配置服务器](spring-cloud-quickstart-setup-config-server.md)
