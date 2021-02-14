---
title: 清除令牌缓存 (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: 了解如何使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 清除令牌缓存。
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 02/02/2021
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 82eeadff09bda228a8b9e767d000838ece877564
ms.sourcegitcommit: ef5fa52ac5e0e3881f72bd8b56fc73e49444ccc2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99540778"
---
# <a name="clear-the-token-cache-using-msalnet"></a>使用 MSAL.NET 清除令牌缓存

使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) [获取访问令牌](msal-acquire-cache-tokens.md)时，将缓存该令牌。 应用程序需要令牌时，应该首先调用 `AcquireTokenSilent` 方法来验证缓存中是否有可接受的令牌。 

通过从缓存中删除帐户来实现清除缓存。 不过，这不会删除浏览器中的会话 Cookie。  以下示例实例化公共客户端应用程序，获取应用程序的帐户，并删除帐户。

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

若要详细了解如何获取和缓存令牌，请阅读[获取访问令牌](msal-acquire-cache-tokens.md)。
