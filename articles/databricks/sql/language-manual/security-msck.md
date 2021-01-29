---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: MSCK - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 MSCK 语法。
ms.openlocfilehash: 471632320a17028a11f8aacdb1647e6421f2d944
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692143"
---
# <a name="msck"></a>MSCK

删除与表关联的所有特权。

## <a name="syntax"></a>语法

```sql
MSCK TABLE [db_name.]table_name
```