---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/03/2020
title: Alteryx - Azure Databricks
description: 了解如何将 Alteryx 与 Azure Databricks 配合使用。
ms.openlocfilehash: 3f599afca9bb3243435c636f5e5274268141a96e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058753"
---
# <a name="alteryx"></a>Alteryx

本文介绍如何将 [Alteryx](https://www.alteryx.com/) 与 Azure Databricks 配合使用。

## <a name="requirements"></a>要求

Alteryx 10.6 及更高版本。 要进行数据库内处理，需要具有 64 位数据库驱动程序的 64 位 Alteryx。

## <a name="step-1-get-azure-databricks-connection-information"></a>步骤 1：获取 Azure Databricks 连接信息

1. 获取[个人访问令牌](../../dev-tools/api/latest/authentication.md#token-management)。
2. 获取服务器[主机名、端口和 HTTP 路径](jdbc-odbc-bi.md#get-server-hostname-port-http-path-and-jdbc-url)。

## <a name="step-2-configure-the-simba-spark-odbc-driver"></a>步骤 2：配置 Simba Spark ODBC 驱动程序

1. 打开与驱动程序类型相对应的 ODBC 管理控制台。
2. 在“用户”选项卡中，单击“添加”。
3. 选择“Simba Spark ODBC 驱动程序”。
4. 输入以下内容：
   * 数据源名称：``Databricks``
   * 说明：（可选）
   * Spark 服务器类型：``SparkThriftServer``
   * 主机：步骤 2 中的主机。
   * 端口：步骤 2 中的端口。
   * 身份验证：``HTTP``
   * 机制：``Token``
   * 用户名：``token``
   * 密码：步骤 2 中的个人访问令牌。
5. 选择“高级选项”。
6. 选择“快速 SQLPrepare”。
7. 选择“使用查询获取表”。
8. 选择“显示系统表”。
9. 单击“测试”来测试连接设置。
10. 单击“确定”。

## <a name="step-3-configure-connection-in-alteryx-to-an-azure-databricks-cluster"></a>步骤 3：在 Alteryx 中配置到 Azure Databricks 群集的连接

1. 在 Alteryx 设计器中，转到“数据库内”工具选项卡。
2. 将“连接数据库内”工具拖到画布上。
3. 在配置面板中，单击“连接名称”下的下拉菜单。
4. 选择“管理连接…”
5. 输入用户的连接类型。
6. 在“连接”下，单击“新建”并输入以下内容：
   * **连接名称**：Databricks（或其他首选名称）
   * **密码加密**：为用户加密（或其他首选对象）
7. 单击“读取”选项卡，然后输入以下内容：
   1. **驱动程序**：Spark ODBC
   1. 单击“连接字符串”下的下拉菜单。
   1. 选择“新建数据库连接…”。
      1. 单击“Spark 数据源名称”下拉菜单，然后选择“Databricks (用户)” 。
      2. 单击“确定”。
8. 单击“写入”选项卡，然后输入以下内容：
   1. 驱动程序：**Databricks 大容量加载程序 (Avro) 或 (CSV)**
   1. 单击“连接字符串”下的下拉菜单。
   1. 选择“新建 Databricks 连接…”。
   1. 在“ODBC 数据源”下，选择“Databricks (用户)”。
      * 在“用户名”字段中，输入 `token`。
      * 在“密码”字段中，输入步骤 2 中的个人访问令牌。
      * 在“Databricks URL”中，输入 `https://` 和 步骤 2 中的主机。
9. 单击“确定”。