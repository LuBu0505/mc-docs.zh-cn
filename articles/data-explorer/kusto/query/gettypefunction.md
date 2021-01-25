---
title: gettype() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 gettype()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 01/22/2021
ms.openlocfilehash: cc65a2818d34b4303d5b2367b28831ae6d430bcd
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611553"
---
# <a name="gettype"></a>gettype()

返回其单个参数的运行时类型。

对于其名义（静态）类型为 `dynamic` 的表达式，运行时类型可能不同于名义类型；在这种情况下，`gettype()` 可用于揭示实际值的类型（内存中值的编码方式）。

## <a name="syntax"></a>语法

`gettype(`Expr`)`

## <a name="returns"></a>返回

字符串，表示其单个参数的运行时类型。

## <a name="examples"></a>示例

|表达式                          |返回      |
|------------------------------------|-------------|
|`gettype("a")`                      |`string`     |
|`gettype(111)`                      |`long`       |
|`gettype(1==1)`                     |`bool`       |
|`gettype(now())`                    |`datetime`   |
|`gettype(1s)`                       |`timespan`   |
|`gettype(parse_json('1'))`           |`int`        |
|`gettype(parse_json(' "abc" '))`     |`string`     |
|`gettype(parse_json(' {"abc":1} '))` |`dictionary` | 
|`gettype(parse_json(' [1, 2, 3] '))` |`array`      |
|`gettype(123.45)`                   |`real`       |
|`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))`|`guid`| 
|`gettype(parse_json(''))`            |`null`|