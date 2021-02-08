---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/03/2020
title: SQL Workbench/J - Azure Databricks
description: 了解如何将 SQL Workbench/J 与 Azure Databricks 配合使用。
ms.openlocfilehash: 8df998aee0c9d4271c5c9ef05d38db1b300a7c23
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058976"
---
# <a name="sql-workbenchj"></a><a id="sql-workbenchj"> </a><a id="workbenchj"> </a>SQL Workbench/J

本文介绍如何将 [SQL Workbench/J](https://www.sql-workbench.eu/) 与 Azure Databricks 配合使用。

## <a name="step-1-get-azure-databricks-connection-information"></a>步骤 1：获取 Azure Databricks 连接信息

1. 获取[个人访问令牌](../../dev-tools/api/latest/authentication.md#token-management)。
2. 获取服务器[主机名、端口和 HTTP 路径](jdbc-odbc-bi.md#get-server-hostname-port-http-path-and-jdbc-url)。

## <a name="step-2-configure-azure-databricks-cluster-connection-in-workbenchj"></a>步骤 2：在 Workbench/J 中配置 Azure Databricks 群集连接

1. 启动 SQL Workbench/J。
2. 选择“文件”>“连接”窗口。
3. 在“选择连接配置文件”对话框中，单击“管理驱动程序”。
   1. 在“名称”字段中，键入 ``Spark JDBC``。
   1. 在“库”字段中，单击“选择 JAR 文件”图标。 浏览到下载了 Simba Spark JDBC 驱动程序 JAR 的目录。
   1. 验证“Classname”字段是否已填充。

      > [!div class="mx-imgBorder"]
      > ![Spark JDBC 驱动程序](../../_static/images/third-party-integrations/workbenchj/workbenchj-driver.png)

   1. 单击“确定”。
4. 单击“新建连接配置文件”![“连接配置文件”图标](../../_static/images/third-party-integrations/workbenchj/workbenchj-new-connection-profile.png) 图标。
   1. 键入配置文件的名称。
   1. 在“驱动程序”字段中，选择“com.simba.spark.jdbc.Driver”。
   1. 在“URL”字段中，输入在步骤 1 中构造的 URL。
   1. 在“用户名”字段中，输入 ``token``。
   1. 在“密码”字段中，输入步骤 1 中的个人访问令牌。

      > [!div class="mx-imgBorder"]
      > ![连接配置文件](../../_static/images/third-party-integrations/workbenchj/workbenchj-profile.png)

   1. 单击“测试”。

      > [!div class="mx-imgBorder"]
      > ![连接测试](../../_static/images/third-party-integrations/workbenchj/workbenchj-test.png)

   1. 单击“确定”两次。