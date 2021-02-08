---
title: 映射数据流中的筛选转换
description: 使用 Azure 数据工厂映射数据流中的筛选转换筛选掉行
author: WenJason
ms.author: v-jay
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
origin.date: 05/26/2020
ms.date: 02/01/2021
ms.openlocfilehash: 7d9df39b935639ce5cba50788a3b5be3692343b7
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060785"
---
# <a name="filter-transformation-in-mapping-data-flow"></a>映射数据流中的筛选转换

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

筛选转换允许根据条件对行进行筛选。 输出流包括与筛选条件匹配的所有行。 筛选转换类似于 SQL 中的 WHERE 子句。

## <a name="configuration"></a>Configuration

使用数据流表达式生成器为筛选条件输入表达式。 若要打开表达式生成器，请单击蓝色框。 筛选条件必须为布尔型。 有关如何创建表达式的详细信息，请参阅[表达式生成器](concepts-data-flow-expression-builder.md)文档。

![筛选转换](media/data-flow/filter1.png "筛选转换")

## <a name="data-flow-script"></a>数据流脚本

### <a name="syntax"></a>语法

```
<incomingStream>
    filter(
        <conditionalExpression>
    ) ~> <filterTransformationName>
```

### <a name="example"></a>示例

下面的示例是一个名为 `FilterBefore1960` 的筛选转换，它接收传入流 `CleanData`。 筛选条件是 `year <= 1960` 表达式。

在数据工厂 UX 中，此转换如下图所示：

![筛选转换](media/data-flow/filter1.png "筛选转换")

此转换的数据流脚本位于下面的代码片段中：

```
CleanData
    filter(
        year <= 1960
    ) ~> FilterBefore1960

```

## <a name="next-steps"></a>后续步骤

使用[选择转换](data-flow-select.md)筛选掉列
