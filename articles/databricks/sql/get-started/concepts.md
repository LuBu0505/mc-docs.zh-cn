---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: Azure Databricks SQL Analytics 概念 - Azure Databricks
description: 了解基本的 Azure Databricks SQL Analytics 概念。
ms.openlocfilehash: 65d4d0bb380284374b5c31a96cdbb4781f8da5b9
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692404"
---
# <a name="azure-databricks-sql-analytics-concepts"></a><a id="azure-databricks-sql-analytics-concepts"> </a><a id="concepts"> </a>Azure Databricks SQL Analytics 概念

> [!IMPORTANT]
>
> 此功能目前以[公共预览版](../../release-notes/release-types.md)提供。 请联系 Azure Databricks 代表，以申请访问权限。

本文介绍有效使用 Azure Databricks SQL Analytics 需要了解的一组基本概念。

## <a name="interface"></a>接口

本节介绍 Azure Databricks 支持的用于访问 Azure Databricks SQL Analytics 资产的接口：UI 和 API。

**UI**：仪表板和查询、SQL 端点、查询历史记录和警报的图形界面。

> [!div class="mx-imgBorder"]
> ![登陆页面](../../_static/images/sql/landing-sql-analytics.png)

**[REST API](../api/index.md)** 使你能够在 SQL 终结点和查询历史记录上自动执行任务的界面。

## <a name="data-management"></a>数据管理

**[SQL 终结点](../admin/sql-endpoints.md)** ：与在其上运行 SQL 查询的一组内部数据对象的连接。

**[外部数据源](../admin/external-data-sources/index.md)** ：与在其上运行 SQL 查询的一组外部数据对象的连接。

**[可视化效果](../user/visualizations/index.md)** ：运行查询的结果的图形表示形式。

**[仪表板](../user/dashboards/index.md)** ：查询可视化效果和注释的表示形式。

**[警报](../user/alerts/index.md)** ：关于查询所返回的字段已达到阈值的通知。

## <a name="computation-management"></a>计算管理

本节介绍在 Azure Databricks SQL Analytics 中运行 SQL 查询时需要了解的概念。

**[查询](../user/queries/index.md)** ：可在连接上运行的有效 SQL 语句。

**[查询历史记录](../admin/query-history.md)** ：已执行的查询及其性能特征的列表。

## <a name="authentication-and-authorization"></a>身份验证和授权

本节介绍管理 Azure Databricks <SQA> 用户和组及其对资产的访问时需要了解的概念。

**[用户和组](../admin/users-groups.md)** ：用户是有权访问系统的唯一个体。 组是用户的集合。

**[个人访问令牌](../user/security/personal-access-tokens.md)** ：不透明字符串用于对 REST API 进行身份验证，并由[商业智能工具](../../integrations/bi/index.md)连接到 SQL 终结点。

**[访问控制列表](../user/security/access-control/data-acl.md)** ：附加到需要访问对象的主体的一组权限。 ACL 条目指定对象以及允许对该对象执行的操作。 ACL 中的每个条目指定一个主体、操作类型和对象。