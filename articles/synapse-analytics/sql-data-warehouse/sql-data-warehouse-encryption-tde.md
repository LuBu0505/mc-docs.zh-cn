---
title: 用于专用 SQL 池（以前称为 SQL DW）的透明数据加密（门户）
description: 用于 Azure Synapse Analytics 中专用 SQL 池（以前称为 SQL DW）的透明数据加密 (TDE)
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
origin.date: 04/30/2019
ms.date: 01/11/2021
ms.author: v-jay
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: d99f8221282e18a063e9d9d6efd137ec03379554
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023049"
---
# <a name="get-started-with-transparent-data-encryption-tde-for-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>用于 Azure Synapse Analytics 中专用 SQL 池（以前称为 SQL DW）的透明数据加密 (TDE) 入门

> [!div class="op_single_selector"]
>
> * [安全性概述](sql-data-warehouse-overview-manage-security.md)
> * [身份验证](sql-data-warehouse-authentication.md)
> * [加密（门户）](sql-data-warehouse-encryption-tde.md)
> * [加密 (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permissions"></a>所需权限

若要启用透明数据加密 (TDE)，用户必须是管理员或 dbmanager 角色的成员。

## <a name="enabling-encryption"></a>启用加密

若要启用 TDE，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.cn)
2. 在数据库边栏选项卡中，单击“设置”按钮 
3. 选择“透明数据加密”  选项![门户设置](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. 选择“开”  设置![门户设置“开”](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png)
5. 选择“保存”  
   ![门户设置“保存”](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png)  

## <a name="disabling-encryption"></a>禁用加密

若要禁用 TDE，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.cn)
2. 在数据库边栏选项卡中，单击“设置”按钮 
3. 选择“透明数据加密”  选项![门户设置](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. 选择“关”  设置![门户设置“关”](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png)
5. 选择“保存”  
   ![门户设置“保存”2](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png)  

## <a name="encryption-dmvs"></a>加密 DMV

可使用下列 DMV 来确认加密：

* [sys.databases](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
