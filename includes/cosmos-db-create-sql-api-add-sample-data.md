---
title: include 文件
description: include 文件
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: include
origin.date: 08/07/2019
ms.date: 03/15/2021
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 68743ba196df5ec68575ecfef7a6e286354fada1
ms.sourcegitcommit: fb2fba1c106406553ed84b8652a915c823d9ab07
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "102996574"
---
现在可以使用数据资源管理器将数据添加到新容器。

1. 在“数据资源管理器”中展开“Tasks”数据库，然后展开“Items”容器。 依次选择“项”、“新建项”。

    ![在 Azure 门户的数据资源管理器中创建新文档](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-new-document.png)

2. 现在，将文档添加到具有以下结构的容器。

    ```json
    {
        "id": "1",
        "category": "personal",
        "name": "groceries",
        "description": "Pick up apples and strawberries.",
        "isComplete": false
    }
    ```

3. 将 json 添加到“文档”选项卡以后，即可选择“保存”。

    ![通过复制添加 JSON 数据，然后在 Azure 门户上的数据资源管理器中选择“保存”](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-save-document.png)

4. 再创建并保存一个文档，在其中插入 `id` 属性的唯一值，并将其他属性更改为适当值。 新文档可以具有所需的任何结构，因为 Azure Cosmos DB 不对数据施加任何架构。

<!-- Update_Description: wording update -->
