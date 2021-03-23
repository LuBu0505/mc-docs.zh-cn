---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 03/30/2020
ms.date: 09/24/2020
ms.author: v-tawe
ms.openlocfilehash: ab82daa11c383a226bbcd40cc55615ef88a8136f
ms.sourcegitcommit: f3fee8e6a52e3d8a5bd3cf240410ddc8c09abac9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "104767170"
---
## <a name="explore-quick-queries-and-tools"></a>探索快速查询和工具

在引入进度下方的磁贴中，探索“快速查询”或“工具” ： 
 * “快速查询”包含指向 Web UI（其中包含示例查询）的链接。
 * “工具”包含一个指向 Web UI 上的“撤消”或“删除新数据”的链接，因此，你可以通过运行相关的 `.drop` 命令来排查问题  。

     > [!NOTE]
     > 使用 `.drop` 命令时，可能会丢失数据。 请谨慎使用。
     > Drop 命令只会还原此引入流所做的更改（新建范围和列）， 而不会删除任何其他内容。
