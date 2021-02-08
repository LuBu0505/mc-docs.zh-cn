---
title: 渲染功能
description: 标准 Azure Batch 功能用于运行渲染工作负荷与应用。 Batch 包含用于支持渲染工作负荷的特定功能。
origin.date: 01/14/2021
author: rockboyfor
ms.date: 02/01/2021
ms.testscope: no
ms.testdate: 09/07/2018
ms.author: v-yeche
ms.topic: how-to
ms.service: batch
ms.openlocfilehash: dfd735bb63b20961e15e2fa36b38524da12f6fb6
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059510"
---
# <a name="azure-batch-rendering-capabilities"></a>Azure Batch 的渲染功能

标准 Azure Batch 功能用于运行渲染工作负荷与应用程序。 Batch 还包含用于支持渲染工作负荷的特定功能。

有关 Batch 概念的概述，包括池、作业和任务，请参阅[此文](./batch-service-workflow-features.md)。

## <a name="batch-pools-using-custom-vm-images-and-standard-application-licensing"></a>使用自定义 VM 映像和标准应用程序许可的 Batch 池

与其他工作负荷和其他类型的应用程序一样，可以使用所需的渲染应用程序和插件创建自定义 VM 映像。自定义 VM 映像位于[共享映像库](../virtual-machines/shared-image-galleries.md)中并且[可用于创建 Batch 池](batch-sig-images.md)。

任务命令行字符串将需要引用创建自定义 VM 映像时使用的应用程序和路径。

大多数渲染应用程序都需要从许可证服务器获取的许可证。 如果存在现有的本地许可证服务器，则池和许可证服务器都需要位于同一[虚拟网络](../virtual-network/virtual-networks-overview.md)上。 还可以在 Azure VM 上运行许可证服务器，将 Batch 池和许可证服务器 VM 置于同一虚拟网络中。

## <a name="batch-pools-using-rendering-vm-images"></a>使用渲染 VM 映像的 Batch 池

### <a name="rendering-application-installation"></a>渲染应用程序安装

如果只需使用预装的应用程序，则可以在池配置中指定 Azure 市场渲染 VM 映像。

有一个 Windows 2016 映像和一个 CentOS 映像。  在 [Azure 市场](https://market.azure.cn/)中，可以通过搜索“batch 渲染”找到 VM 映像。

有关示例池配置，请参阅 [Azure CLI 渲染教程](./tutorial-rendering-cli.md)。  Azure 门户和 Batch Explorer 提供了 GUI 工具用于在创建池时选择渲染 VM 映像。  如果使用 Batch API，请在创建池时，为 [ImageReference](https://docs.microsoft.com/rest/api/batchservice/pool/add#imagereference) 指定以下属性值：

| 发布者 | 产品/服务 | SKU | 版本 |
|---------|---------|---------|--------|
| 批处理 | rendering-centos73 | 呈现 | 最新 |
| 批处理 | rendering-windows2016 | 呈现 | 最新 |

如果池 VM 上需要其他应用程序，则可以使用其他选项：

* 共享映像库中的自定义映像
    * 可以使用此选项为 VM 配置所需的具体应用程序和版本。 有关详细信息，请参阅[使用共享映像库创建池](batch-sig-images.md)。 Autodesk 和 Chaos Group 已分别修改了 Arnold 和 V-Ray，可以验证 Azure Batch 许可服务。 请确保这些应用程序的版本提供此支持，否则，即用即付许可模式将不适用。 运行无头模式（批处理/命令行模式）时，最新版本的 Maya 或 3ds Max 不需要许可证服务器。 如果不确定如何使用此选项，请联系 Azure 支持部门。
* [应用程序包](./batch-application-packages.md)：
    * 使用一个或多个 ZIP 文件打包应用程序文件，通过 Azure 门户上传，然后在池配置中指定该包。 创建池 VM 时，将下载 ZIP 文件并解压缩文件。
* 资源文件：
    * 应用程序文件将上传到 Azure Blob 存储；在[池启动任务](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)中指定文件引用。 创建池 VM 时，会将资源文件下载到每个 VM。

### <a name="pay-for-use-licensing-for-pre-installed-applications"></a>预装应用程序的即用即付许可

需在池配置中指定要使用的并且会产生许可费的应用程序。

* [创建池](https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body)时指定 `applicationLicenses` 属性。  可在字符串数组中指定以下值 -“vray”、“arnold”、“3dsmax”、“maya”。
* 指定一个或多个应用程序时，这些应用程序的费用将与 VM 费用相加。  [Azure Batch 定价页面](https://www.azure.cn/pricing/details/batch/#graphic-rendering)上列出了应用程序价格。

> [!NOTE]
> 若改为通过连接到许可证服务器来使用渲染应用程序，则不要指定 `applicationLicenses` 属性。

可以使用 Azure 门户或 Batch Explorer 选择应用程序和显示应用程序价格。

如果尝试使用某个应用程序，但尚未在池配置的 `applicationLicenses` 属性中指定该应用程序，或未连接许可证服务器，则应用程序执行会失败，并出现许可错误和非零退出代码。

### <a name="environment-variables-for-pre-installed-applications"></a>预装应用程序的环境变量

若要为渲染任务创建命令行，必须指定渲染应用程序可执行文件的安装位置。  Azure 市场 VM 映像中已创建系统环境变量，可以改用这些环境变量，而无需指定实际的路径。  这些环境变量补充了为每个任务创建的[标准 Batch 环境变量](./batch-compute-node-environment-variables.md)。

|应用程序|应用程序可执行文件|环境变量|
|---------|---------|---------|
|Autodesk 3ds Max 2018|3dsmaxcmdio.exe|3DSMAX_2018_EXEC|
|Autodesk 3ds Max 2019|3dsmaxcmdio.exe|3DSMAX_2019_EXEC|
|Autodesk Maya 2017|render.exe|MAYA_2017_EXEC|
|Autodesk Maya 2018|render.exe|MAYA_2018_EXEC|
|Chaos Group V-Ray Standalone|vray.exe|VRAY_3.60.4_EXEC|
|Arnold 2017 命令行|kick.exe|ARNOLD_2017_EXEC|
|Arnold 2018 命令行|kick.exe|ARNOLD_2018_EXEC|
|Blender|blender.exe|BLENDER_2018_EXEC|

## <a name="azure-vm-families"></a>Azure VM 系列

与其他工作负荷一样，渲染应用程序的系统要求和性能要求根据作业与项目的不同而异。  Azure 中根据要求提供了多种不同的 VM 系列 - 最低成本、最高性价比、最佳性能，等等。
有些渲染应用程序（例如 Arnold）基于 CPU，而有些（例如 V-Ray 和 Blender Cycles）则可以使用 CPU 和/或 GPU。
有关可用 VM 系列和 VM 大小的说明，请参阅 [VM 类型和大小](../virtual-machines/sizes.md)。

<!--Not Available on ## Low-priority VMs-->

## <a name="jobs-and-tasks"></a>作业和任务

作业和任务不需要特定于渲染的支持。  主要配置项是任务命令行，它需要引用所需的应用程序。
使用 Azure 市场 VM 映像时，最好是使用环境变量来指定路径和应用程序可执行文件。

## <a name="next-steps"></a>后续步骤

有关 Batch 渲染的示例，请尝试学习以下两篇教程：

* [使用 Azure CLI 进行渲染](./tutorial-rendering-cli.md)
* [使用 Batch Explorer 进行渲染](./tutorial-rendering-batchexplorer-blender.md)

<!-- Update_Description: update meta properties, wording update, update link -->