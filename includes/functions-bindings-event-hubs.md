---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 11/30/2020
ms.author: v-junlch
ms.openlocfilehash: b0a43a92ad6dec6d833e8aa845f60865be78fca6
ms.sourcegitcommit: a1f565fd202c1b9fd8c74f814baa499bbb4ed4a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96507345"
---
## <a name="add-to-your-functions-app"></a>添加到 Functions 应用

### <a name="functions-2x-and-higher"></a>Functions 2.x 及更高版本

使用触发器和绑定需要引用相应的包。 NuGet 包用于 .NET 类库，而扩展捆绑包用于其他所有应用程序类型。

| 语言                                        | 添加方式...                                   | 备注 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | 安装 [NuGet 包]版本 3.x | |
| C# 脚本、Java、JavaScript、PowerShell | 注册[扩展捆绑包]          | 建议将 [Azure Tools 扩展]用于 Visual Studio Code。 |
| C# 脚本（Azure 门户中仅限联机）         | 添加绑定                            | 若要更新现有绑定扩展而不必重新发布函数应用，请参阅[更新扩展]。 |

[NuGet 包]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs
[core tools]: ../articles/azure-functions/functions-run-local.md
[扩展捆绑包]: ../articles/azure-functions/functions-bindings-register.md#extension-bundles
[更新扩展]: ../articles/azure-functions/functions-bindings-register.md
[Azure 工具扩展]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1.x 应用会自动引用 [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet 程序包（版本 2.x）。

