---
title: Synapse 工作区的持续集成和交付
description: 了解如何使用持续集成和交付将工作区中的更改从一个环境（开发、测试、生产）部署到另一个环境。
services: synapse-analytics
author: WenJason
ms.service: synapse-analytics
ms.topic: conceptual
origin.date: 11/20/2020
ms.date: 03/08/2021
ms.author: v-jay
ms.reviewer: pimorano
ms.openlocfilehash: 37ebb964a66240130d564f1603c48ca1b580c035
ms.sourcegitcommit: 5707919d0754df9dd9543a6d8e6525774af738a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102207096"
---
# <a name="continuous-integration-and-delivery-for-azure-synapse-workspace"></a>Azure Synapse 工作区的持续集成和交付

## <a name="overview"></a>概述

持续集成是每次团队成员将更改提交到版本控制时自动生成和测试代码的过程。 持续部署 (CD) 指从多个测试或过渡环境到生产环境的生成、测试、配置和部署过程。

对于 Azure Synapse 工作区，持续集成和交付 (CI/CD) 将所有实体从一个环境（开发、测试、生产）移到另一个环境。 若要将工作区提升到其他工作区，请完成两个部分：使用 [Azure 资源管理器模板](../../azure-resource-manager/templates/overview.md)创建或更新工作区资源（池和工作区）；在 Azure DevOps 中通过 Synapse CI/CD 工具迁移项目（SQL 脚本、笔记本、Spark 作业定义、管道、数据集、数据流等）。 

本文概述如何使用 Azure 发布管道将 Synapse 工作区自动部署到多个环境。

## <a name="prerequisites"></a>先决条件

-   用于开发的工作区已在 Studio 中使用 Git 存储库进行配置，请参阅 [Synapse Studio 中的源代码管理](source-control.md)。
-   Azure DevOps 项目已准备好运行发布管道。

## <a name="set-up-a-release-pipelines"></a>设置发布管道

1.  在 [Azure DevOps](https://dev.azure.com/)中，打开为发布创建的项目。

1.  在页面左侧选择“管道”，然后选择“发布”。 

    ![依次选择“管道”、“发布”](media/create-release-1.png)

1.  选择“新建管道”；如果有现有的管道，请依次选择“新建”、“新建发布管道”。  

1.  选择“空作业”模板。

    ![选择“空作业”](media/create-release-select-empty.png)

1.  在“阶段名称”框中，输入环境的名称。

1.  选择“添加项目”，然后选择配置了开发 Synapse Studio 的 git 存储库。 选择用于管理池和工作区的 ARM 模板的 git 存储库。 如果使用 GitHub 作为源，需要为 GitHub 帐户和拉取存储库创建服务连接。 了解有关[服务连接](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints)的信息 

    ![添加发布分支](media/release-creation-github.png)

1.  选择用于资源更新的 ARM 模板分支。 对于“默认版本”，请选择“默认分支中的最新版本”。 

    ![添加 ARM 模板](media/release-creation-arm-branch.png)

1.  选择“默认分支”对应的存储库的[发布分支](source-control.md#configure-publishing-settings)。 默认情况下，此发布分支为 `workspace_publish`。 对于“默认版本”，请选择“默认分支中的最新版本”。 

    ![添加项目](media/release-creation-publish-branch.png)

## <a name="set-up-a-stage-task-for-arm-resource-create-and-update"></a>为 ARM 资源创建和更新设置阶段任务 

添加 Azure 资源管理器部署任务来创建或更新资源，包括工作区和池：

1. 在阶段视图中选择“查看阶段任务”。

    ![阶段视图](media/release-creation-stage-view.png)

1. 创建新任务。 搜索“ARM 模板部署”，然后选择“添加”。 

1. 在“部署任务”中，选择目标工作区的订阅、资源组和位置。 根据需要提供凭据。

1. 在“操作”列表中，选择“创建或更新资源组”。 

1. 选择“模板”框旁边的省略号按钮 ( **...** )。 浏览目标工作区的 Azure 资源管理器模板

1. 选择“模板参数”框旁边 的“…”，以便选择参数文件。

1. 在“替代模板参数”框旁边 （位于“替代模板参数”框旁边），并输入目标工作区的所需参数值。 

1. 选择“增量”作为“部署模式”。 
    
    ![工作区和池部署](media/pools-resource-deploy.png)

1. 根据需要添加 Azure PowerShell 来授予和更新工作区角色分配。 如果使用发布管道创建 Synapse 工作区，则会将管道的服务主体添加为默认工作区管理员。可以运行 PowerShell 来向其他帐户授予对工作区的访问权限。 
    
    ![授予权限](media/release-creation-grant-permission.png)

 > [!WARNING]
> 在完整部署模式下，将删除资源组中已存在但尚未在新资源管理器模板中指定的资源。 有关详细信息，请参阅 [Azure 资源管理部署模式](../../azure-resource-manager/templates/deployment-modes.md)

## <a name="set-up-a-stage-task-for-artifacts-deployment"></a>为项目部署设置阶段任务 

使用 [Synapse 工作区部署](https://marketplace.visualstudio.com/items?itemName=AzureSynapseWorkspace.synapsecicd-deploy)扩展在 Synapse 工作区中部署其他项，例如数据集、SQL 脚本、笔记本、spark 作业定义、数据流、管道、链接服务、凭据和 IR（集成运行时）。  

1. 搜索并从 Azure DevOps 市场 (https://marketplace.visualstudio.com/azuredevops) 获取扩展 

     ![获取扩展](media/get-extension-from-market.png)

1. 选择要安装扩展的组织。 

     ![安装扩展](media/install-extension.png)

1. 请确保已向 Azure DevOps 管道的服务主体授予订阅的权限，并将其分配为目标工作区的工作区管理员。 

1. 创建新任务。  搜索“Synapse 工作区部署”，然后选择“添加”。

     ![添加扩展](media/add-extension-task.png)

1.  在任务中选择“...” （位于“模板”框旁边）以选择模板文件。

1. 选择“模板参数”框旁边 的“…”，以便选择参数文件。

1. 选择连接、资源组和目标工作区的名称。 

1. 在“替代模板参数”框旁边 （位于“替代模板参数”框旁边），并输入目标工作区的所需参数值。 

    ![Synapse 工作区部署](media/create-release-artifacts-deployment.png)

> [!IMPORTANT]
> 在 CI/CD 方案中，不同环境中的集成运行时 (IR) 类型必须相同。 例如，如果在开发环境中使用自承载 IR，则同一 IR 在测试和生产等其他环境中的类型也必须是自承载。 同样，如果跨多个阶段共享集成运行时，则必须在所有环境（例如开发、测试和生产）中将集成运行时配置为链接自承载。

## <a name="create-release-for-deployment"></a>创建发布进行部署 

保存所有更改后，可以选择“创建发布”以手动创建发布。 若要自动创建发布，请参阅 [Azure DevOps 发布触发器](https://docs.microsoft.com/azure/devops/pipelines/release/triggers)

   ![选择“创建发布”](media/release-creation-manually.png)

## <a name="best-practices-for-cicd"></a>CI/CD 最佳做法

如果使用 Synapse 工作区的 Git 集成，并且某个 CI/CD 管道会将更改从开发环境依次转移到测试和生产环境，则我们建议采用以下最佳做法：

-   **Git 集成**。 仅配置具有 Git 集成的开发 Synapse 工作区。 对测试和生产工作区所做的更改将通过 CI/CD 进行部署，不需要 Git 集成。
-   **在项目迁移之前准备池**。 如果已将 SQL 脚本或笔记本附加到开发工作区中的池，则需要在不同环境中使用相同的池名称。 
-   **基础结构即代码 (IaC)** 。 在描述性模型中管理基础结构（网络、虚拟机、负载均衡器和连接拓扑），使用与 DevOps 团队所用相同的版本控制来管理源代码。 

## <a name="troubleshooting-artifacts-deployment"></a>排查项目部署问题 

### <a name="use-the-synapse-workspace-deployment-task"></a>使用 Synapse 工作区部署任务

在 Synapse 中，有许多不是 ARM 资源的项目。 不同于 Azure 数据工厂。 ARM 模板部署任务无法正常工作来部署 Synapse 项目
 
### <a name="unexpected-token-error-in-release"></a>发布中出现意外的标记错误

如果参数文件包含未转义的参数值，则发布管道将无法分析文件，并生成错误“意外标记”。 建议替代参数或使用 Azure KeyVault 检索参数值。 还可以使用双转义符作为变通方法。
