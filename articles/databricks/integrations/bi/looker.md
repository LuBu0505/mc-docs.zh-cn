---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/03/2020
title: Looker - Azure Databricks
description: 了解如何将 Looker 与 Azure Databricks 配合使用。
ms.openlocfilehash: d80c54aaeca7839fd127b112236171b610279f3f
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058981"
---
# <a name="looker"></a>Looker

本文介绍如何将 [Looker](https://looker.com/) 与 Azure Databricks 配合使用。

## <a name="step-1-get-azure-databricks-connection-information"></a>步骤 1：获取 Azure Databricks 连接信息

1. 获取[个人访问令牌](../../dev-tools/api/latest/authentication.md#token-management)。
2. 获取服务器[主机名、端口和 HTTP 路径](jdbc-odbc-bi.md#get-server-hostname-port-http-path-and-jdbc-url)。

## <a name="step-2-configure--azure-databricks-cluster-connection-in-looker"></a>步骤 2：在 Looker 中配置 Azure Databricks 群集连接

1. 在 Looker 中，转到“管理员”>“连接”>“新数据库连接”。

   > [!div class="mx-imgBorder"]
   > ![群集连接参数](../../_static/images/third-party-integrations/looker/looker-spark-2-x.png)

2. 输入名称和 Apache Spark 方言。
3. 在“主机”和“端口”字段中，输入在步骤 1 中检索到的信息。
4. 在“用户名”字段中输入 ``token``，在“密码”字段中输入步骤 2 中的令牌。
5. 如果要将查询转换为其他时区，请调整“查询时区”。
6. 将其他参数设置为 ``looker_db;AuthMech=3;transportMode=http;ssl=1;httpPath=``，并追加步骤 1 中的 HTTP 路径。
7. 对于剩余字段，保留默认值：
   * 不要启用持久性派生表。
   * 保持“最大连接”和“连接池超时”默认值。
   * 将数据库时区留空（假设将所有内容存储在 UTC 中）。

有关详细信息，请参阅 [Looker 文档](https://docs.looker.com/)。

## <a name="step-3-begin-modeling-your-database-in-looker-by-creating-a-project-and-running-the-generator"></a>步骤 3：通过创建项目和运行生成器，开始在 Looker 中构建数据库模型

此步骤假定群集的默认数据库中存储了永久表。

1. 如有必要，可通过将“开发”按钮从“关闭”切换到“打开”来应用开发人员模式  。
2. 请转到“LookML”>“管理项目”。
3. 单击“新 LookML 项目”。
4. 配置新项目。
   * 为项目指定名称。
   * 选择“生成模型和视图”。
   * 选择创建数据库连接时提供的连接名称。
   * 选择“所有表”。
   * 将“架构”设置为“默认”，除非群集中有要建模的其他数据库 。
5. 单击“创建项目”。

创建项目和生成器运行后，Looker 将显示一个包含一个模型文件和多个视图文件的用户界面。 模型文件显示架构中的表及其之间任何已发现的联接关系，视图文件列出架构中每个表可用的每个维度（列）。