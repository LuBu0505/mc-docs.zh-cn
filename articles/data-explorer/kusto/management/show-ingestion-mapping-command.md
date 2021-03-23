---
title: .show ingestion mappings - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 .show ingestion mappings。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 03/18/2021
ms.openlocfilehash: 4bf57cea969bef308fbf7dd1c16c25f7a1945ba7
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766397"
---
# <a name="show-ingestion-mapping"></a>.show ingestion mapping

显示引入映射（所有引入映射或按名称指定的某个引入映射）。

* `.show` `table` *TableName* `ingestion` *MappingKind*  `mappings`

* `.show` `table` *TableName* `ingestion` *MappingKind*  `mapping` *MappingName* 

显示所有映射类型的所有引入映射：

* `.show` `table` *TableName* `ingestion`  `mappings`
 
**示例** 
 
```kusto
.show table MyTable ingestion csv mapping "Mapping1" 

.show table MyTable ingestion csv mappings 

.show table MyTable ingestion mappings 
```

**示例输出**

| 名称     | 种类 | 映射     |
|----------|------|-------------|
| mapping1 | CSV  | `[{"Name":"rownumber","DataType":"int","CsvDataType":null,"Ordinal":0,"ConstValue":null},{"Name":"rowguid","DataType":"string","CsvDataType":null,"Ordinal":1,"ConstValue":null}]` |
