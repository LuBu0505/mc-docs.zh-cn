---
title: 映射数据流中的多个分支
description: 复制包含多个分支的映射数据流中的数据流
author: WenJason
ms.author: v-jay
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
origin.date: 01/08/2020
ms.date: 02/01/2021
ms.openlocfilehash: 0963930316e5606a78ba44f3e8d238481db48893
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060779"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>在映射数据流中创建新分支

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

添加新分支，对同一数据流执行多组操作和转换。 如果要为多个接收器使用相同的源，或将自联接数据一起使用，则添加新分支会很有用。

与其他转换类似，可以从转换列表中添加新的分支。 仅在你尝试分支的转换之后存在现有转换时，“新建分支”才可作为操作使用。

![屏幕截图显示“多个输入/输出”菜单中的“新建分支”选项。](media/data-flow/new-branch2.png "添加新分支")

在下面的示例中，数据流正在读取出租车行程数据。 需要按日期和供应商汇总的输出。 可以添加一个新的分支，而不是创建从同一源读取的两个单独的数据流。 这样，两种聚合都可以作为同一数据流的一部分执行。 

![屏幕截图显示来自源的两个分支的数据流。](media/data-flow/new-branch.png "添加新分支")
