---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/21/2020
title: 表访问控制 - Azure Databricks
description: 了解如何使用表访问控制来允许和拒绝对数据的访问。
ms.openlocfilehash: 0b01d0b7f3ee9711e95e2397ae7017937a408d97
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060079"
---
# <a name="table-access-control"></a>表访问控制

可以通过表访问控制以编程方式授予和撤销从 Python 和 SQL 访问数据的权限。

默认情况下，所有用户均有权访问存储在群集的托管表中的所有数据，除非已为该群集启用了表访问控制。 启用表访问控制后，用户可以为该群集上的数据对象设置权限。

## <a name="requirements"></a>要求

访问控制需要 [Azure Databricks Premium 计划](https://databricks.com/product/azure-pricing)。

本部分的内容：

* [为群集启用表访问控制](table-acl.md)
* [数据对象特权](object-privileges.md)