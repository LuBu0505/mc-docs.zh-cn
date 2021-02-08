---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/13/2020
title: Explain - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Apache Spark 2.x SQL 语言的 EXPLAIN 语法。
ms.openlocfilehash: 3f55028999c63e1bc275318cbf6f9dd6db3e6c35
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061177"
---
# <a name="explain"></a>说明

```sql
EXPLAIN [EXTENDED | CODEGEN] statement
```

提供有关 ``statement`` 的详细计划信息，而无需实际运行。 默认情况下，这只会输出有关物理计划的信息。 不支持说明 ``DESCRIBE TABLE``。

**``EXTENDED``**

在分析和优化前后输出有关逻辑计划的信息。

**``CODEGEN``**

输出语句的生成代码（如果有）。