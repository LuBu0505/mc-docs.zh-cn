---
title: 为容器或 blob 创建服务 SAS
titleSuffix: Azure Storage
description: 了解如何使用适用于 Blob 存储的 Azure 存储库为容器或 blob 创建服务共享访问签名 (SAS)。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 11/20/2020
ms.date: 01/18/2021
ms.author: v-jay
ms.reviewer: dineshm
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: 93870567e96522114471f69ba62712e3cf5989ea
ms.sourcegitcommit: f086abe8bd2770ed10a4842fa0c78b68dbcdf771
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98163082"
---
# <a name="create-a-service-sas-for-a-container-or-blob"></a>为容器或 blob 创建服务 SAS

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

本文介绍如何使用存储帐户密钥通过用于 Blob 存储的 Azure 存储客户端库为容器或 blob 创建服务 SAS。

## <a name="create-a-service-sas-for-a-blob-container"></a>为 blob 容器创建服务 SAS

下面的代码示例为容器创建 SAS。 如果提供现有存储访问策略的名称，则该策略与 SAS 关联。 如果未提供存储访问策略，则代码会在容器上创建一个临时 SAS。

### <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

服务 SAS 将使用帐户访问密钥进行签名。 使用 [StorageSharedKeyCredential](https://docs.microsoft.com/dotnet/api/azure.storage.storagesharedkeycredential) 类创建用于为 SAS 签名的凭据。 接下来，新建 [BlobSasBuilder](https://docs.microsoft.com/dotnet/api/azure.storage.sas.blobsasbuilder) 对象，并调用 [ToSasQueryParameters](https://docs.microsoft.com/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) 以获取 SAS 令牌字符串。

```csharp
private static Uri GetServiceSasUriForContainer(BlobContainerClient containerClient,
                                          string storedPolicyName = null)
{
    // Check whether this BlobContainerClient object has been authorized with Shared Key.
    if (containerClient.CanGenerateSasUri)
    {
        // Create a SAS token that's valid for one hour.
        BlobSasBuilder sasBuilder = new BlobSasBuilder()
        {
            BlobContainerName = containerClient.Name,
            Resource = "c"
        };

        if (storedPolicyName == null)
        {
            sasBuilder.ExpiresOn = DateTimeOffset.UtcNow.AddHours(1);
            sasBuilder.SetPermissions(BlobContainerSasPermissions.Read);
        }
        else
        {
            sasBuilder.Identifier = storedPolicyName;
        }

        Uri sasUri = containerClient.GenerateSasUri(sasBuilder);
        Console.WriteLine("SAS URI for blob container is: {0}", sasUri);
        Console.WriteLine();

        return sasUri;
    }
    else
    {
        Console.WriteLine(@"BlobContainerClient must be authorized with Shared Key 
                          credentials to create a service SAS.");
        return null;
    }
}
```

### <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

若要为容器创建服务 SAS，请调用 [CloudBlobContainer.GetSharedAccessSignature](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getsharedaccesssignature) 方法。

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, 
                                         string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define
        // the parameters of an ad hoc SAS, and to construct a shared access policy
        // that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed
            // to be the time when the storage service receives the request. Omitting
            // the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate the shared access signature on the container,
        // setting the constraints directly on the signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the container. In this case,
        // all of the constraints for the shared access signature are specified
        // on the stored access policy, which is provided by name. It is also possible
        // to specify some constraints on an ad hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

服务 SAS 将使用帐户访问密钥进行签名。 使用 [StorageSharedKeyCredential](https://docs.microsoft.com/javascript/api/@azure/storage-blob/storagesharedkeycredential) 类创建用于为 SAS 签名的凭据。 接下来，调用 [generateBlobSASQueryParameters](https://docs.microsoft.com/javascript/api/@azure/storage-blob/#generateBlobSASQueryParameters_BlobSASSignatureValues__StorageSharedKeyCredential_) 函数并提供所需选项，以获取 SAS 令牌字符串。

```javascript
function getContainerSasUri(containerClient, sharedKeyCredential, storedPolicyName) {
    const sasOptions = {
        containerName: containerClient.containerName,
        permissions: ContainerSASPermissions.parse("c")
    };

    if (storedPolicyName == null) {
        sasOptions.startsOn = new Date();
        sasOptions.expiresOn = new Date(new Date().valueOf() + 3600 * 1000);
    } else {
        sasOptions.identifier = storedPolicyName;
    }

    const sasToken = generateBlobSASQueryParameters(sasOptions, sharedKeyCredential).toString();
    console.log(`SAS token for blob container is: ${sasToken}`);

    return `${containerClient.url}?${sasToken}`;
}
```

---

## <a name="create-a-service-sas-for-a-blob"></a>为 blob 创建服务 SAS

下面的代码示例在 blob 上创建 SAS。 如果提供现有存储访问策略的名称，则该策略与 SAS 关联。 如果未提供存储访问策略，则代码会在 Blob 上创建一个临时 SAS。

# <a name="net-v12"></a>[\.NET v12](#tab/dotnet)

服务 SAS 将使用帐户访问密钥进行签名。 使用 [StorageSharedKeyCredential](https://docs.microsoft.com/dotnet/api/azure.storage.storagesharedkeycredential) 类创建用于为 SAS 签名的凭据。 接下来，新建 [BlobSasBuilder](https://docs.microsoft.com/dotnet/api/azure.storage.sas.blobsasbuilder) 对象，并调用 [ToSasQueryParameters](https://docs.microsoft.com/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) 以获取 SAS 令牌字符串。

```csharp
private static Uri GetServiceSasUriForBlob(BlobClient blobClient,
    string storedPolicyName = null)
{
    // Check whether this BlobClient object has been authorized with Shared Key.
    if (blobClient.CanGenerateSasUri)
    {
        // Create a SAS token that's valid for one hour.
        BlobSasBuilder sasBuilder = new BlobSasBuilder()
        {
            BlobContainerName = blobClient.GetParentBlobContainerClient().Name,
            BlobName = blobClient.Name,
            Resource = "b"
        };

        if (storedPolicyName == null)
        {
            sasBuilder.ExpiresOn = DateTimeOffset.UtcNow.AddHours(1);
            sasBuilder.SetPermissions(BlobSasPermissions.Read |
                BlobSasPermissions.Write);
        }
        else
        {
            sasBuilder.Identifier = storedPolicyName;
        }

        Uri sasUri = blobClient.GenerateSasUri(sasBuilder);
        Console.WriteLine("SAS URI for blob is: {0}", sasUri);
        Console.WriteLine();

        return sasUri;
    }
    else
    {
        Console.WriteLine(@"BlobClient must be authorized with Shared Key 
                          credentials to create a service SAS.");
        return null;
    }
}
```

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

若要为 blob 创建服务 SAS，请调用 [CloudBlob.GetSharedAccessSignature](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob.getsharedaccesssignature) 方法。

```csharp
private static string GetBlobSasUri(CloudBlobContainer container,
                                    string blobName,
                                    string policyName = null)
{
    string sasBlobToken;

    // Get a reference to a blob within the container.
    // Note that the blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters
        // of an ad hoc SAS, and to construct a shared access policy that is saved to
        // the container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be
            // the time when the storage service receives the request. Omitting the start time
            // for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read |
                          SharedAccessBlobPermissions.Write |
                          SharedAccessBlobPermissions.Create
        };

        // Generate the shared access signature on the blob,
        // setting the constraints directly on the signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the blob. In this case, all of the constraints
        // for the SAS are specified on the container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

# <a name="javascript-v12"></a>[JavaScript v12](#tab/javascript)

若要为 blob 创建服务 SAS，请调用 [CloudBlob.GetSharedAccessSignature](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob.getsharedaccesssignature) 方法。

若要为 blob 创建服务 SAS，请调用 [generateBlobSASQueryParameters](https://docs.microsoft.com/javascript/api/@azure/storage-blob/#generateBlobSASQueryParameters_BlobSASSignatureValues__StorageSharedKeyCredential_) 函数，提供所需选项。

```javascript
function getBlobSasUri(containerClient, blobName, sharedKeyCredential, storedPolicyName) {
    const sasOptions = {
        containerName: containerClient.containerName,
        blobName: blobName
    };

    if (storedPolicyName == null) {
        sasOptions.startsOn = new Date();
        sasOptions.expiresOn = new Date(new Date().valueOf() + 3600 * 1000);
        sasOptions.permissions = BlobSASPermissions.parse("r");
    } else {
        sasOptions.identifier = storedPolicyName;
    }

    const sasToken = generateBlobSASQueryParameters(sasOptions, sharedKeyCredential).toString();
    console.log(`SAS token for blob is: ${sasToken}`);

    return `${containerClient.getBlockBlobClient(blobName).url}?${sasToken}`;
}
```

---

## <a name="create-a-service-sas-for-a-directory"></a>为目录创建服务 SAS

在启用了分层命名空间的存储帐户中，可以为目录创建服务 SAS。 若要创建服务 SAS，请确保已安装 12.5.0 或更高版本的 [Azure.Storage.Files.DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/) 包。

下面的示例演示如何使用适用于 .NET 的 v12 客户端库为目录创建服务 SAS：

```csharp
private static Uri GetServiceSasUriForDirectory(DataLakeDirectoryClient directoryClient,
                                          string storedPolicyName = null)
{
    if (directoryClient.CanGenerateSasUri)
    {
        // Create a SAS token that's valid for one hour.
        DataLakeSasBuilder sasBuilder = new DataLakeSasBuilder()
        {
            // Specify the file system name, the path, and indicate that
            // the client object points to a directory.
            FileSystemName = directoryClient.FileSystemName,
            Resource = "d",
            IsDirectory = true,
            Path = directoryClient.Path,
        };

        // If no stored access policy is specified, create the policy
        // by specifying expiry and permissions.
        if (storedPolicyName == null)
        {
            sasBuilder.ExpiresOn = DateTimeOffset.UtcNow.AddHours(1);
            sasBuilder.SetPermissions(DataLakeSasPermissions.Read |
                DataLakeSasPermissions.Write |
                DataLakeSasPermissions.List);
        }
        else
        {
            sasBuilder.Identifier = storedPolicyName;
        }

        // Get the SAS URI for the specified directory.
        Uri sasUri = directoryClient.GenerateSasUri(sasBuilder);
        Console.WriteLine("SAS URI for ADLS directory is: {0}", sasUri);
        Console.WriteLine();

        return sasUri;
    }
    else
    {
        Console.WriteLine(@"DataLakeDirectoryClient must be authorized with Shared Key 
                          credentials to create a service SAS.");
        return null;
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

[!INCLUDE [storage-blob-javascript-resources-include](../../../includes/storage-blob-javascript-resources-include.md)]

## <a name="next-steps"></a>后续步骤

- [使用共享访问签名 (SAS) 授予对 Azure 存储资源的有限访问权限](../common/storage-sas-overview.md)
- [创建服务 SAS](https://docs.microsoft.com/rest/api/storageservices/create-service-sas)
