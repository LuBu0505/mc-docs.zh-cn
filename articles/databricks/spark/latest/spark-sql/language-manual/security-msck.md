---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/04/2020
title: MSCK - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL 语言的 MSCK 语法。
ms.openlocfilehash: ab5ddf1fd68252ebc1ae4e5e5f2ca5d97f2a46b6
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060933"
---
# <a name="msck"></a>MSCK

删除与表关联的所有特权。

## <a name="syntax"></a>语法

```sql
MSCK TABLE [db_name.]table_name
```