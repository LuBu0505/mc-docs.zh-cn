---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 多个列上的警报 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 中多个列上的警报。
ms.openlocfilehash: 55cecb3bc00d574196a41a18abe058642c3c4502
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692168"
---
# <a name="alert-on-multiple-columns"></a>多个列上的警报

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

若要基于查询的多个列设置警报，查询可实现警报逻辑，并返回一个布尔值作为警报触发阈值。 例如：

```sql
SELECT CASE WHEN drafts_count > 10000 AND archived_count > 5000 THEN 1 ELSE 0 END
FROM (
SELECT sum(CASE WHEN is_archived THEN 1 ELSE 0 END) AS archived_count,
sum(CASE WHEN is_draft THEN 1 ELSE 0 END) AS drafts_count
FROM queries) data
```

``drafts_count > 10000 and archived_count > 5000`` 时，此查询将返回 ``1``。
然后，可将警报配置为在值为 ``1`` 时触发。