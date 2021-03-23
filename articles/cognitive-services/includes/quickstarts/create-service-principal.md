---
title: 创建 Azure 服务主体
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/08/2021
ms.author: v-johya
ms.openlocfilehash: bfa054a27983d67e351b22d5af7c40a7643a590f
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104803296"
---
## <a name="create-an-azure-service-principal"></a>创建 Azure 服务主体

若要使应用程序与 Azure 帐户交互，需要使用 Azure 服务主体来管理权限。 请按照[创建 Azure 服务主体](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?viewFallbackFrom=azps-3.3.0)中的说明操作。

创建服务主体时，你会看到它有一个机密值、一个 ID 和一个应用程序 ID。 将应用程序 ID 和机密保存到某个临时位置，以供后续步骤使用。

