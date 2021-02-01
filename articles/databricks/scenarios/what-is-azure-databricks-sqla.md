---
title: 什么是 Azure Databricks SQL Analytics？
description: 了解 Azure Databricks SQL Analytics（一种支持对数据湖运行快速临时 SQL 查询的环境）。
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.subservice: databricks-sql-analytics
ms.workload: big-data
ms.topic: overview
ms.date: 04/10/2020
ms.author: mamccrea
ms.custom: mvc
ms.openlocfilehash: 10286869720ecc2f5ac8a162433e4fb86777632c
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061096"
---
# <a name="what-is-azure-databricks-sql-analytics-preview"></a>什么是 Azure Databricks SQL Analytics（预览版）？

Azure Databricks SQL Analytics 支持对数据湖运行快速临时 SQL 查询。 查询支持多种可视化类型，有助于从不同角度探索查询结果。

### <a name="fully-managed-sql-endpoints-in-the-cloud"></a>云中完全托管的 SQL 终结点

SQL 查询在完全托管的 SQL 终结点上运行，这些终结点的大小根据查询延迟需求和并发用户数进行了调整。

### <a name="dashboards-for-sharing-insights"></a>用于共享见解的仪表板

仪表板支持合并可视化效果和文本，用于共享通过查询获取的见解。

### <a name="alerts-help-you-monitor-and-integrate"></a>警报可助力监视和集成

查询返回的字段达到阈值时，你会收到警报。 使用警报来监视业务或将其与工具集成，以启动用户加入或支持工单等工作流。

## <a name="enterprise-security"></a>企业安全性

Azure Databricks SQL Analytics 提供企业级的 Azure 安全性，包括 Azure Active Directory 集成、基于角色的控制，以及可保护数据和业务的 SLA。

* 与 Azure Active Directory 集成后，可以使用 Azure Databricks SQL Analytics 运行基于 Azure 的完整解决方案。
* 基于角色的访问可实现针对警报、仪表板、SQL 终结点、查询和数据的精细化用户权限。
* 企业级 SLA。

> [!IMPORTANT]
>
> Azure Databricks SQL Analytics 是部署在全局 Azure 公有云基础结构上的 Microsoft Azure 第一方服务。 服务组件之间的所有通信（包括控制平面和客户数据平面中的公共 IP 之间的通信）都留在 Microsoft Azure 网络主干内进行。 另请参阅 [Microsoft 全球网络](https://docs.microsoft.com/azure/networking/microsoft-global-network)。

## <a name="integration-with-azure-services"></a>与 Azure 服务集成

Azure Databricks SQL Analytics 与以下 Azure 数据库和存储集成：Synapse Analytics、Cosmos DB、Data Lake Store 和 Blob 存储。

## <a name="integration-with-power-bi"></a>与 Power BI 集成

通过与 Power BI 的多样化集成，可在 Azure Databricks SQL Analytics 中快速轻松地发现和共享有影响力的见解。 还可以使用其他 BI 工具，例如 Tableau 软件。

## <a name="next-steps"></a>后续步骤

* [快速入门：启用用户和创建 SQL 终结点](/azure/databricks/sql/admin/admin-quickstart)
* [快速入门：运行查询和创建仪表板](/azure/databricks/sql/user/user-quickstart)
* [使用查询](/azure/databricks/sql/user/queries/index)
* [创建仪表板](/azure/databricks/sql/user/dashboards/index)

 
