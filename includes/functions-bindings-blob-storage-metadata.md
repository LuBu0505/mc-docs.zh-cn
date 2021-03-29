---
title: 包含文件
description: 包含文件
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 03/22/2021
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: b79ca3b618e090f609d43c787fd5eb168dba3cc1
ms.sourcegitcommit: bed93097171aab01e1b61eb8e1cec8adf9394873
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105609442"
---
Blob 触发器提供了几个元数据属性。 这些属性可在其他绑定中用作绑定表达式的一部分，或者用作代码中的参数。 这些值的语义与 [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob) 类型相同。

|属性  |类型  |描述  |
|---------|---------|---------|
|`BlobTrigger`|`string`|触发 Blob 的路径。|
|`Uri`|`System.Uri`|主位置的 blob 的 URI。|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Blob 的系统属性。 |
|`Metadata` |`IDictionary<string,string>`|Blob 的用户定义元数据。|

例如，以下 C# 脚本和 JavaScript 示例会记录触发 blob 的路径，包括容器：

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```
