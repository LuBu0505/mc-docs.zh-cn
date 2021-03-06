---
title: 教程 - 如何将 Azure Key Vault 与 .NET Azure Web 应用配合使用 | Azure
description: 本教程介绍如何将 ASP.NET Core 应用程序配置为从 Key Vault 读取机密。
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
origin.date: 05/06/2020
ms.date: 06/02/2020
ms.author: v-tawe
ms.openlocfilehash: 08c33078f8de5270e38fd141d3f10eb68288d2e7
ms.sourcegitcommit: 87b6bb293f39c5cfc2db6f38547220a13816d78f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96430980"
---
# <a name="tutorial-use-a-managed-identity-to-connect-key-vault-to-an-azure-web-app-with-net"></a>教程：使用托管标识将 Key Vault 连接到 .NET Azure Web 应用

虽然 Azure Key Vault 提供安全存储凭据及其他机密的方式，但代码需要对 Key Vault 进行身份验证才能检索这些凭据和机密。 [Azure 资源的托管标识概述](../../active-directory/managed-identities-azure-resources/overview.md)可帮助你解决此问题，其中介绍了如何在 Azure AD 中为 Azure 服务提供自动托管的标识。 此标识可用于通过支持 Azure AD 身份验证的任何服务（包括 Key Vault）的身份验证，这样就无需在代码中插入任何凭据了。

本教程使用托管标识将 Azure Web 应用向 Azure Key Vault 进行身份验证。 尽管这些步骤使用[适用于 .NET 的 Azure Key Vault v4 客户端库](https://docs.microsoft.com/dotnet/api/overview/azure/key-vault)和 [Azure CLI](/cli/get-started-with-azure-cli)，但在使用你选择的开发语言、Azure PowerShell 和/或 Azure 门户时，相同的基本原则也适用。

## <a name="prerequisites"></a>先决条件

若要完成本快速入门教程，需先执行以下操作：

* Azure 订阅 - [试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
* [.NET Core 3.1 SDK 或更高版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。
* [Azure CLI](/cli/install-azure-cli) 或 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)

## <a name="create-a-resource-group"></a>创建资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。 使用 [az group create](/cli/group#az-group-create) 命令创建一个资源组，用于存放密钥保管库和 Web 应用：

```azurecli
az group create --name "myResourceGroup" -l "ChinaEast"
```

## <a name="set-up-your-key-vault"></a>设置密钥保管库

现在，你将创建一个密钥保管库，并在其中放置机密，以便在本教程的后面部分使用。

若要创建密钥保管库，请使用 [az keyvault create](/cli/keyvault#az-keyvault-create) 命令：

> [!Important]
> 每个密钥保管库必须具有唯一的名称。 将以下示例中的 <your-unique-keyvault-name> 替换为你的密钥保管库名称。

```azurecli
az keyvault create --name "<your-keyvault-name>" -g "myResourceGroup"
```

记下返回的 `vaultUri`（其格式将为“https://<your-keyvault-name>.vault.azure.cn/”）。 它将在[更新代码](#update-the-code)步骤中使用。

现在可以使用 [az keyvault secret set](/cli/keyvault/secret#az-keyvault-secret-set) 命令将机密放入密钥保管库中。 将机密名称设置为“MySecret”，将值设置为 "Success!"。

```azurecli
az keyvault secret set --vault-name "<your-keyvault-name>" --name "MySecret" --value "Success!"
```

## <a name="create-a-net-web-app"></a>创建 .NET Web 应用

### <a name="create-a-local-app"></a>创建本地应用

在计算机的终端窗口中，创建一个名为 `akvwebapp` 的目录，并将当前目录切换到该目录。

```bash
mkdir akvwebapp
cd akvwebapp
```

现在，使用 [dotnet new web](https://docs.microsoft.com/dotnet/core/tools/dotnet-new) 命令创建新的 .NET Core 应用程序：

```bash
dotnet new web
```

在本地运行应用程序，以便你能了解将它部署到 Azure 时它的外观应该是什么样的。 

```bash
dotnet run
```

打开 Web 浏览器并导航到 `http://localhost:5000` 处的应用。

页面中会显示该示例应用发出的 Hello World 消息。

### <a name="initialize-the-git-repository"></a>初始化 Git 存储库

在终端窗口中，按 **Ctrl+C** 退出 Web 服务器。  为 .NET Core 项目初始化 Git 存储库。

```bash
git init
git add .
git commit -m "first commit"
```

### <a name="configure-a-deployment-user"></a>配置部署用户

可以使用“deployment user”将 FTP 和本地 Git 部署到 Azure Web 应用。 配置部署用户之后，可对所有 Azure 部署使用此用户。 帐户级部署用户名和密码不同于 Azure 订阅凭据。 

若要配置部署用户，请运行 [az webapp deployment user set](/cli/webapp/deployment/user#az-webapp-deployment-user-set) 命令。 选择符合以下准则的用户名和密码： 

- 用户名在 Azure 中必须唯一，并且为了本地Git推送，不能包含“@”符号。 
- 密码必须至少为 8 个字符，且具有字母、数字和符号这三种元素中的两种。 

```azurecli
az webapp deployment user set --user-name "<username>" --password "<password>"
```

JSON 输出会将该密码显示为 `null`。 如果收到 `'Conflict'. Details: 409` 错误，请更改用户名。 如果收到 `'Bad Request'. Details: 400` 错误，请使用更强的密码。 

请记录你要用于部署 Web 应用的用户名和密码。

### <a name="create-an-app-service-plan"></a>创建应用服务计划

使用 Azure CLI [az appservice plan create](/cli/appservice/plan) 命令创建应用服务计划。 以下示例在“免费”定价层中创建名为 `myAppServicePlan` 的应用服务计划：

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

创建应用服务计划后，Azure CLI 将显示类似于以下示例的信息：

<pre>
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "China East 2",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "China East 2",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
</pre>


### <a name="create-a-remote-web-app"></a>创建远程 Web 应用

在 `myAppServicePlan` 应用服务计划中创建 Azure Web 应用。 

> [!Important]
> 与密钥保管库类似，Azure Web 应用必须具有独一无二的名称。 在以下示例中，请将 \<your-webapp-name\> 替换为 Web 应用的名称。


```azurecli
az webapp create --resource-group "myResourceGroup" --plan "myAppServicePlan" --name "<your-webapp-name>" --deployment-local-git
```

创建 Web 应用后，Azure CLI 会显示类似于以下示例的输出：

<pre>
Local git is configured with url of 'https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.chinacloudsites.cn/&lt;ayour-webapp-name&gt;.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;your-webapp-name&gt;.chinacloudsites.cn",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.chinacloudsites.cn/&lt;your-webapp-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>


Git 远程的 URL 将显示在 `deploymentLocalGitUrl` 属性中，其格式为 `https://<username>@<your-webapp-name>.scm.chinacloudsites.cn/<your-webapp-name>.git`。 保存此 URL，因为稍后会需要它。

浏览到新建的应用。 将 &lt;your-webapp-name> 替换为你的应用名称。

```bash
https://<your-webapp-name>.chinacloudsites.cn
```

你将看到新创建的 Azure Web 应用的默认网页。

### <a name="deploy-your-local-app"></a>部署本地应用

回到本地终端窗口，将 Azure 远程库添加到本地 Git 存储库，将 \<deploymentLocalGitUrl-from-create-step> 替换为在[创建远程 Web 应用](#create-a-remote-web-app)步骤中保存的 Git 远程库的 URL。

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

使用以下命令推送到 Azure 远程库以部署应用。 当 Git 凭据管理器提示你输入凭据时，请使用你在[配置部署用户](#configure-a-deployment-user)步骤中创建的凭据。

```bash
git push azure master
```

此命令可能需要花费几分钟时间运行。 运行时，该命令会显示类似于以下示例的信息：
<pre>
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 285 bytes | 95.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'd6b54472f7'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote:
remote: Oryx Version      : 0.2.20200114.13, Commit: 204922f30f8e8d41f5241b8c218425ef89106d1d, ReleaseTagName: 20200114.13
remote: Build Operation ID: |imoMY2y77/s=.40ca2a87_
remote: Repository Commit : d6b54472f7e8e9fd885ffafaa64522e74cf370e1
.
.
.
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;your-webapp-name&gt;.scm.chinacloudsites.cn/newui/jsonviewer?view_url=/api/deployments/d6b54472f7e8e9fd885ffafaa64522e74cf370e1/log'
To https://&lt;your-webapp-name&gt;.scm.chinacloudsites.cn:443/&lt;your-webapp-name&gt;.git
   d87e6ca..d6b5447  master -> master
</pre>

使用 Web 浏览器浏览到（或刷新）已部署的应用程序。

```bash
http://<your-webapp-name>.chinacloudsites.cn
```

此时会显示“Hello World!” 消息，你以前在访问 `http://localhost:5000` 时看到过该消息。

## <a name="create-and-assign-a-managed-identity"></a>创建并分配托管标识

在 Azure CLI 中，若要为此应用程序创建标识，请运行 [az webapp-identity assign](https://docs.microsoft.com/cli/azure/webapp/identity#az-webapp-identity-assign) 命令：

```azurecli
az webapp identity assign --name "<your-webapp-name>" --resource-group "myResourceGroup"
```

该操作将返回此 JSON 代码片段：

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

若要为 Web 应用授予在密钥保管库上执行 **get** 和 **list** 的权限，请将 principalID 传递到 Azure CLI [az keyvault set-policy](/cli/keyvault#az-keyvault-set-policy) 命令：

```azurecli
az keyvault set-policy --name "<your-keyvault-name>" --object-id "<principalId>" --secret-permissions get list
```


## <a name="modify-the-app-to-access-your-key-vault"></a>修改应用以访问密钥保管库

### <a name="install-the-packages"></a>安装包

在终端窗口中，安装适用于 .NET 的 Azure Key Vault 客户端库包：

```console
dotnet add package Azure.Identity
dotnet add package Azure.Security.KeyVault.Secrets
```

### <a name="update-the-code"></a>更新代码

在 akvwebapp 项目中找到 Startup.cs 文件并将其打开。 

在该文件头部添加以下两行：

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
```

将以下行添加到 `app.UseEndpoints` 调用之前，更新 URI 以反映密钥保管库的 `vaultUri`。 下面的代码将 [DefaultAzureCredential()](https://docs.microsoft.com/dotnet/api/azure.identity.defaultazurecredential) 用于向密钥保管库进行身份验证，该类使用来自应用程序托管标识的令牌进行身份验证。 它还在密钥保管库受到限制的情况下将指数退避用于重试。

```csharp
SecretClientOptions options = new SecretClientOptions()
    {
        Retry =
        {
            Delay= TimeSpan.FromSeconds(2),
            MaxDelay = TimeSpan.FromSeconds(16),
            MaxRetries = 5,
            Mode = RetryMode.Exponential
         }
    };
var client = new SecretClient(new Uri("https://<your-unique-key-vault-name>.vault.azure.cn/"), new DefaultAzureCredential(),options);

KeyVaultSecret secret = client.GetSecret("mySecret");

string secretValue = secret.Value;
```

将 `await context.Response.WriteAsync("Hello World!");` 行更新为：

```csharp
await context.Response.WriteAsync(secretValue);
```

请确保在继续下一步之前保存更改。

### <a name="redeploy-your-web-app"></a>重新部署 Web 应用

更新代码后，可以使用以下 Git 命令将其重新部署到 Azure：

```bash
git add .
git commit -m "Updated web app to access my key vault"
git push azure master
```

## <a name="visit-your-completed-web-app"></a>访问已完成的 Web 应用

```bash
http://<your-webapp-name>.chinacloudsites.cn
```

在以前显示 **Hello World** 的地方，现在应显示机密值：**成功！**

## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure 资源的托管标识](../../active-directory/managed-identities-azure-resources/overview.md)
- 详细了解[应用服务的托管标识](../../app-service/overview-managed-identity.md?tabs=dotnet)
- 请参阅[适用于 .NET 的 Azure Key Vault 客户端库 API 参考](https://docs.microsoft.com/dotnet/api/overview/azure/key-vault)
- 请参阅[适用于 .NET 的 Azure Key Vault 客户端库源代码](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault)
- 请参阅[适用于 .NET 的 v4 Azure Key Vault 客户端库 NuGet 包](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/)


