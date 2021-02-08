---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/03/2020
title: TIBCO Spotfire Analyst - Azure Databricks
description: 了解如何将 TIBCO Spotfire Analyst 与 Azure Databricks 配合使用。
ms.openlocfilehash: f4f4e4bed06dc8f051f427fe6160c5d0b40c895d
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061199"
---
# <a name="tibco-spotfire-analyst"></a>TIBCO Spotfire Analyst

本文介绍如何将 [TIBCO Spotfire Analyst](https://www.tibco.com/products/tibco-spotfire) 与 Azure Databricks 配合使用。

## <a name="step-1-get-azure-databricks-connection-information"></a>步骤 1：获取 Azure Databricks 连接信息

1. 获取[个人访问令牌](../../dev-tools/api/latest/authentication.md#token-management)。
2. 获取服务器[主机名、端口和 HTTP 路径](jdbc-odbc-bi.md#get-server-hostname-port-http-path-and-jdbc-url)。

## <a name="step-2-configure-azure-databricks-cluster-connection-in-tibco-spotfire"></a>步骤 2：在 TIBCO Spotfire 中配置 Azure Databricks 群集连接

若要将 TIBCO Spotfire Analyst 连接到 Azure Databricks 群集，请使用 TIBCO Spotfire 的本机 Apache Spark SQL 连接器。

1. 在 TIBCO Spotfire Analyst 中，打开“文件和数据”浮出控件，然后单击“连接到” 。

   > [!div class="mx-imgBorder"]
   > ![文件和数据](../../_static/images/third-party-integrations/tibco/tibco-image1.png)

2. 选择“Apache Spark SQL”，然后单击“新建连接” 。

   > [!div class="mx-imgBorder"]
   > ![Apache Spark SQL 连接](../../_static/images/third-party-integrations/tibco/tibco-image2.png)

   1. 输入群集的服务器主机名和端口，以冒号分隔。
   1. 选择身份验证方法“用户名”和“密码” 。
   1. 输入 ``token`` 为“用户名”，输入你的 Databricks 个人访问令牌值为“密码” 。
   1. 为群集选择正确的“SSL 选项”。
   1. 在“高级”选项卡上，选择 Thrift 传输模式“HTTP”，然后输入群集的“HTTP 路径”  。
   1. 单击“连接”  。
   1. 使用下拉菜单选择数据库，然后单击“确定”。

## <a name="step-3-select-the-azure-databricks-data-to-analyze"></a>步骤 3：选择要分析的 Azure Databricks 数据

在“连接中的视图”窗格中选择数据。

> [!div class="mx-imgBorder"]
> ![可用表](../../_static/images/third-party-integrations/tibco/tibco-image3.png)

1. 浏览群集中的可用表。
2. 将所需的表添加为视图，这些表将是在 Spotfire 中分析的数据表。
3. 对于每个视图，可以决定要包含哪些列。 如果要创建非常具体且灵活的数据选择，则可以访问此对话框中的一系列强大工具，例如：
   * 自定义查询。 使用自定义查询，可以通过键入自定义 SQL 查询来选择要分析的数据。
   * 提示。 将数据选择留给分析文件的用户。 基于选择的列配置提示。 然后，打开分析的最终用户可以选择限制和查看仅相关值的数据。 例如，用户可以选择特定时间范围内或特定地理区域内的数据。
4. 单击“确定”。

## <a name="step-4-push-down-queries-to-azure-databricks-or-import-data"></a>步骤 4：将查询下推到 Azure Databricks 或导入数据

选择要分析的数据后，最后一步是选择要如何从 Azure Databricks 群集检索数据。 将显示要添加到分析中的数据表的摘要，可以单击每个表以更改数据加载方法。

> [!div class="mx-imgBorder"]
> ![顺序表示例](../../_static/images/third-party-integrations/tibco/tibco-image4.png)

Azure Databricks 的默认选项是“外部”。 这意味着数据表将保留在 Azure Databricks 中的数据库中，并且 Spotfire 将基于你在分析中的操作将不同的查询推送到数据库，以获取相关的数据切片。

还可以选择“导入”，Spotfire 将预先提取整个数据表，从而可以进行本地内存中分析。 导入数据表时，还可以在 Spotfire 的嵌入式内存中数据引擎中使用分析功能

第三个选项是“按需”（对应于动态 ``WHERE`` 子句），这意味着将基于分析中的用户操作提取数据切片。 可以定义条件，这些条件可以是诸如标记或筛选数据或更改文档属性之类的操作。 按需数据加载也可以与“外部”数据表结合使用。