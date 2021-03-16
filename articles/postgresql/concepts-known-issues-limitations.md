---
title: 已知问题和限制 - Azure Database for PostgreSQL - 单一服务器和灵活服务器（预览版）
description: 列出客户应该注意的已知问题。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 02/05/2020
ms.date: 03/08/2021
ms.openlocfilehash: 0f048fb2bb3e2d50a32543fea540b5f77a7a8339
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751757"
---
# <a name="azure-database-for-postgresql---known-issues-and-limitations"></a>Azure Database for PostgreSQL - 已知问题和限制

此页提供 Azure Database for PostgreSQL 中可能影响应用程序的已知问题列表。 它还列出了规避此问题的所有缓解措施和建议。

## <a name="intelligent-performance---query-store"></a>智能性能 - 查询存储

| 适用 | 原因 | 补救|
| ----- | ------ | ---- | 
| PostgreSQL 9.6、10、11 | 在某些罕见的情况下，打开服务器参数 `pg_qs.replace_parameter_placeholders` 可能会导致服务器关闭。 | 通过 Azure 门户的“服务器参数”部分，将参数 `pg_qs.replace_parameter_placeholders` 值切换为 `OFF` 并保存。   | 

## <a name="server-parameters"></a>服务器参数

| 适用 | 原因 | 补救| 
| ----- | ------ | ---- | 
| PostgreSQL 9.6、10、11 | 将服务器参数 `max_locks_per_transaction` 更改为比[建议值](https://www.postgresql.org/docs/11/kernel-resources.html)更高的值可能会导致服务器在重启后无法启动。 | 将其保留为默认值（32 或 64），或按照 PostgreSQL [文档](https://www.postgresql.org/docs/11/kernel-resources.html)中的说明更改为合理的值。 <br> <br> 从服务端来看，这是为了根据 SKU 限制较高的值。  | 

## <a name="next-steps"></a>后续步骤
- 请参阅查询存储[最佳做法](./concepts-query-store-best-practices.md)
