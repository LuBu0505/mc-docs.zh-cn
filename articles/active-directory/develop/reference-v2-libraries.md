---
title: Microsoft 标识平台身份验证库 | Azure
description: 与 Microsoft 标识平台兼容的客户端库和中间件的列表。 使用这些库可添加对用户登录（身份验证）和对应用程序的受保护的 Web API 访问（授权）的支持。
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 02/23/2021
ms.author: v-junlch
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 91b86c2efef821b9af3323b7f65c7e75dd50d685
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697420"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Microsoft 标识平台身份验证库

下表显示了针对多种应用程序类型的 Microsoft 身份验证库支持。 它们包括指向库源代码的链接，获取应用项目包的位置以及库是否支持用户登录（身份验证），访问受保护的 Web API（授权）或两者兼而有之。

Microsoft 标识平台已由 OpenID Foundation 认证为[经认证的 OpenID 提供程序](https://openid.net/certification/)。 如果你希望使用 Microsoft 身份验证库 (MSAL) 以外的其他库或 Microsoft 支持的其他库，请选择具有[经认证的 OpenID Connect 实现](https://openid.net/developers/certified/)的库。

如果选择手动编码自己的 [OAuth 2.0 或 OpenID Connect 1.0](active-directory-v2-protocols.md) 的协议级实现，请密切注意每个标准规范中的安全性注意事项，并遵循软件开发生命周期 (SDL) 方法，例如 [Microsoft SDL][Microsoft-SDL]。

## <a name="single-page-application-spa"></a>单页面应用程序 (SPA)

单页应用程序完全在浏览器表面上运行，以动态方式或在应用程序加载时提取页面数据（HTML、CSS 和 JavaScript）。 它可以调用 Web API 与后端数据源进行交互。

因为 SPA 的代码完全在浏览器中运行，所以它被视为公共客户端，无法安全存储机密。

| 语言/框架 | 项目<br/>GitHub                                                                                                    | 包                                                                      | 获取<br/>started                             | 用户登录                                         | 访问 Web API                                                 | 正式发布 (GA) 或<br/>公共预览版<sup>1</sup> |
|----------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|:-----------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Angular              | [MSAL Angular 2.0](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)         | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     | —                                               | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | 公共预览版                                               |
| Angular              | [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/msal-angular-v1/lib/msal-angular) | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     | [教程](tutorial-v2-angular.md)              | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| AngularJS            | [MSAL AngularJS](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs)         | [@azure/msal-angularjs](https://www.npmjs.com/package/@azure/msal-angularjs) | —                                               | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | 公共预览版                                               |
| JavaScript           | [MSAL.js 2.0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)              | [@azure/msal-browser](https://www.npmjs.com/package/@azure/msal-browser)     |  | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| JavaScript           | [MSAL.js 1.0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-core)              | [@azure/msal-core](https://www.npmjs.com/package/@azure/msal-core)     | [教程](tutorial-v2-javascript-spa.md) | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| React                | [MSAL React](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-react)                 | [@azure/msal-react](https://www.npmjs.com/package/@azure/msal-react)         | —                                               | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | 公共预览版                                               |
<!--
| Vue | [Vue MSAL]( https://github.com/mvertopoulos/vue-msal) | [vue-msal]( https://www.npmjs.com/package/vue-msal) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Azure 预览版的补充使用条款][preview-tos]适用于公共预览版中的库。

## <a name="web-application"></a>Web 应用程序

Web 应用程序在服务器上运行代码，该服务器生成 HTML、CSS 和 JavaScript 并将其发送到用户的 Web 浏览器以进行呈现。 系统将用户标识作为用户浏览器（前端）和 Web 服务器（后端）之间的会话来进行维护。

由于 Web 应用程序的代码在 Web 服务器上运行，因此它被视为可以安全存储机密的机密客户端。

| 语言/框架 | 项目<br/>GitHub                                                                                     | 包                                                                                                    | 获取<br/>started                               | 用户登录                                            | 访问 Web API                                                    | 正式发布 (GA) 或<br/>公共预览版<sup>1</sup> |
|----------------------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|:-------------------------------------------------:|:--------------------------------------------------------:|:------------------------------------------------------------------:|:------------------------------------------------------------:|
| .NET                 | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)                      | —                                                 | ![库无法为用户登录请求 ID令牌。][n] | ![库可以为受保护的 Web API 请求访问令牌。][y]    | GA                                                           |
| ASP.NET Core         | [ASP.NET 安全性](https://docs.microsoft.com/aspnet/core/security/)                                                                | [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) | —                                                 | ![库可以为用户登录请求 ID令牌。][y]    | ![库无法为受保护的 Web API 请求访问令牌。][n] | GA                                                           |
| ASP.NET Core         | [Microsoft.Identity.Web](https://github.com/AzureAD/microsoft-identity-web)                               | [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web)                            | —                                                 | ![库可以为用户登录请求 ID令牌。][y]    | ![库可以为受保护的 Web API 请求访问令牌。][y]    | GA                                                           |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)                            | [msal4j](https://search.maven.org/artifact/com.microsoft.azure/msal4j)                                     | [快速入门](quickstart-v2-java-webapp.md)        | ![库可以为用户登录请求 ID令牌。][y]    | ![库可以为受保护的 Web API 请求访问令牌。][y]    | GA                                                           |
| Node.js              | [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-node](https://www.npmjs.com/package/@azure/msal-node)                                                | [快速入门](quickstart-v2-nodejs-webapp-msal.md) | ![库可以为用户登录请求 ID令牌。][y]    | ![库可以为受保护的 Web API 请求访问令牌。][y]    | GA                                               |
| Node.js              | [Azure AD Passport](https://github.com/AzureAD/passport-azure-ad)                                         | [passport-azure-ad](https://www.npmjs.com/package/passport-azure-ad)                                       | [快速入门](quickstart-v2-nodejs-webapp.md)      | ![库可以为用户登录请求 ID令牌。][y]    | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| Python               | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)                     | [msal](https://pypi.org/project/msal)                                                                      | [快速入门](quickstart-v2-python-webapp.md)      | ![库可以为用户登录请求 ID令牌。][y]    | ![库可以为受保护的 Web API 请求访问令牌。][y]    | GA                                                           |
<!--
| Java | [ScribeJava](https://github.com/scribejava/scribejava) | [ScribeJava 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
| Java | [Gluu oxAuth](https://github.com/GluuFederation/oxAuth) | [oxAuth 3.0.2](https://github.com/GluuFederation/oxAuth/releases/tag/3.0.2) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| Node.js | [openid-client](https://github.com/panva/node-openid-client/) | [openid-client 2.4.5](https://github.com/panva/node-openid-client/releases/tag/v2.4.5) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| PHP | [PHP League oauth2-client](https://github.com/thephpleague/oauth2-client) | [oauth2-client 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth) | [omniauth 1.3.1](https://github.com/omniauth/omniauth/releases/tag/v1.3.1)<br/>[omniauth-oauth2 1.4.0](https://github.com/intridea/omniauth-oauth2) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Azure 预览版的补充使用条款][preview-tos]适用于公共预览版中的库。

## <a name="desktop-application"></a>桌面应用程序

桌面应用程序通常是显示用户界面的二进制（编译）代码，旨在在用户桌面上运行。

由于桌面应用程序在用户的桌面上运行，因此它被视为无法安全存储机密的公共客户端。

| 语言/框架 | 项目<br/>GitHub                                                                                     | 包                                                                               | 获取<br/>started                        | 用户登录                                         | 访问 Web API                                                 | 正式发布 (GA) 或<br/>公共预览版<sup>1</sup> |
|----------------------|-----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|:------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Electron             | [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [@azure/msal-node](https://www.npmjs.com/package/@azure/msal-node)                    | [教程](tutorial-v2-nodejs-desktop.md)   | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                               |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)                            | [msal4j](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j)               | —                                          | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| macOS (Swift/Obj-C)  | [适用于 iOS 和 macOS 的 MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc)            | [MSAL](https://cocoapods.org/pods/MSAL)                                               | [教程](tutorial-v2-ios.md)             | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| UWP                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [教程](tutorial-v2-windows-uwp.md)     | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| WPF                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [教程](tutorial-v2-windows-desktop.md) | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
<!--
| Java | Scribe | [Scribe Java](https://mvnrepository.com/artifact/org.scribe/scribe) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| React Native | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth/blob/main/docs/config-examples/azure-active-directory.md) | [react-native-app-auth](https://www.npmjs.com/package/react-native-app-auth) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Azure 预览版的补充使用条款][preview-tos]适用于公共预览版中的库。

## <a name="mobile-application"></a>移动应用程序

移动应用程序通常是显示用户界面的二进制（编译）代码，旨在在用户的移动设备上运行。

由于移动应用程序在用户的移动设备上运行，因此它被视为无法安全存储机密的公共客户端。

| 平台          | 项目<br/>GitHub                                                                          | 包                                                                               | 获取<br/>started                    | 用户登录                                         | 访问 Web API                                                 | 正式发布 (GA) 或<br/>公共预览版<sup>1</sup> |
|-------------------|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|:--------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Android (Java)    | [MSAL Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)        | [MSAL](https://mvnrepository.com/artifact/com.microsoft.identity.client/msal)         | [快速入门](quickstart-v2-android.md) | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| Android (Kotlin)  | [MSAL Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)        | [MSAL](https://mvnrepository.com/artifact/com.microsoft.identity.client/msal)         | —                                      | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| iOS (Swift/Obj-C) | [适用于 iOS 和 macOS 的 MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [MSAL](https://cocoapods.org/pods/MSAL)                                               | [教程](tutorial-v2-ios.md)         | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| Xamarin (.NET)    | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)             | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | —                                      | ![库可以为用户登录请求 ID令牌。][y] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
<!--
| React Native |[React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth/blob/main/docs/config-examples/azure-active-directory.md) | [react-native-app-auth](https://www.npmjs.com/package/react-native-app-auth) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Azure 预览版的补充使用条款][preview-tos]适用于公共预览版中的库。

## <a name="service--daemon"></a>服务/守护程序

服务和守护程序通常用于服务器到服务器通信以及其他无人参与（有时称为无外设）的通信。 因为没有用户在键盘上输入凭据或同意访问资源，所以在请求对 Web API 资源的授权访问时，这些应用程序将以自身而非用户身份进行身份验证。

在服务器上运行的服务或守护程序被视为可以安全存储其机密的机密客户端。

| 语言/框架 | 项目<br/>GitHub                                                                 | 包                                                                                | 获取<br/>started                           | 用户登录                                            | 访问 Web API                                                 | 正式发布 (GA) 或<br/>公共预览版<sup>1</sup> |
|----------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|:---------------------------------------------:|:--------------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| .NET                 | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)    | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client/) | [快速入门](quickstart-v2-netcore-daemon.md) | ![库无法为用户登录请求 ID令牌。][n] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)        | [msal4j](https://javadoc.io/doc/com.microsoft.azure/msal4j/latest/index.html)          | —                                             | ![库无法为用户登录请求 ID令牌。][n] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA                                                           |
| 节点               | [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-node](https://www.npmjs.com/package/@azure/msal-node)  | [快速入门](quickstart-v2-nodejs-console.md)  | ![库无法为用户登录请求 ID令牌。][n] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA  |
| Python               | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python) | [msal-python](https://github.com/AzureAD/microsoft-authentication-library-for-python)  | —  | ![库无法为用户登录请求 ID令牌。][n] | ![库可以为受保护的 Web API 请求访问令牌。][y] | GA |
<!--
|PHP| [The PHP League oauth2-client](https://oauth2-client.thephpleague.com/usage/) | [League\OAuth2](https://oauth2-client.thephpleague.com/) | ![Green check mark.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Azure 预览版的补充使用条款][preview-tos]适用于公共预览版中的库。

## <a name="next-steps"></a>后续步骤

有关 Microsoft 身份验证库的详细信息，请参阅 [Microsoft 身份验证库 (MSAL) 概述](msal-overview.md)。

<!--Image references-->
[y]: ./media/common/yes.png
[n]: ./media/common/no.png

<!--Reference-style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[Microsoft-SDL]: https://www.microsoft.com/securityengineering/sdl/
[preview-tos]: https://www.azure.cn/support/legal/
