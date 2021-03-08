---
title: 将调用 Web API 的 Web 应用移动到生产 | Azure
titleSuffix: Microsoft identity platform
description: 了解如何将调用 Web API 的 Web 应用移到生产环境。
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/23/2021
ms.author: v-junlch
ms.custom: aaddev
ms.openlocfilehash: c5dfca0fb1c64efaa749665b8793d6d6c4c390a0
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697648"
---
# <a name="a-web-app-that-calls-web-apis-move-to-production"></a>调用 Web API 的 Web 应用：移动到生产

现在你已了解如何获取用于调用 Web API 的令牌，下面是将应用程序移到生产环境时需要考虑的一些事项。

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>后续步骤

通过试用 ASP.NET Core Web 应用的完整渐进式教程了解更多信息。 本教程：

- 演示如何将用户登录到多个受众或国家云，或使用社交标识登录。
- 调用 Microsoft Graph。
- 调用多个 Microsoft API。
- 处理增量同意。
- 调用自己的 Web API。

[ASP.NET Core Web 应用教程](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial#scope-of-this-tutorial)

<!--- Removing this diagram as it's already shown from the next step linked tutorial

![Tutorial overview](./media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
