---
title: substring() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 substring()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 01/22/2021
ms.localizationpriority: high
ms.openlocfilehash: 7311e13d7813156818056addba32c165ce067630
ms.sourcegitcommit: 7be0e8a387d09d0ee07bbb57f05362a6a3c7b7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611641"
---
# <a name="substring"></a>substring()

从源字符串的某个索引开始到字符串结尾，提取子字符串。

（可选）可以指定请求子字符串的长度。

```kusto
substring("abcdefg", 1, 2) == "bc"
```

## <a name="syntax"></a>语法

`substring(`*source*`,` *startingIndex* [`,` *length*]`)`

## <a name="arguments"></a>参数

* *source*：将要从中提取子字符串的源字符串。
* startingIndex：所请求子字符串的从零开始的起始字符位置。
* *length*:一个可选参数，可用于在子字符串中指定所请求的字符数。 

**备注**

startingIndex 可以为负数，在这种情况下，将从源字符串的末尾检索子字符串。

## <a name="returns"></a>返回

给定字符串中的子字符串。 子字符串在 startingIndex（从零开始）字符位置开始，并持续到字符串或长度字符的末尾（如果指定）。

## <a name="examples"></a>示例

```kusto
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
substring("123456", -2, 2)    // 56
```