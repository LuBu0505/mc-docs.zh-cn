---
title: 以编程方式创建 Azure 服务总线实体 | Azure Docs
description: 本文介绍如何以动态或编程方式预配服务总线命名空间和实体。
ms.devlang: dotnet
ms.service: service-bus-messaging
ms.topic: article
origin.date: 01/13/2021
ms.date: 02/01/2021
ms.testscope: yes
ms.testdate: 07/20/2020
ms.author: v-yeche
author: rockboyfor
ms.openlocfilehash: eee85acd383d165ef8e5945b749564c495c5fcf4
ms.sourcegitcommit: 136164cd330eb9323fe21fd1856d5671b2f001de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196466"
---
# <a name="dynamically-provision-service-bus-namespaces-and-entities"></a>动态预配服务总线命名空间和实体 
Azure 服务总线管理库可以动态预配服务总线命名空间和实体。 这样可以实现复杂的部署和消息方案，并能以编程方式确定要预配的实体。 这些库目前可用于 .NET。

## <a name="overview"></a>概述
目前有 3 个管理库可用于创建和管理服务总线实体。 它们是：

- [Azure.Messaging.ServiceBus.Administration](#azuremessagingservicebusadministration)
- [Microsoft.Azure.ServiceBus.Management](#microsoftazureservicebusmanagement)
- [Microsoft.Azure.Management.ServiceBus](#microsoftazuremanagementservicebus)

这些包都支持对队列、主题和订阅执行创建、获取、列出、删除和更新操作。 但只有 [Microsoft.Azure.Management.ServiceBus](#microsoftazuremanagementservicebus) 支持对命名空间执行创建、更新、列出、获取和删除操作，这会列出和重新生成 SAS 密钥等。 

Microsoft.Azure.Management.ServiceBus 库仅支持 Azure Active Directory (Azure AD) 身份验证，不支持使用连接字符串。 其他两个库（Azure.Messaging.ServiceBus 和 Microsoft.Azure.ServiceBus）支持使用连接字符串对服务进行身份验证，而且更易于使用。 在这些库中，Azure.Messaging.ServiceBus 是最新的，建议使用该库。

以下各部分更详细地介绍了这些库。 

## <a name="azuremessagingservicebusadministration"></a>Azure.Messaging.ServiceBus.Administration
可使用 [Azure.Messaging.ServiceBus.Administration](https://docs.microsoft.com/dotnet/api/azure.messaging.servicebus.administration) 命名空间中的 [ServiceBusAdministrationClient](https://docs.microsoft.com/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient) 类来管理命名空间、队列、主题和订阅。 下面是示例代码。 如需查看完整示例，请参阅 [CRUD 示例](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/servicebus/Azure.Messaging.ServiceBus/tests/Samples/Sample07_CrudOperations.cs)。

<!--CORRECT ON https://docs.microsoft.com/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient-->
<!--CORRECT ON https://docs.microsoft.com/dotnet/api/azure.messaging.servicebus.administration-->

```csharp
using System;
using System.Threading.Tasks;

using Azure.Messaging.ServiceBus.Administration;

namespace adminClientTrack2
{
    class Program
    {
        public static void Main()
        {
            MainAsync().GetAwaiter().GetResult();
        }

        private static async Task MainAsync()
        {
            string connectionString = "SERVICE BUS NAMESPACE CONNECTION STRING";
            string QueueName = "QUEUE NAME";
            string TopicName = "TOPIC NAME";
            string SubscriptionName = "SUBSCRIPTION NAME";

            var adminClient = new ServiceBusAdministrationClient(connectionString);
            bool queueExists = await adminClient.QueueExistsAsync(QueueName);
            if (!queueExists)
            {
                var options = new CreateQueueOptions(QueueName)
                {
                    MaxDeliveryCount = 3                    
                };
                await adminClient.CreateQueueAsync(options);
            }


            bool topicExists = await adminClient.TopicExistsAsync(TopicName);
            if (!topicExists)
            {
                var options = new CreateTopicOptions(TopicName)
                {
                    MaxSizeInMegabytes = 1024
                };
                await adminClient.CreateTopicAsync(options);
            }

            bool subscriptionExists = await adminClient.SubscriptionExistsAsync(TopicName, SubscriptionName);
            if (!subscriptionExists)
            {
                var options = new CreateSubscriptionOptions(TopicName, SubscriptionName)
                {
                    DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0)
                };
                await adminClient.CreateSubscriptionAsync(options);
            }
        }
    }
}

```


## <a name="microsoftazureservicebusmanagement"></a>Microsoft.Azure.ServiceBus.Management 
可使用 [Microsoft.Azure.ServiceBus.Management](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management) 命名空间中的 [ManagementClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management.managementclient) 类来管理命名空间、队列、主题和订阅。 下面是示例代码： 

> [!NOTE]
> 建议使用 `Azure.Messaging.ServiceBus.Administration` 库中的 `ServiceBusAdministrationClient` 类，这是最新的 SDK。 有关详细信息，请参阅[第一部分](#azuremessagingservicebusadministration)。 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.ServiceBus.Management;

namespace SBusManagementClient
{
    class Program
    {
        public static void Main()
        {
            MainAsync().GetAwaiter().GetResult();
        }

        private static async Task MainAsync()
        {
            string connectionString = "SERVICE BUS NAMESPACE CONNECTION STRING";
            string QueueName = "QUEUE NAME";
            string TopicName = "TOPIC NAME";
            string SubscriptionName = "SUBSCRIPTION NAME";

            var managementClient = new ManagementClient(connectionString);
            bool queueExists = await managementClient.QueueExistsAsync(QueueName);
            if (!queueExists)
            {
                QueueDescription qd = new QueueDescription(QueueName);
                qd.MaxSizeInMB = 1024;
                qd.MaxDeliveryCount = 3;
                await managementClient.CreateQueueAsync(qd);
            }


            bool topicExists = await managementClient.TopicExistsAsync(TopicName);
            if (!topicExists)
            {
                TopicDescription td = new TopicDescription(TopicName);
                td.MaxSizeInMB = 1024;
                td.DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0);
                await managementClient.CreateTopicAsync(td);
            }

            bool subscriptionExists = await managementClient.SubscriptionExistsAsync(TopicName, SubscriptionName);
            if (!subscriptionExists)
            {
                SubscriptionDescription sd = new SubscriptionDescription(TopicName, SubscriptionName);
                sd.DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0);
                sd.MaxDeliveryCount = 3;
                await managementClient.CreateSubscriptionAsync(sd);
            }
        }
    }
}
```


## <a name="microsoftazuremanagementservicebus"></a>Microsoft.Azure.Management.ServiceBus 
此库是基于 Azure 资源管理器的控制平面 SDK 的一部分。 

### <a name="prerequisites"></a>先决条件

若要开始使用服务总线管理库，必须使用 Azure Active Directory (Azure AD) 服务进行身份验证。 Azure AD 要求身份验证为服务主体，并且该主体提供对 Azure 资源的访问权限。 有关创建服务主体的信息，请参阅以下文章之一：  

* [使用 Azure 门户创建可访问资源的 Active Directory 应用程序和服务主体](../active-directory/develop/howto-create-service-principal-portal.md)
* [使用 Azure PowerShell 创建服务主体来访问资源](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [使用 Azure CLI 创建服务主体来访问资源](https://docs.azure.cn/cli/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)

这些教程提供 `AppId`（客户端 ID）、`TenantId` 和 `ClientSecret`（身份验证密钥），这些都将用于管理库进行的身份验证。 对于用于运行的资源组，需要至少具有 [Azure 服务总线数据所有者](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner)或[参与者](../role-based-access-control/built-in-roles.md#contributor)权限 。

## <a name="programming-pattern"></a>编程模式

所有服务总线资源的操纵模式都遵循常用协议：

1. 使用 **Microsoft.IdentityModel.Clients.ActiveDirectory** 库从 Azure AD 获取令牌：
    ```csharp
    var context = new AuthenticationContext($"https://login.chinacloudapi.cn/{tenantId}");

    var result = await context.AcquireTokenAsync("https://management.chinacloudapi.cn/", new ClientCredential(clientId, clientSecret));
    ```
2. 创建 `ServiceBusManagementClient` 对象：

    ```csharp
    var creds = new TokenCredentials(token);
    var sbClient = new ServiceBusManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```
3. 将 `CreateOrUpdate` 参数设置为指定值：

    ```csharp
    var queueParams = new QueueCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"],
        EnablePartitioning = true
    };
    ```
4. 执行调用：

    ```csharp
    await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
    ```

## <a name="complete-code-to-create-a-queue"></a>完成创建队列的代码
下面是创建服务总线队列的完整代码： 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.Management.ServiceBus;
using Microsoft.Azure.Management.ServiceBus.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;

namespace SBusADApp
{
    class Program
    {
        static void Main(string[] args)
        {
            CreateQueue().GetAwaiter().GetResult();
        }

        private static async Task CreateQueue()
        {
            try
            {
                var subscriptionID = "<SUBSCRIPTION ID>";
                var resourceGroupName = "<RESOURCE GROUP NAME>";
                var namespaceName = "<SERVICE BUS NAMESPACE NAME>";
                var queueName = "<NAME OF QUEUE YOU WANT TO CREATE>";

                var token = await GetToken();

                var creds = new TokenCredentials(token);
                var sbClient = new ServiceBusManagementClient(creds)
                {
                    SubscriptionId = subscriptionID,
                };

                var queueParams = new SBQueue();

                Console.WriteLine("Creating queue...");
                await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, queueName, queueParams);
                Console.WriteLine("Created queue successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not create a queue...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

        private static async Task<string> GetToken()
        {
            try
            {
                var tenantId = "<AZURE AD TENANT ID>";
                var clientId = "<APPLICATION/CLIENT ID>";
                var clientSecret = "<CLIENT SECRET>";

                var context = new AuthenticationContext($"https://login.chinacloudapi.cn/{tenantId}");

                var result = await context.AcquireTokenAsync(
                    "https://management.chinacloudapi.cn/",
                    new ClientCredential(clientId, clientSecret)
                );

                // If the token isn't a valid string, throw an error.
                if (string.IsNullOrEmpty(result.AccessToken))
                {
                    throw new Exception("Token result is empty!");
                }

                return result.AccessToken;
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not get a token...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

    }
}
```

> [!IMPORTANT]
> 有关完整示例，请参阅 [GitHub 上的 .NET 管理示例](https://github.com/Azure-Samples/service-bus-dotnet-management/)。 

## <a name="next-steps"></a>后续步骤
[Microsoft.Azure.Management.ServiceBus API 参考](https://docs.azure.cn/dotnet/api/Microsoft.Azure.Management.ServiceBus?view=azure-dotnet)

<!-- Update_Description: update meta properties, wording update, update link -->