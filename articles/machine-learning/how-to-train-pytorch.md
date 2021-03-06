---
title: 训练深度学习 PyTorch 模型
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习 PyTorch 估算器类在企业范围内运行 PyTorch 训练脚本。  示例脚本对鸡和火鸡图像进行分类，以基于 PyTorch 的迁移学习教程构建深度学习神经网络。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: peterlu
author: mx-iao
ms.reviewer: peterlu
ms.date: 09/28/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 1db5d640b4fa3c90aa25ec6a3efb09f745b4f9da
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98229985"
---
# <a name="train-pytorch-models-at-scale-with-azure-machine-learning"></a>使用 Azure 机器学习大规模训练 PyTorch 模型

本文介绍了如何使用 Azure 机器学习在企业范围内运行 [PyTorch](https://pytorch.org/) 训练脚本。

本文中的示例脚本用来对鸡和火鸡图像进行分类，以基于 PyTorch 的迁移学习[教程](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)构建深度学习神经网络 (DNN)。 迁移学习是一种将解决某个问题时获得的知识应用于虽然不同但却相关的问题的技术。 与从头开始训练相比，这需要较少的数据、时间和计算资源，从而简化了训练过程。

无论是从头开始训练深度学习 PyTorch 模型，还是将现有模型引入云中，都可以通过 Azure 机器学习使用弹性云计算资源来横向扩展开源训练作业。 你可以通过 Azure 机器学习来构建、部署和监视生产级模型以及对其进行版本控制。 

## <a name="prerequisites"></a>先决条件

在以下任一环境中运行此代码：

- Azure 机器学习计算实例 - 无需下载或安装

    - 在开始本教程之前完成[教程：设置环境和工作区](tutorial-1st-experiment-sdk-setup.md)创建预先加载了 SDK 和示例存储库的专用笔记本服务器。
    - 在笔记本服务器上的示例深度学习文件夹中，通过导航到以下目录，找到已完成且已展开的笔记本：**how-to-use-azureml > ml-frameworks > pytorch > train-hyperparameter-tune-deploy-with-pytorch** 文件夹。 
 
 - 你自己的 Jupyter 笔记本服务器
    - [安装 Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py) (>= 1.15.0)。
    - [创建工作区配置文件](how-to-configure-environment.md#workspace)。
    - [下载示例脚本文件](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch) `pytorch_train.py`
     
    此外还可以在 GitHub 示例页上查找本指南的完整 [Jupyter Notebook 版本](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb)。 该笔记本包含扩展部分，其中涵盖智能超参数优化、模型部署和笔记本小组件。

## <a name="set-up-the-experiment"></a>设置试验

本部分通过加载所需的 Python 包、初始化工作区、创建计算目标和定义训练环境来设置训练实验。

### <a name="import-packages"></a>导入程序包

首先，导入必需的 Python 库。

```Python
import os
import shutil

from azureml.core.workspace import Workspace
from azureml.core import Experiment
from azureml.core import Environment

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>初始化工作区

[Azure 机器学习工作区](concept-workspace.md)是服务的顶级资源。 它提供了一个集中的位置来处理创建的所有项目。 在 Python SDK 中，可以通过创建 [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?preserve-view=true&view=azure-ml-py) 对象来访问工作区项目。

根据在[先决条件部分](#prerequisites)中创建的 `config.json` 文件创建工作区对象。

```Python
ws = Workspace.from_config()
```

### <a name="get-the-data"></a>获取数据

该数据集针对火鸡和鸡分别包含了大约 120 个训练图像，每个类有 100 个验证图像。 在训练脚本 `pytorch_train.py` 中，我们将下载并提取该数据集。 这些图像是 [Open Images v5 Dataset](https://storage.googleapis.com/openimages/web/index.html) 的子集。

### <a name="prepare-training-script"></a>准备训练脚本

在本教程中，已提供了训练脚本 `pytorch_train.py`。 实际上，你可以原样接受任何自定义的训练脚本，并使用 Azure 机器学习运行它。

为训练脚本创建一个文件夹。

```python
project_folder = './pytorch-birds'
os.makedirs(project_folder, exist_ok=True)
shutil.copy('pytorch_train.py', project_folder)
```

### <a name="create-a-compute-target"></a>创建计算目标

创建用于运行 PyTorch 作业的计算目标。 在此示例中，创建启用了 GPU 的 Azure 机器学习计算群集。

```Python
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

有关计算目标的详细信息，请参阅[什么是计算目标](concept-compute-target.md)一文。

### <a name="define-your-environment"></a>定义环境

若要定义封装训练脚本依赖项的 Azure ML [环境](concept-environments.md)，可以定义自定义环境或使用和 Azure ML 特选环境。

#### <a name="use-a-curated-environment"></a>使用特选环境
如果不想定义自己的环境，Azure ML 会提供预生成的特选环境。 Azure ML 具有几个适用于 PyTorch 的 CPU 和 GPU 特选环境，这些环境对应不同版本的 PyTorch。 有关详细信息，请参阅[此文](resource-curated-environments.md)。

若要使用特选环境，可以改为运行以下命令：

```python
curated_env_name = 'AzureML-PyTorch-1.6-GPU'
pytorch_env = Environment.get(workspace=ws, name=curated_env_name)
```

若要查看特选环境中包含的包，可以将 conda 依赖项写入磁盘：
```python
pytorch_env.save_to_directory(path=curated_env_name)
```

确保特选环境包括训练脚本所需的所有依赖项。 如果没有，则必须修改环境以包含缺少的依赖项。 请注意，如果修改了环境，则必须为它提供新名称，因为“AzureML”前缀是为特选环境保留的。 如果修改了 conda 依赖项 YAML 文件，则可以使用新名称从该文件创建新环境，例如：
```python
pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')
```

如果改为直接修改了特选环境对象，则可以使用新名称克隆该环境：
```python
pytorch_env = pytorch_env.clone(new_name='pytorch-1.6-gpu')
```

#### <a name="create-a-custom-environment"></a>创建自定义环境

还可以创建自己的 Azure ML 环境，以封装训练脚本的依赖项。

首先，在 YAML 文件中定义 conda 依赖项；在本例中，该文件名为 `conda_dependencies.yml`。

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - future==0.17.1
  - pillow
```

基于此 conda 环境规范创建 Azure ML 环境。 此环境将在运行时打包到 Docker 容器中。

在默认情况下，如果未指定基础映像，Azure ML 将使用 CPU 映像 `azureml.core.environment.DEFAULT_CPU_IMAGE` 作为基础映像。 由于本示例在 GPU 群集上运行训练，因此你需要指定具有必要 GPU 驱动程序和依赖项的 GPU 基础映像。 Azure ML 维护一组在 Microsoft 容器注册表 (MCR) 上发布的基础映像，你可以使用这些映像，请参阅 [Azure/AzureML 容器](https://github.com/Azure/AzureML-Containers) GitHub 存储库获取详细信息。

```python
pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')

# Specify a GPU base image
pytorch_env.docker.enabled = True
pytorch_env.docker.base_image = 'mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04'
```

> [!TIP]
> 或者，也可以直接在自定义 Docker 映像或 Dockerfile 中捕获所有依赖项，然后从中创建环境。 有关详细信息，请参阅[通过自定义映像进行训练](how-to-train-with-custom-image.md)。

有关创建和使用环境的详细信息，请参阅[在 Azure 机器学习中创建和使用软件环境](how-to-use-environments.md)。

## <a name="configure-and-submit-your-training-run"></a>配置和提交训练运行

### <a name="create-a-scriptrunconfig"></a>创建 ScriptRunConfig

创建一个 [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py) 对象，以指定训练作业的配置详细信息，包括训练脚本、要使用的环境，以及要在其上运行的计算目标。 如果在 `arguments` 参数中指定，训练脚本的任何参数都将通过命令行传递。 

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_train.py',
                      arguments=['--num_epochs', 30, '--output_dir', './outputs'],
                      compute_target=compute_target,
                      environment=pytorch_env)
```

> [!WARNING]
> Azure 机器学习通过复制整个源目录来运行训练脚本。 如果你有不想上传的敏感数据，请使用 [.ignore 文件](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots)或不将其包含在源目录中。 改为使用 Azure ML [数据集](how-to-train-with-datasets.md)来访问数据。

有关通过 ScriptRunConfig 配置作业的详细信息，请参阅[配置并提交训练运行](how-to-set-up-training-targets.md)。

> [!WARNING]
> 如果你以前使用 PyTorch 估算器来配置 PyTorch 训练作业，请注意，自 1.19.0 SDK 发行版起，该估算器已弃用。 对于不低于 1.15.0 版本的 Azure ML SDK，建议使用 ScriptRunConfig 作为配置训练作业（包括使用深度学习框架的作业）的方法。 有关常见的迁移问题，请参阅[估算器到 ScriptRunConfig 迁移指南](how-to-migrate-from-estimators-to-scriptrunconfig.md)。

## <a name="submit-your-run"></a>提交运行

[运行对象](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py)在作业运行时和运行后提供运行历史记录的接口。

```Python
run = Experiment(ws, name='Tutorial-pytorch-birds').submit(src)
run.wait_for_completion(show_output=True)
```

### <a name="what-happens-during-run-execution"></a>在运行执行过程中发生的情况
执行运行时，会经历以下阶段：

- **准备**：根据所定义的环境创建 docker 映像。 将映像上传到工作区的容器注册表，缓存以用于后续运行。 还会将日志流式传输到运行历史记录，可以查看日志以监视进度。 如果改为指定特选环境，则会使用支持该特选环境的缓存映像。

- **缩放**：如果 Batch AI 群集执行运行所需的节点多于当前可用节点，则群集将尝试纵向扩展。

- **正在运行**：将脚本文件夹中的所有脚本上传到计算目标，装载或复制数据存储，然后执行 `script`。 将 stdout 和 ./logs 文件夹中的输出流式传输到运行历史记录，即可将其用于监视运行。

- **后期处理**：将运行的 ./outputs 文件夹复制到运行历史记录。

## <a name="register-or-download-a-model"></a>注册或下载模型

训练模型后，可以将其注册到工作区。 凭借模型注册，可以在工作区中存储模型并对其进行版本控制，从而简化[模型管理和部署](concept-model-management-and-deployment.md)。

```Python
model = run.register_model(model_name='pytorch-birds', model_path='outputs/model.pt')
```

> [!TIP]
> 部署指南包含有关模型注册的部分，但由于你已有一个已注册的模型，因而可以直接跳到[创建计算目标](how-to-deploy-and-where.md#choose-a-compute-target)进行部署。

此外，还可以使用“运行”对象下载模型的本地副本。 在训练脚本 `pytorch_train.py` 中，一个 PyTorch“保存”对象将模型保存到本地文件夹（计算目标的本地）。 可以使用“运行”对象下载副本。

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

# Download the model from run history
run.download_file(name='outputs/model.pt', output_file_path='./model/model.pt'), 
```

## <a name="distributed-training"></a>分布式训练

Azure 机器学习还支持多节点分布式 PyTorch 作业，以便可以缩放训练工作负荷。 你可以轻松运行分布式 PyTorch 作业，Azure ML 将为你管理业务流程。

Azure ML 支持使用 Horovod 和 PyTorch 的内置 DistributedDataParallel 模块来运行分布式 PyTorch 作业。

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) 是 Uber 开发的用于分布式训练的开放源代码 all reduce 框架。 它提供了一个简单的路径来编写分布式 PyTorch 代码进行训练。

训练代码必须使用 Horovod 检测，以进行分布式训练。 有关将 Horovod 与 PyTorch 配合使用的详细信息，请参阅 [Horovod 文档](https://horovod.readthedocs.io/en/stable/pytorch.html)。

此外，请确保训练环境包含 horovod 包。 如果你使用的是 PyTorch 特选环境，则 horovod 已作为依赖项之一包含在内。 如果使用自己的环境，请确保包含 horovod 依赖项，例如：

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - horovod==0.19.5
```

若要在 Azure ML 上使用 MPI/Horovod 执行分布式作业，必须指定到 ScriptRunConfig 构造函数的 `distributed_job_config` 参数的 [MpiConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?preserve-view=true&view=azure-ml-py)。 以下代码将配置每个节点运行一个进程的 2 节点分布式作业。 如果你还希望每个节点运行多个进程，（即，如果群集 SKU 有多个 GPU），请在 MpiConfiguration 中另外指定 `process_count_per_node` 参数（默认值为 `1`）。

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import MpiConfiguration

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_horovod_mnist.py',
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=MpiConfiguration(node_count=2))
```

有关如何在 Azure ML 上使用 Horovod 运行分布式 PyTorch 的完整教程，请参阅[使用 Horovod 的分布式 PyTorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-horovod)。

### <a name="distributeddataparallel"></a>DistributedDataParallel
如果你使用的是 PyTorch 的内置 [DistributedDataParallel](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html) 模块（在训练代码中使用 **torch.distributed** 包生成），也可以通过 Azure ML 来启动分布式作业。

若要使用 DistributedDataParallel 运行分布式 PyTorch 作业，请将 [PyTorchConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration?preserve-view=true&view=azure-ml-py) 指定到 ScriptRunConfig 构造函数的 `distributed_job_config` 参数。 若要将 NCCL 后端用于 torch.distributed，请在 PyTorchConfiguration 中指定 `communication_backend='Nccl'`。 以下代码将配置一个 2 节点分布式作业。 对于 PyTorch 分布式 GPU 训练，建议使用 NCCL 后端作为后端。

对于通过 PyTorchConfiguration 配置的分布式 PyTorch 作业，Azure ML 将在计算目标的节点上设置以下环境变量：

* `AZ_BATCHAI_PYTORCH_INIT_METHOD`：进程组的共享文件系统初始化的 URL
* `AZ_BATCHAI_TASK_INDEX`：工作进程的全局排名

可以通过 ScriptRunConfig 的 `arguments` 参数将这些环境变量指定到训练脚本的相应参数。

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import PyTorchConfiguration

args = ['--dist-backend', 'nccl',
        '--dist-url', '$AZ_BATCHAI_PYTORCH_INIT_METHOD',
        '--rank', '$AZ_BATCHAI_TASK_INDEX',
        '--world-size', 2]

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_mnist.py',
                      arguments=args,
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=PyTorchConfiguration(communication_backend='Nccl', node_count=2))
```

如果要改用 Gloo 后端来进行分布式训练，请改为指定 `communication_backend='Gloo'`。 对于分布式 CPU 训练，建议使用 Gloo 后端。

有关如何在 Azure ML 上运行分布式 PyTorch 的完整教程，请参阅[使用 DistributedDataParallel 的分布式 PyTorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-nccl-gloo)。

### <a name="troubleshooting"></a>疑难解答

* **Horovod 已关闭**：在大多数情况下，如果遇到“AbortedError:Horovod 已关闭”，则表示某个进程中存在潜在异常，导致 Horovod 关闭。 MPI 作业中的每个排名都会在 Azure ML 中生成专属的日志文件。 这些日志名为 `70_driver_logs`。 对于分布式训练，日志名称带有 `_rank` 后缀，以方便区分日志。 若要查找导致 Horovod 关闭的确切错误，请浏览所有日志文件，并查看 driver_log 文件末尾的 `Traceback`。 其中的某个文件会指出实际的根本性异常。 

## <a name="export-to-onnx"></a>导出到 ONNX

若要使用 [ONNX 运行时](concept-onnx.md)优化推理，请将训练后的 PyTorch 模型转换为 ONNX 格式。 推理或模型评分是将部署的模型用于预测（通常针对生产数据）的阶段。 有关示例，请参阅此[教程](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb)。

## <a name="next-steps"></a>后续步骤

在本文中，你使用 Azure 机器学习中的 PyTorch 训练并注册了一个深度学习神经网络。 若要了解如何部署模型，请继续参阅模型部署一文。

* [部署模型的方式和位置](how-to-deploy-and-where.md)
* [在训练期间跟踪运行指标](how-to-track-experiments.md)
* [优化超参数](how-to-tune-hyperparameters.md)
* [部署定型的模型](how-to-deploy-and-where.md)
* [Azure 中分布式深度学习训练的参考体系结构](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/training-deep-learning)

