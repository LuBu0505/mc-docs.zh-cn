---
title: 获取多个资源的许可 (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: 了解用户如何使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 获得多个资源的预先同意。
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 02/23/2021
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 6901927944dfc77dd3342a0471deb27a5e6f267e
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101696973"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>用户使用 MSAL.NET 获得多个资源的同意
Microsoft 标识平台不允许你一次获取多个资源的令牌。 使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 时，获取令牌方法中的 scopes 参数应仅包含单个资源的范围。 但是，可以通过使用 `.WithExtraScopeToConsent` 生成器方法指定其他范围来预先同意多个资源。

> [!NOTE]
> 获得多个资源的同意适用于 Microsoft 标识平台，但不适用于 Azure AD B2C。 Azure AD B2C 仅支持管理员同意，不支持用户同意。

例如，如果你有两个资源（每个资源有两个范围）：

- https:\//mytenant.partner.onmschina.cn/customerapi（具有 2 个范围 `customer.read` 和 `customer.write`）
- https:\//mytenant.partner.onmschina.cn/vendorapi（具有 2 个范围 `vendor.read` 和 `vendor.write`）

应该使用具有 *extraScopesToConsent* 参数的 `.WithExtraScopeToConsent` 修饰符，如以下示例所示：

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.partner.onmschina.cn/customerapi/customer.read",
  "https://mytenant.partner.onmschina.cn/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.partner.onmschina.cn/vendorapi/vendor.read",
 "https://mytenant.partner.onmschina.cn/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

这将为你获得第一个 Web API 的访问令牌。 然后，若要访问第二个 Web API，可以从令牌缓存中以无提示方式获取令牌：

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
