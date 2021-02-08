---
title: 防止 Azure Cosmos DB API for MongoDB 操作的速率限制错误。
description: 了解如何使用 SSR（服务器端重试）功能防止 Azure Cosmos DB API for MongoDB 操作遇到速率限制错误。
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
origin.date: 01/13/2021
author: rockboyfor
ms.date: 02/08/2021
ms.testscope: yes
ms.testdate: 02/01/2021
ms.author: v-yeche
ms.openlocfilehash: 9dd5f595ca0d5c766fd0bfd5fa0c68b628071559
ms.sourcegitcommit: 0232a4d5c760d776371cee66b1a116f6a5c850a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99580586"
---
<!--Verified successfully-->
# <a name="prevent-rate-limiting-errors-for-azure-cosmos-db-api-for-mongodb-operations"></a>防止 Azure Cosmos DB API for MongoDB 操作的速率限制错误
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

如果 Azure Cosmos DB API for MongoDB 操作超出集合的吞吐量限制 (RU)，则可能失败，出现速率限制 (16500/429) 错误。 

可以启用服务器端重试 (SSR) 功能，并让服务器自动重试这些操作。 短暂延迟后，你的帐户中的所有集合的请求会重试。 此功能是处理客户端应用程序中的速率限制错误的便捷替代方法。

## <a name="use-the-azure-portal"></a>使用 Azure 门户

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

1. 导航到你的 Azure Cosmos DB API for MongoDB 帐户。

1. 转到“设置”部分下面的“功能”窗格 。

1. 选择“服务器端重试”。

1. 单击“启用”为帐户中的所有集合启用此功能。

:::image type="content" source="./media/prevent-rate-limiting-errors/portal-features-server-side-retry.png" alt-text="适用于 Azure Cosmos DB API for MongoDB 的服务器端重试功能的屏幕截图":::

## <a name="next-steps"></a>后续步骤

若要了解有关排除常见错误的更多信息，请参阅此文：

* [解决 Azure Cosmos DB 的 API for MongoDB 中的常见问题](mongodb-troubleshoot.md)

<!-- Update_Description: new article about prevent rate limiting errors -->
<!--NEW.date: 02/01/2021-->