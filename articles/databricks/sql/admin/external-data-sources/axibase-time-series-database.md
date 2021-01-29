---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: Axibase 时序数据库 - Azure Databricks
description: 了解 Azure Databricks SQL Analytics Axibase 时序数据库外部数据源及其管理方式。
ms.openlocfilehash: b8a8b240ee38b081baf078f2c266a1350dc2da45
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692216"
---
# <a name="axibase-time-series-database"></a>Axibase 时序数据库

## <a name="create-user-group-with-read-only-permission"></a>创建具有只读权限的用户组

1. 在 [https://atsd_host:8443](https://atsd_host:8443/) 登录到 PTSD Web 界面。
2. 打开“管理”>“用户”组页面，再单击“创建” 。
3. 指定用户组名称和可选说明。
4. 从“所有实体”向组授予读取的权限 。
5. 单击“保存”按钮  。

   > [!div class="mx-imgBorder"]
   > ![分配权限](../../../_static/images/sql/atsd-user-group.png)

## <a name="create-user"></a>创建用户

1. 打开“管理”>“用户”页面，再单击“创建” 。
2. 如果需要，请指定用户名、密码和其他字段。
3. 在“实体权限”部分，将用户作为成员添加到之前创建的用户组中。
4. 单击“保存”按钮  。

   > [!div class="mx-imgBorder"]
   > ![创建用户](../../../_static/images/sql/atsd-user.png)

## <a name="create-data-source"></a>创建数据源

1. 登录 SQL Analytics Web 界面。
2. 打开“新建数据源”页面，选择“Axibase 时序数据库”作为类型。
3. 在配置表单上完成以下字段：

   | **名称**                       | **默认值** | **必需** | **说明**                                                                                                                                                                                                                  |
   |--------------------------------|-------------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | **用户名**                   | —                 | 是          | 用户名。                                                                                                                                                                                                                       |
   | **密码**                   | —                 | 是          | 用户密码。                                                                                                                                                                                                                   |
   | **指标限制**               | 5000              | 否           | 在 SQL Analytics 中以表格形式显示的 ATSD [指标](https://github.com/axibase/atsd-docs/blob/master/api/meta/metric/list.md#query-parameters)最大数量。                                                                |
   | **指标筛选器**              | —                 | 否           | 包含与[表达式筛选器](https://github.com/axibase/atsd-docs/blob/master/api/meta/metric/list.md#query-parameters)匹配的指标。                                                                                    |
   | **指标最小插入日期** | —                 | 否           | 包含等于或大于指定日期的[最后插入日期](https://github.com/axibase/atsd-docs/blob/master/api/meta/metric/list.md#query-parameters)的指标。 支持 ISO 日期格式和 endtime 语法。 |
   | 协议                   | http              | 是          | 连接协议。                                                                                                                                                                                                             |
   | **信任 SSL 证书**      | False             | 否           | 信任 SSL 证书（如果是自签名证书）。                                                                                                                                                                        |
   | **主机**                       | localhost         | 否           | ATSD 主机名或 IP 地址。                                                                                                                                                                                                     |
   | 端口                       | 8088              | 否           | ATSD http (8088) 或 https (8443) 端口。                                                                                                                                                                                           |
   | **连接超时值**         | 600               | 否           | 连接超时（秒）。                                                                                                                                                                                                   |

   > [!div class="mx-imgBorder"]
   > ![Axibase 数据源](../../../_static/images/sql/atsd-datasource.png)

4. 保存数据源。
5. 单击“测试”按钮来测试连接。

## <a name="run-a-query"></a>运行查询

测试成功后，可查询存储在 Axibase 时序数据库中的数据。

> [!div class="mx-imgBorder"]
> ![运行 ATSD 查询](../../../_static/images/sql/atsd-query.png)