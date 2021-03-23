---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 06/11/2020
ms.date: 01/22/2021
ms.author: v-tawe
ms.openlocfilehash: 013c4cc957abecdbd8cd06a67092149b6b034e77
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "104767234"
---
以下参数决定了你可在表中进行的更改：
* 表类型为“新”或“现有”
* 映射类型为“新”或“现有”

表类型 | 映射类型 | 可用调整|
|---|---|---|
|新建表   | 新映射 |更改数据类型，重命名列，新建列，删除列，更新列，升序排序，降序排序  |
|现有表  | 新映射 | 新建列（你随后可在其上更改数据类型、进行重命名和更新）， <br> 更新列，升序排序，降序排序  |
| | 现有映射 | 升序排序，降序排序

> [!NOTE]
> 添加新列或更新列时，可更改映射转换。 有关详细信息，请参阅[映射转换](../ingest-data-one-click.md#mapping-transformations)