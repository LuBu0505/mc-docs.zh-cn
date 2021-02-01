---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/19/2021
title: 商业智能工具 - Azure Databricks
description: 了解如何将 BI 工具连接到 Azure Databricks 群集。
ms.openlocfilehash: 478909e4483d2d351c6a9176827265f4a05e8b38
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058986"
---
# <a name="business-intelligence-tools"></a><a id="bi"> </a><a id="business-intelligence-tools"> </a>商业智能工具

可使用本机连接器或 JDBC/ODBC 驱动程序将你最喜爱的商业智能 (BI) 工具与 Azure Databricks 群集和 SQL 终结点相连接，访问 Azure Databricks 表中的数据。 以下 BI 工具与 Azure Databricks 集成：

| 产品                                                                                                                         | 要求和建议                                                                                                                                                                                       | 文档                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [![Power BI 徽标](../../_static/images/third-party-integrations/power-bi/logo-ms-power-bi.png)](https://powerbi.microsoft.com/) | * Power BI Desktop<br>  * 建议使用 2.85.681.0 或更高版本：无要求<br>  * 低于 2.85.681.0：[Databricks ODBC 驱动程序](https://databricks.com/spark/odbc-driver-download)<br>* Power BI 服务：无要求 | * Azure Databricks [Power BI 集成](power-bi.md)                                                                                                                                                                                           |
| [![Tableau 徽标](../../_static/images/third-party-integrations/tableau/logo-tableau.png)](https://www.tableau.com/)             | * Tableau Desktop 2019.3 或更高版本：[Databricks ODBC 驱动程序](https://databricks.com/spark/odbc-driver-download)<br>* Tableau Online：无要求                                                                    | * Azure Databricks [Tableau 集成](tableau.md)<br>* Tableau [Databricks 连接器](https://help.tableau.com/current/pro/desktop/en-us/examples_databricks.htm)                                                                             |
| [![Qlik 徽标](../../_static/images/third-party-integrations/qlik/qlik-logo.png)](https://www.qlik.com/)                         | 无要求                                                                                                                                                                                                        | * Databricks [Qlik 集成](../ingestion/qlik.md)<br>* Qlik [创建 Apache Spark 连接](https://help.qlik.com/en-US/connectors/Subsystems/ODBC_connector_help/Content/Connectors_ODBC/ApacheSPARK/Create-ApacheSPARK-connection.htm) |
| [![Looker 徽标](../../_static/images/third-party-integrations/looker/logo-looker.png)](https://looker.com/)                     | 无要求                                                                                                                                                                                                        | * Azure Databricks [Looker 集成](looker.md)                                                                                                                                                                                               |
| [![Spotfire 徽标](../../_static/images/third-party-integrations/tibco/logo-tibco.png)](https://www.tibco.com/)                  | [Databricks ODBC 驱动程序](https://databricks.com/spark/odbc-driver-download)                                                                                                                                            | * Azure Databricks [Spotfire Analyst 集成](spotfire.md)<br>* TIBCO [连接到 Databricks](https://docs.tibco.com/pub/spotfire/general/drivers/data_sources/connector_apache_spark_sql.htm#Connecting_to_Databricks_Cloud)             |
| [SQL Workbench/J](https://www.sql-workbench.eu/)                                                                                | [Databricks JDBC 驱动程序](https://databricks.com/spark/odbc-driver-download)                                                                                                                                            | * Azure Databricks [SQL Workbench/J 集成](workbenchj.md)<br>* SQL Workbench/J [入门](https://www.sql-workbench.eu/getting-started.html)                                                                                        |