---
title: include 文件
description: include 文件
services: azure-resource-manager
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: include
origin.date: 07/07/2020
ms.date: 03/01/2021
ms.testscope: no
ms.testdate: 09/15/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: f66825ea5e5113a89dfcdb148c1285ddbfb7ba41
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102052676"
---
> [!NOTE]
> 当我们使用以 `https://raw.githubusercontent.com/` 开头的指定模板文件 URI 部署资源时，控制台有时会生成错误，如 `Unable to download deployment content`。
>
> 可以执行以下操作来解决相应问题。
> 1. 复制模板 URI，通过更改前缀、中缀和模板文件名来转换 URI。
>     例如，源 URI 是 `https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-autoscale/azuredeploy.json`
>
>     | 类别 | 原始值 | 转换后的值 |  操作  |
>     |----------|----------------|-----------------|----------|
>     | 前缀   | `https://raw.githubusercontent.com`  |  `https://github.com`  | 更新 |
>     | 中辍    |                | `blob`          |  在 `master` 或 `main` 之前添加分支名称 |
>     | 模板文件名  |azuredeploy.json | 你的下载模板文件名 | update |
>
>     修改后，转换后的 URI 看起来将类似于 `https://github.com/Azure/azure-quickstart-templates/blob/master/101-cosmosdb-sql-autoscale/azuredeploy.json`。
> 2. 复制转换后的 URI，并在 Internet 浏览器中手动下载特定的模板内容。
> 3. 修改从 GitHub 存储库下载或引用的模板，以适应 Azure 中国云环境。 
>    例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；必要时更改某些不受支持的位置、VM 映像、VM 大小、SKU 以及资源提供程序的 API 版本。
> 4. 将 `TemplateUri` 的参数替换为 `TemplateFile`，然后用下载的实际文件名更新指定的 URI，并再次运行该脚本。
> 
>    | 语言类别   | 参考链接                   | 操作   |
>    |---         | --------                         |----------|
>    | PowerShell | [`New-AzResourceGroupDeployment`](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzResourceGroupDeployment) | 将 `-TemplateUri` 替换为 `-TemplateFile` <br /> 如有必要，请按照前面的步骤下载 `--TemplateParameterUri` 内容并在 cmdlet 中替换为 `--TemplateParameterFile`。 |
>    | Azure CLI  | [`az deployment group create`](https://docs.azure.cn/zh-cn/cli/deployment/group#az_deployment_group_create) | 将 `--template-uri` 替换为 `--template-file ` |
>