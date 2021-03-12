---
title: 排查 Azure Cosmos DB 的“已禁止”异常的问题
description: 了解如何诊断和修复“已禁止”异常。
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
origin.date: 02/05/2021
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 03/01/2021
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 63fe5b686927023f9b47e78539b21b99688484ca
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055752"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-forbidden-exceptions"></a>诊断并排查 Azure Cosmos DB 的“已禁止”异常的问题
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

HTTP 状态代码 403 表示已禁止完成请求。

## <a name="firewall-blocking-requests"></a>防火墙阻止请求
在这种情况下，通常会看到如下错误：

```
Request originated from client IP {...} through public internet. This is blocked by your Cosmos DB account firewall settings.
```

```
Request is blocked. Please check your authorization token and Cosmos DB account firewall settings
```

### <a name="solution"></a>解决方案
验证当前[防火墙设置](how-to-configure-firewall.md)是否正确，并包括要尝试发起连接的 IP 或网络。
如果最近更新过这些设置，请记住，应用这些更改最多可能会需要 15 分钟。

## <a name="next-steps"></a>后续步骤
* 配置 [IP 防火墙](how-to-configure-firewall.md)。
* 配置从[虚拟网络](how-to-configure-vnet-service-endpoint.md)进行的访问。
* 配置从[专用终结点](how-to-configure-private-endpoints.md)进行的访问。

<!--Update_Description: new article about troubleshoot forbidden-->
<!--NEW.date: 03/01/2021-->