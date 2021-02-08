---
title: 将 Azure 应用程序配置与 .NET Core 结合使用的快速入门 | Azure Docs
description: 在本快速入门中，使用 Azure 应用程序配置创建 .NET Core 应用，集中存储和管理与代码分离的应用程序设置。
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.custom: devx-track-csharp
ms.date: 02/01/2021
ms.author: lcozzens
ms.openlocfilehash: 1eb21b0d07d09abdc27a14af57e8a0f04a51888e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059057"
---
# <a name="quickstart-create-a-net-core-app-with-app-configuration"></a>快速入门：使用应用程序配置创建 .NET Core 应用

在本快速入门中，会将 Azure 应用程序配置合并到 .NET Core 控制台应用中，以集中存储和管理与代码分离的应用程序设置。

## <a name="prerequisites"></a>先决条件

- Azure 订阅 - [创建试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)
- [.NET Core SDK](https://dotnet.microsoft.com/download)。

<!--Cloud Shell-->

## <a name="create-an-app-configuration-store"></a>创建应用配置存储区

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

7. 选择“配置资源管理器” > “创建” > “键-值”来添加以下键值对    ：

    | 密钥 | 值 |
    |---|---|
    | TestApp:Settings:Message | Azure 应用配置的数据 |

    暂时将“标签”和“内容类型”保留为空   。

8. 选择“应用”。 

## <a name="create-a-net-core-console-app"></a>创建 .NET Core 控制台应用

你使用 [.NET Core 命令行接口 (CLI)](https://docs.microsoft.com/dotnet/core/tools/) 创建新的 .NET Core 控制台应用项目。 通过 Visual Studio 使用 .NET Core CLI 的优点是，它可用于 Windows、macOS 和 Linux 平台。  

<!--Cloud Shell-->

1. 为项目新建一个文件夹。

2. 在新文件夹中，运行以下命令以创建新的 .NET Core 控制台应用项目：

    ```dotnetcli
    dotnet new console
    ```

## <a name="connect-to-an-app-configuration-store"></a>连接到应用程序配置存储区

1. 通过运行以下命令，添加对 `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet 包的引用：

    ```dotnetcli
    dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration
    ```

2. 运行以下命令，还原项目包：

    ```dotnetcli
    dotnet restore
    ```

3. 打开 *Program.cs*，并添加对 .NET Core 应用程序配置提供程序的引用。

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

4. 通过调用 `builder.AddAzureAppConfiguration()` 方法，更新 `Main` 方法以使用应用配置。

    ```csharp
    static void Main(string[] args)
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));

        var config = builder.Build();
        Console.WriteLine(config["TestApp:Settings:Message"] ?? "Hello world!");
    }
    ```

## <a name="build-and-run-the-app-locally"></a>在本地生成并运行应用

1. 设置名为“ConnectionString”的环境变量，并将其设置为应用程序配置存储区的访问密钥  。 在命令行中运行以下命令：

    ```cmd
    setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    如果使用 Windows PowerShell，请运行以下命令：

    ```azurepowershell
    $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```

    如果使用 macOS 或 Linux，则请运行以下命令：

    ```console
    export ConnectionString='connection-string-of-your-app-configuration-store'
    ```

    重启命令提示符以使更改生效。 输出环境变量的值以验证其设置是否正确。

2. 运行以下命令以生成控制台应用：

    ```dotnetcli
    dotnet build
    ```

3. 生成成功完成后，请运行以下命令以在本地运行应用：

    ```dotnetcli
    dotnet run
    ```

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何创建新的应用程序配置存储区，以及如何通过[应用程序配置提供程序](https://docs.microsoft.com/dotnet/api/Microsoft.Extensions.Configuration.AzureAppConfiguration)将其与 .NET Core 控制台应用配合使用。 若要了解如何配置 .NET Core 应用来动态刷新配置设置，请继续学习下一个教程。

> [!div class="nextstepaction"]
> [启用动态配置](./enable-dynamic-configuration-dotnet-core.md)
