---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 函数 - Azure Databricks
description: 了解 Azure Databricks 中支持的 SQL 语言构造中的 SQL 函数。
ms.openlocfilehash: 94ed07866cb43a2bc36e729798275f9e7bbceb38
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060650"
---
# <a name="functions"></a>函数

Spark SQL 提供了两个函数功能来满足各种需求：内置函数和用户定义函数 (UDF)。

## <a name="built-in-functions"></a>内置函数

本文介绍了用于聚合、数组/映射、日期/时间戳和 JSON 数据的常用内置函数类别的用法和说明。

* [内置函数](sql-ref-functions-builtin.md)

## <a name="user-defined-functions"></a>用户定义的函数

用户定义函数 (UDF) 允许你在系统内置函数不足以执行所需任务时定义自己的函数。 若要使用 UDF，请先定义函数，然后将函数注册到 Spark 中，最后调用已注册的函数。 用户定义函数可以对单个行执行操作，也可以一次对多个行执行操作。 Spark SQL 还支持与 UDF、UDAF 和 UDTF 的现有 Hive 实现集成。

* [用户定义的聚合函数 (UDAF)](sql-ref-functions-udf-aggregate.md)
* [与 Hive UDF、UDAF 和 UDTF 的集成](sql-ref-functions-udf-hive.md)
* [用户定义的标量函数 (UDF)](sql-ref-functions-udf-scalar.md)