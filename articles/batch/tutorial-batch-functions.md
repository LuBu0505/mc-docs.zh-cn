---
title: 教程 - 使用 Azure Functions 触发批处理作业
description: 教程 - 在扫描文档添加到存储 Blob 时将 OCR 应用于这些文档
ms.devlang: dotnet
ms.topic: tutorial
ms.service: batch
origin.date: 05/30/2019
author: rockboyfor
ms.date: 01/11/2021
ms.testscope: no
ms.testdate: 08/30/2019
ms.author: v-yeche
ms.custom: mvc, devx-track-csharp
ms.openlocfilehash: 00ac7537977f7616f328ad4e2f46fb4d46957159
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023101"
---
# <a name="tutorial-trigger-a-batch-job-using-azure-functions"></a>教程：使用 Azure Functions 触发批处理作业

在本教程中，将学习如何使用 [Azure Functions](../azure-functions/functions-overview.md) 触发批处理作业。 我们将通过一个示例说明如何利用 Azure Batch 向添加到 Azure 存储 Blob 容器的文档应用光学字符识别 (OCR)。 若要简化 OCR 处理，我们可以配置一个 Azure 函数，该函数在每次将文件添加到 Blob 容器时运行批处理 OCR 作业。 你将学习如何执行以下操作：

> [!div class="checklist"]
> * 使用 Batch Explorer 创建池和作业
> * 使用存储资源管理器创建 Blob 容器和共享访问签名 (SAS)
> * 创建 Blob 触发的 Azure 函数
> * 将输入文件上传到存储
> * 监视任务执行情况
> * 检索输出文件

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果没有 Azure 订阅，请在开始之前创建一个[试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
* Azure Batch 帐户和关联的 Azure 存储帐户。 有关如何创建和链接帐户的详细信息，请参阅[创建批处理帐户](quick-create-portal.md#create-a-batch-account)。
* [Batch Explorer](https://azure.github.io/BatchExplorer/)
* [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)

## <a name="sign-in-to-azure"></a>登录 Azure

登录到 [Azure 门户](https://portal.azure.cn)。

## <a name="create-a-batch-pool-and-batch-job-using-batch-explorer"></a>使用 Batch Explorer 创建批处理池和批处理作业

在本部分中，你将使用 Batch Explorer 创建运行 OCR 任务的批处理池和批处理作业。 

### <a name="create-a-pool"></a>创建池

1. 使用 Azure 凭据登录到 Batch Explorer。
1. 选择左侧栏上的“池”来创建池，然后选择搜索窗体上方的“添加”按钮   。 
    1. 选择 ID 和显示名称。 本示例使用 `ocr-pool`。
    1. 缩放类型设置为“固定大小”，专用节点计数设置为 3  。
    1. 选择“Ubuntu 18.04 LTS”作为操作系统  。
    1. 选择 `Standard_f2s_v2` 作为虚拟机大小。
    1. 启用启动任务，并添加命令 `/bin/bash -c "sudo update-locale LC_ALL=C.UTF-8 LANG=C.UTF-8; sudo apt-get update; sudo apt-get -y install ocrmypdf"`。 请务必将用户标识设置为“任务默认用户 (Admin)”，使启动任务可以包括含 `sudo` 的命令  。
    1. 选择“确定”  。
### <a name="create-a-job"></a>创建作业

1. 选择左侧栏上的“作业”来创建池上的作业，然后选择搜索窗体上方的“添加”按钮   。 
    1. 选择 ID 和显示名称。 本示例使用 `ocr-job`。
    1. 将池设置为 `ocr-pool` 或为池选择其他任何名称。
    1. 选择“确定”  。

## <a name="create-blob-containers"></a>创建 Blob 容器

此处将创建 Blob 容器，用于存储 OCR 批处理作业的输入和输出文件。

1. 使用 Azure 凭据登录到存储资源管理器。
1. 使用链接到批处理帐户的存储帐户，按照[创建 Blob 容器](../vs-azure-tools-storage-explorer-blobs.md#create-a-blob-container)的步骤创建两个 Blob 容器（一个用于输入文件，一个用于输出文件）。

在本例中，输入容器命名为 `input`，所有未应用 OCR 的文档最初都会上传到输入容器进行处理。 输出容器名为 `output`，批处理作业将应用了 OCR 的处理过的文档写入输入容器。  
    * 在本例中，我们将调用输入容器 `input` 和输出容器 `output`。  
    * 输入容器是所有未应用 OCR 的文档最初上传的位置。  
    * 输出容器是批处理作业将应用了 OCR 的文档写入的位置。  

在存储资源管理器中为输出容器创建共享访问签名。 为此，右键单击输出容器并选择“获取共享访问签名...”  。在“权限”下，选中“写入”   。 不需要其他权限。  

## <a name="create-an-azure-function"></a>创建 Azure 函数

在本节中，你将创建 Azure 函数，每当文件上传到输入容器时，该函数就会触发 OCR 批处理作业。

1. 按照[创建由 Azure Blob 存储触发的函数](../azure-functions/functions-create-storage-blob-triggered-function.md)中的步骤创建函数。
    1. 当提示输入存储帐户时，请使用与批处理帐户关联的同一存储帐户。
    1. 对于运行时堆栈，选择“.NET”  。 我们将使用 C# 编写函数，以利用批处理 .NET SDK。
1. 创建 Blob 触发函数后，在函数中使用 GitHub 中的 [`run.csx`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/run.csx) 和 [`function.proj`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/function.proj)。
    * `run.csx` 在将新 Blob 添加到输入 Blob 容器时运行。
    * `function.proj` 列出函数代码中的外部库（例如，批处理 .NET SDK）。
1. 更改 `run.csx` 文件的 `Run()` 函数中变量的占位符值，以反映批处理和存储凭据。 在“Azure 门户”中批处理帐户的“密钥”部分可以找到批处理和存储帐户凭据  。
    * 请在“Azure 门户”中批处理帐户的“密钥”部分检索你的批处理和存储帐户凭据  。 

## <a name="trigger-the-function-and-retrieve-results"></a>触发函数并检索结果

将 GitHub 上的 [`input_files`](https://github.com/Azure-Samples/batch-functions-tutorial/tree/master/input_files) 目录中的任何或所有扫描文件上传到输入容器。 监视 Batch Explorer 以确认每个文件的任务都已添加到 `ocr-pool`。 几秒钟后，将应用了 OCR 的文件添加到输出容器中。 然后，该文件在存储资源管理器上可见并可检索。

此外，可以在 Azure Functions Web 编辑器窗口底部查看日志文件，在此处可以看到每个上传到输入容器的文件的消息，如下所示：

```
2019-05-29T19:45:25.846 [Information] Creating job...
2019-05-29T19:45:25.847 [Information] Accessing input container <inputContainer>...
2019-05-29T19:45:25.847 [Information] Adding <fileName> as a resource file...
2019-05-29T19:45:25.848 [Information] Name of output text file: <outputTxtFile>
2019-05-29T19:45:25.848 [Information] Name of output PDF file: <outputPdfFile>
2019-05-29T19:45:26.200 [Information] Adding OCR task <taskID> for <fileName> <size of fileName>...
```

若要将输出文件从存储资源管理器下载到本地计算机，请首先选择所需的文件，然后在顶部功能区选择“下载”  。 

> [!TIP]
> 如果在 PDF 阅读器中打开下载的文件，则可以对其进行搜索。

## <a name="clean-up-resources"></a>清理资源

只要有节点在运行，就会对池收费，即使没有计划作业。 不再需要池时，请将其删除。 在帐户视图中，选择“池”和池的名称。 然后选择“删除”。 删除池时会删除节点上的所有任务输出。 但是，输出文件保留在存储帐户中。 当不再需要时，还可以删除 Batch 帐户和存储帐户。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：

> [!div class="checklist"]
> * 使用 Batch Explorer 创建池和作业
> * 使用存储资源管理器创建 Blob 容器和共享访问签名 (SAS)
> * 创建 Blob 触发的 Azure 函数
> * 将输入文件上传到存储
> * 监视任务执行情况
> * 检索输出文件


通过 Batch Explorer 在“库”部分浏览可用的渲染应用程序以继续。 每个应用程序都有多个可用的模板，这些模板会随时间而扩展。 例如，Blender 的模板可将单个图像拆分成多个图块，这样就可以并行渲染图像的各个部分。

如需更多示例，以便了解如何使用 .NET API 来计划和处理 Batch 工作负荷，请参阅 GitHub 上的示例。

> [!div class="nextstepaction"]
> [Batch C# 示例](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp)

<!-- Update_Description: update meta properties, wording update, update link -->