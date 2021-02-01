---
title: 安装 Visual Studio 2019
description: 安装 Visual Studio 和适用于 Synapse SQL 的 SQL Server 开发工具 (SSDT)
services: synapse-analytics
ms.custom: vs-azure, azure-synapse
ms.workload: azure-vs
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
origin.date: 05/11/2020
ms.date: 02/01/2021
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: bb088e4cde0aa35bfc264f83c9d831ece77735ba
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058416"
---
# <a name="getting-started-with-visual-studio-2019"></a>Visual Studio 2019 入门

Visual Studio **2019** SQL Server Data Tools (SSDT) 是一个工具，可用于执行以下操作：

- 连接、查询和开发应用程序
- 利用对象资源管理器直观地浏览数据模型中的所有对象，包括表、视图、存储过程等。
- 为对象生成 T-SQL 数据定义语言 (DDL) 脚本
- 对 SSDT 数据库项目使用基于状态的方法来开发数据仓库
- 将数据库项目与源代码管理系统（例如 Git 和 Azure Repos）集成
- 通过自动化服务器（如 Azure DevOps）设置持续集成和部署管道

## <a name="install-visual-studio-2019"></a>安装 Visual Studio 2019

请参阅 [下载 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 以下载并安装 Visual Studio **16.3 及更高版本**。 在安装过程中，选择“数据存储和处理”工作负载。 Visual Studio 2019 不再需要单独安装 SSDT。

## <a name="unsupported-features-in-ssdt"></a>SSDT 中不支持的功能

有时，Synapse SQL 的功能版可能不包括对 SSDT 的支持。 目前不支持以下功能：


- [工作负荷管理](sql-data-warehouse-workload-management.md) - 工作负荷组和分类器
- [行级安全性](https://docs.microsoft.com/sql/relational-databases/security/row-level-security?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)（包括表值函数）
- 具有[标识列](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql-identity-property?view=sql-server-ver15)的表
- 某些 T-SQL 功能，如：
   - [STRING_AGG](https://docs.microsoft.com/sql/t-sql/functions/string-agg-transact-sql?view=sqlallproducts-allversions) 字符串函数中的 WITHIN GROUP 子句。

## <a name="next-steps"></a>后续步骤

安装最新版本的 SSDT 后，便可以[连接](sql-data-warehouse-query-visual-studio.md)到 SQL 池。
