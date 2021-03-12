---
title: 使用 .NET 创建或删除 Blob 容器 - Azure 存储
description: 了解如何使用 .NET 客户端库在 Azure 存储帐户中创建或删除 Blob 容器。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 02/04/2020
ms.date: 03/08/2021
ms.author: v-jay
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: 1c738adbbd107f77438a7434b73051a86a6d0286
ms.sourcegitcommit: 0b49bd1b3b05955371d1154552f4730182c7f0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196264"
---
# <a name="create-or-delete-a-container-in-azure-storage-with-net"></a>使用 .NET 在 Azure 存储中创建或删除容器

Azure 存储中的 Blob 已组织成容器。 必须先创建容器，才能上传 Blob。 本文介绍如何使用[适用于 .NET 的 Azure 存储客户端库](https://docs.azure.cn/dotnet/api/overview/storage/client?view=azure-dotnet)创建和删除容器。

## <a name="name-a-container"></a>为容器命名

容器名称必须是有效的 DNS 名称，因为它是用于对容器或其 Blob 寻址的唯一 URI 的一部分。 为容器命名时，请遵循以下规则：

- 容器名称的长度可以是 3 到 63 个字符。
- 容器名称必须以字母或数字开头，并且只能包含小写字母、数字和短划线 (-) 字符。
- 容器名称中不允许出现两个或更多个连续的短划线字符。

容器的 URI 采用以下格式：

`https://myaccount.blob.core.chinacloudapi.cn/mycontainer`

## <a name="create-a-container"></a>创建容器

若要创建容器，请调用以下方法之一：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

- [CreateBlobContainer](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobserviceclient.createblobcontainer)
- [CreateBlobContainerAsync](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobserviceclient.createblobcontainerasync)

如果已存在同名的容器，这些方法将引发异常。

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

- [创建](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.create)
- [CreateAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createasync)
- [CreateIfNotExists](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexists)
- [CreateIfNotExistsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexistsasync)

如果已存在同名的容器，**Create** 和 **CreateAsync** 方法将引发异常。

**CreateIfNotExists** 和 **CreateIfNotExistsAsync** 方法返回一个指示是否已创建容器的布尔值。 如果已存在同名的容器，这些方法将返回 False，指示未创建新容器。

---

将立即在存储帐户下创建容器。 无法将一个容器嵌套在另一个容器下。

以下示例以异步方式创建一个容器：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

```csharp
//-------------------------------------------------
// Create a container
//-------------------------------------------------
private static async Task<BlobContainerClient> CreateSampleContainerAsync(BlobServiceClient blobServiceClient)
{
    // Name the sample container based on new GUID to ensure uniqueness.
    // The container name must be lowercase.
    string containerName = "container-" + Guid.NewGuid();

    try
    {
        // Create the container
        BlobContainerClient container = await blobServiceClient.CreateBlobContainerAsync(containerName);

        if (await container.ExistsAsync())
        {
            Console.WriteLine("Created container {0}", container.Name);
            return container;
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.Status, e.ErrorCode);
        Console.WriteLine(e.Message);
    }

    return null;
}
```

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static async Task<CloudBlobContainer> CreateSampleContainerAsync(CloudBlobClient blobClient)
{
    // Name the sample container based on new GUID, to ensure uniqueness.
    // The container name must be lowercase.
    string containerName = "container-" + Guid.NewGuid();

    // Get a reference to a sample container.
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    try
    {
        // Create the container if it does not already exist.
        bool result = await container.CreateIfNotExistsAsync();
        if (result == true)
        {
            Console.WriteLine("Created container {0}", container.Name);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }

    return container;
}
```
---

## <a name="create-the-root-container"></a>创建根容器

根容器充当存储帐户的默认容器。 每个存储帐户只能包含一个根容器，该容器必须命名为 $root。 必须显式创建或删除根容器。

可以引用存储在根容器中的 Blob，而无需包含根容器名称。 根容器允许引用位于存储帐户层次结构顶层的 Blob。 例如，可通过以下方式引用根容器中的 blob：

`https://myaccount.blob.core.chinacloudapi.cn/default.html`

以下示例以同步方式创建根容器：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

```csharp
//-------------------------------------------------
// Create root container
//-------------------------------------------------
private static void CreateRootContainer(BlobServiceClient blobServiceClient)
{
    try
    {
        // Create the root container or handle the exception if it already exists
        BlobContainerClient container =  blobServiceClient.CreateBlobContainer("$root");

        if (container.Exists())
        {
            Console.WriteLine("Created root container.");
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.Status, e.ErrorCode);
        Console.WriteLine(e.Message);
    }
}
```

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static void CreateRootContainer(CloudBlobClient blobClient)
{
    try
    {
        // Create the root container if it does not already exist.
        CloudBlobContainer container = blobClient.GetContainerReference("$root");
        if (container.CreateIfNotExists())
        {
            Console.WriteLine("Created root container.");
        }
        else
        {
            Console.WriteLine("Root container already exists.");
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }
}
```
---

## <a name="delete-a-container"></a>删除容器

若要在 .NET 中删除容器，请使用以下方法之一：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

- [删除](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.delete)
- [DeleteAsync](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.deleteasync)
- [DeleteIfExists](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.deleteifexists)
- [DeleteIfExistsAsync](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.deleteifexistsasync)

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

- [删除](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.delete)
- [DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync)
---

如果该容器不存在，Delete 和 DeleteAsync 方法将引发异常 。

**DeleteIfExists** 和 **DeleteIfExistsAsync** 方法返回一个指示是否已删除容器的布尔值。 如果指定的容器不存在，则这些方法将返回 False，指示未删除该容器。

删除容器后，至少在 30 秒内无法使用相同的名称创建容器。 尝试使用相同的名称创建容器将会失败，并出现 HTTP 错误代码 409（冲突）。 针对容器或其包含的 Blob 执行任何其他操作将会失败，并出现 HTTP 错误代码 404（未找到）。

以下示例删除指定的容器，并在该容器不存在时处理异常：

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

```csharp
//-------------------------------------------------
// Delete a container
//-------------------------------------------------
private static async Task DeleteSampleContainerAsync(BlobServiceClient blobServiceClient, string containerName)
{
    BlobContainerClient container = blobServiceClient.GetBlobContainerClient(containerName);

    try
    {
        // Delete the specified container and handle the exception.
        await container.DeleteAsync();
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.Status, e.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static async Task DeleteSampleContainerAsync(CloudBlobClient blobClient, string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    try
    {
        // Delete the specified container and handle the exception.
        await container.DeleteAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```
---

以下示例演示如何删除以指定的前缀开头的所有容器。

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

```csharp
//-------------------------------------------------
// Delete all containers with the specified prefix
//-------------------------------------------------
private static async Task DeleteContainersWithPrefixAsync(BlobServiceClient blobServiceClient, string prefix)
{
    Console.WriteLine("Delete all containers beginning with the specified prefix");

    try
    {
        foreach (BlobContainerItem container in blobServiceClient.GetBlobContainers())
        {
            if (container.Name.StartsWith(prefix))
            { 
                Console.WriteLine("\tContainer:" + container.Name);
                BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(container.Name);
                await containerClient.DeleteAsync();
            }
        }

        Console.WriteLine();
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
private static async Task DeleteContainersWithPrefixAsync(CloudBlobClient blobClient, string prefix)
{
    Console.WriteLine("Delete all containers beginning with the specified prefix");
    try
    {
        foreach (var container in blobClient.ListContainers(prefix))
        {
            Console.WriteLine("\tContainer:" + container.Name);
            await container.DeleteAsync();
        }

        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```
---

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>请参阅

- [Create Container 操作](https://docs.microsoft.com/rest/api/storageservices/create-container)
- [Delete Container 操作](https://docs.microsoft.com/rest/api/storageservices/delete-container)
