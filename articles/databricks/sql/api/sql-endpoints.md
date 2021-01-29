---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/13/2021
title: SQL 终结点 API - Azure Databricks
description: 了解 Databricks SQL 终结点 API。
ms.openlocfilehash: 267df629a4ffa6d0a74749f654359ab945c00c8a
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692411"
---
# <a name="sql-endpoint-apis"></a>SQL 终结点 API

> [!IMPORTANT]
>
> 此功能以[个人预览版](../../release-notes/release-types.md)提供。 若要试用，请与 Azure Databricks 联系人取得联系。

> [!IMPORTANT]
>
> 要访问 Databricks REST API，必须[进行身份验证](authentication.md)。

## <a name="requirements"></a>要求

* 若要创建 SQL 终结点，必须在 Azure Databricks 工作区中具有[群集创建权限](../../administration-guide/access-control/cluster-acl.md#cluster-create-permission)。
* 若要管理 SQL 终结点，必须在 Azure Databricks SQL Analytics 中具有该终结点的[可管理](../user/security/access-control/sql-endpoint-acl.md)权限。

## <a name="sql-endpoints-api"></a>SQL 终结点 API

### <a name="in-this-section"></a>本节内容：

* [创建](#create)
* [删除](#delete)
* [编辑](#edit)
* [Get](#get)
* [列表](#list)
* [启动](#start)
* [停止](#stop)

### <a name="create"></a>创建

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/endpoints/``                     | ``POST``        |

创建 SQL 终结点。

| 字段名称                        | 类型                                         | 说明                                                                                                                                                                                                                                                                                                                  |
|-----------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                              | ``STRING``                                   | SQL 终结点的名称。 必须是唯一的。 此字段为必需字段。                                                                                                                                                                                                                                                            |
| cluster_size                      | ``String``                                   | 分配给终结点的群集大小：``"2X-Small"``、``"X-Small"``、``"Small"``、``"Medium"``、``"Large"``、``"X-Large"``、``"2X-Large"``、``"3X-Large"``、``"4X-Large"``。 有关从群集到实例大小的映射，请参阅[群集大小](../admin/sql-endpoints.md#cluster-size)。 此字段为必需字段。 |
| min_num_clusters                  | ``INT32``                                    | SQL 终结点运行时可用的最小群集数。 默认值为 1。                                                                                                                                                                                                                                       |
| max_num_clusters                  | ``INT32``                                    | SQL 终结点运行时可用的最大群集数。 此字段为必需字段。 如果[未启用多群集负载均衡](../admin/sql-endpoints.md#edit-endpoint)，则限制为 ``1``。                                                                                                            |
| auto_stop_mins                    | ``INT32``                                    | 在空闲的 SQL 终结点终止所有群集并停止之前等待的时间（以分钟为单位）。 此字段是可选的。 默认值为 0，表示禁用自动停止。                                                                                                                                                                   |
| 标记                              | 一组 [EndpointTagPair](#endpointtagpair) | 一个对象，其中包含终结点资源的一组标记。 Azure Databricks 使用这些标记来标记所有终结点资源。 此字段是可选的。                                                                                                                                                                             |
| enable_photon                     | ``BOOLEAN``                                  | 是否启用 Photon。 此字段是可选的。                                                                                                                                                                                                                                                                            |

#### <a name="example-request"></a>示例请求

```json
{
  "name": "My SQL Endpoint",
  "cluster_size": "Medium",
  "min_num_clusters": 1,
  "max_num_clusters": 10,
  "enable_photon": "true"
}
```

#### <a name="example-response"></a>示例响应

```json
{
  "id": "0123456789abcdef"
}
```

### <a name="delete"></a>删除

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/endpoints/{id}``                 | ``DELETE``      |

删除 SQL 终结点。

### <a name="edit"></a>编辑

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/endpoints/{id}/edit``            | ``POST``        |

修改 SQL 终结点。 所有字段都是可选的。 缺失字段默认为当前值。

| 字段名称                        | 类型                                         | 说明                                                                                                                                                                                                                                                                                          |
|-----------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                                | ``STRING``                                   | SQL 终结点的 ID。                                                                                                                                                                                                                                                                              |
| name                              | ``STRING``                                   | SQL 终结点的名称。                                                                                                                                                                                                                                                                            |
| cluster_size                      | ``String``                                   | 分配给终结点的群集大小：``"2X-Small"``、``"X-Small"``、``"Small"``、``"Medium"``、``"Large"``、``"X-Large"``、``"2X-Large"``、``"3X-Large"``、``"4X-Large"``。 有关从群集到实例大小的映射，请参阅[群集大小](../admin/sql-endpoints.md#cluster-size)。 |
| min_num_clusters                  | ``INT32``                                    | SQL 终结点运行时可用的最小群集数。                                                                                                                                                                                                                                 |
| max_num_clusters                  | ``INT32``                                    | SQL 终结点运行时可用的最大群集数。 此字段为必需字段。 如果[未启用多群集负载均衡](../admin/sql-endpoints.md#edit-endpoint)，则限制为 ``1``。                                                                                            |
| auto_stop_mins                    | ``INT32``                                    | 在空闲的 SQL 终结点终止所有群集并停止之前等待的时间（以分钟为单位）。                                                                                                                                                                                                                        |
| 标记                              | 一组 [EndpointTagPair](#endpointtagpair) | 一个对象，其中包含终结点资源的一组标记。 Azure Databricks 使用这些标记来标记所有终结点资源。                                                                                                                                                                             |
| enable_photon                     | ``BOOLEAN``                                  | 是否启用 Photon。                                                                                                                                                                                                                                                                            |

#### <a name="example-request"></a>示例请求

```json
{
  "name": "My Edited SQL endpoint",
  "cluster_size": "Large",
  "auto_stop_mins": 60
}
```

### <a name="get"></a>获取

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/endpoints/{id}``                 | ``GET``         |

检索 SQL 终结点的信息。

| 字段名称                        | 类型                                         | 说明                                                                                                                                                                                                                                                                                          |
|-----------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                                | ``STRING``                                   | SQL 终结点 ID。                                                                                                                                                                                                                                                                                     |
| name                              | ``STRING``                                   | SQL 终结点的名称。                                                                                                                                                                                                                                                                            |
| cluster_size                      | ``String``                                   | 分配给终结点的群集大小：``"2X-Small"``、``"X-Small"``、``"Small"``、``"Medium"``、``"Large"``、``"X-Large"``、``"2X-Large"``、``"3X-Large"``、``"4X-Large"``。 有关从群集到实例大小的映射，请参阅[群集大小](../admin/sql-endpoints.md#cluster-size)。 |
| auto_stop_mins                    | ``INT32``                                    | 在空闲的 SQL 终结点终止所有群集并停止之前等待的时间。                                                                                                                                                                                                                                   |
| num_clusters                      | ``INT32``                                    | 分配给终结点的群集数。                                                                                                                                                                                                                                                        |
| min_num_clusters                  | ``INT32``                                    | SQL 终结点运行时可用的最小群集数。                                                                                                                                                                                                                                 |
| max_num_clusters                  | ``INT32``                                    | SQL 终结点运行时可用的最大群集数。                                                                                                                                                                                                                                 |
| num_active_sessions               | ``INT32``                                    | SQL 终结点上运行的 JDBC 和 ODBC 活动会话的数目。                                                                                                                                                                                                                                 |
| state                             | [EndpointState](#endpointstate)              | SQL 终结点的状态。                                                                                                                                                                                                                                                                           |
| creator_name                      | ``STRING``                                   | 创建终结点的用户的电子邮件地址。                                                                                                                                                                                                                                                 |
| creator_id                        | ``STRING``                                   | 创建终结点的用户的 Azure Databricks ID。                                                                                                                                                                                                                                           |
| jdbc_url                          | ``STRING``                                   | 用于将 SQL 命令提交到使用 JDBC 的 SQL 终结点的 URL。                                                                                                                                                                                                                                  |
| odbc_params                       | [ODBCParams](#odbcparams)                    | 使用 ODBC 向 SQL 终结点提交 SQL 命令所需的主机、路径、协议和端口信息。                                                                                                                                                                                       |
| 标记                              | 一组 [EndpointTagPair](#endpointtagpair) | 一个对象，其中包含终结点资源的一组标记。 Azure Databricks 使用这些标记来标记所有终结点资源。                                                                                                                                                                             |
| health                            | [EndpointHealth](#endpointhealth)            | 终结点的运行状况。                                                                                                                                                                                                                                                                          |
| enable_photon                     | ``Boolean``                                  | 是否启用 Photon。                                                                                                                                                                                                                                                                           |

#### <a name="example-response"></a>示例响应

```json
{
  "id": "123456790abcdef",
  "name": "My SQL endpoint",
  "cluster_size": "Medium",
  "min_num_clusters": 1,
  "max_num_clusters": 10,
  "auto_stop_mins": 30,
  "num_clusters": 5,
  "num_active_sessions": 30,
  "state": "RUNNING",
  "creator_name": "user@example.com",
  "jdbc_url":"jdbc:spark://<databricks-instance>:443/default;transportMode=http;ssl=1;AuthMech=3;httpPath=/sql/protocolv1/o/0123456790abcdef;",
  "odbc_params": {
    "host": "<databricks-instance>",
    "path": "/sql/protocolv1/o/0/123456790abcdef",
    "protocol": "https",
    "port": 443
  }
}
```

### <a name="list"></a>列表

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/endpoints/``                     | ``GET``         |

列出工作区中的所有 SQL 终结点。

#### <a name="example-response"></a>示例响应

```json
{
  "endpoints": [
    { "id": "123456790abcdef", "name": "My SQL endpoint", "cluster_size": "Medium", },
    { "id": "098765321fedcba", "name": "Another SQL endpoint", "cluster_size": "Large", }
  ]
}
```

### <a name="start"></a>开始

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/endpoints/{id}/start``           | ``POST``        |

启动 SQL 终结点。

### <a name="stop"></a>停止

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``2.0/sql/endpoints/{id}/stop``            | ``POST``        |

停止 SQL 终结点。

## <a name="configure-sql-endpoints-api"></a>配置 SQL 终结点 API

可为所有 SQL 终结点配置安全策略和数据访问。

### <a name="in-this-section"></a>本节内容：

* [Get](#get)
* [编辑](#edit)

### <a name="get"></a>获取

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``/2.0/sql/config/endpoints``              | ``GET``         |

获取所有 SQL 终结点的配置。

| 字段名称                        | 类型                                              | 说明                                                                                                  |
|-----------------------------------|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| security_policy                   | [EndpointSecurityPolicy](#endpointsecuritypolicy) | 用于控制对数据集的访问的策略。                                                               |
| data_access_config                | [EndpointConfPair](#endpointconfpair)             | 一个对象，其中包含一组含有外部 Hive 元存储属性的可选键/值对。 |

#### <a name="example-response"></a>示例响应

```json
{
  "security_policy": "DATA_ACCESS_CONTROL",
  "data_access_config":{}
}
```

### <a name="edit"></a>编辑

编辑所有 SQL 终结点的配置。

> [!IMPORTANT]
>
> * 所有字段都是必填字段。
> * 调用此方法可重启所有正在运行的 SQL 终结点。

| 端点                                   | HTTP 方法     |
|--------------------------------------------|-----------------|
| ``/2.0/sql/config/endpoints``              | ``PUT``         |

| 字段名称                        | 类型                                              | 说明                                                                                                  |
|-----------------------------------|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| security_policy                   | [EndpointSecurityPolicy](#endpointsecuritypolicy) | 用于控制对数据集的访问的策略。                                                               |
| data_access_config                | [EndpointConfPair](#endpointconfpair)             | 一个对象，其中包含一组含有外部 Hive 元存储属性的可选键/值对。 |

#### <a name="example-request"></a>示例请求

```json
{
  "data_access_config":{}
}
```

## <a name="data-structures"></a>数据结构

### <a name="in-this-section"></a>本节内容：

* [EndpointConfPair](#endpointconfpair)
* [EndpointHealth](#endpointhealth)
* [EndpointSecurityPolicy](#endpointsecuritypolicy)
* [EndpointState](#endpointstate)
* [EndpointStatus](#endpointstatus)
* [EndpointTagPair](#endpointtagpair)
* [ODBCParams](#odbcparams)

### <a name="endpointconfpair"></a>EndpointConfPair

| 字段名称                        | 类型                              | 描述                       |
|-----------------------------------|-----------------------------------|-----------------------------------|
| key                               | ``STRING``                        | 配置键名称。           |
| 值                             | ``STRING``                        | 配置键值。          |

### <a name="endpointhealth"></a>EndpointHealth

| 字段名称                        | 类型                              | 说明                                                                                                             |
|-----------------------------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| status                            | [EndpointStatus](#endpointstatus) | 终结点状态。                                                                                                        |
| message                           | ``STRING``                        | 有关运行状况的描述性消息。 包含有关导致当前运行状况的错误的信息。 |

### <a name="endpointsecuritypolicy"></a>EndpointSecurityPolicy

| 选项                     | 说明                                                                                               |
|----------------------------|-----------------------------------------------------------------------------------------------------------|
| ``DATA_ACCESS_CONTROL``    | 使用[数据访问控制](../user/security/access-control/data-acl.md)来控制对数据集的访问。     |

### <a name="endpointstate"></a>EndpointState

SQL 终结点的状态。 可允许的状态转换如下：

* ``STARTING`` -> ``STARTING``, ``RUNNING``, ``STOPPING``, ``DELETING``
* ``RUNNING`` -> ``STOPPING``, ``DELETING``
* ``STOPPING`` -> ``STOPPED``, ``STARTING``
* ``STOPPED`` -> ``STARTING``, ``DELETING``
* ``DELETING`` -> ``DELETED``

| 状态             | 说明                                                                              |
|-------------------|------------------------------------------------------------------------------------------|
| ``STARTING``      | 正在启动终结点。                                              |
| ``RUNNING``       | 启动过程已完成，终结点已可供使用。                           |
| ``STOPPING``      | 正在停止终结点。                                         |
| ``STOPPED``       | 终结点已停止。 首先调用 start 或者提交 JDBC 或 ODBC 请求。 |
| ``DELETING``      | 正在销毁终结点。                                       |
| ``DELETED``       | 终结点已被删除并且无法恢复。                                   |

### <a name="endpointstatus"></a>EndpointStatus

| 状态                  | 说明                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------|
| ``HEALTHY``            | 终结点正常运行，无已知问题。                               |
| ``DEGRADED``           | 终结点可能正常运行，但存在一些已知问题。 性能可能会受到影响。 |
| ``FAILED``             | 终结点受到严重影响，无法为查询提供服务。                          |

### <a name="endpointtagpair"></a>EndpointTagPair

| 字段名称                        | 类型                              | 描述                       |
|-----------------------------------|-----------------------------------|-----------------------------------|
| key                               | ``STRING``                        | 标记键名称。                     |
| 值                             | ``STRING``                        | 标记键值。                    |

### <a name="odbcparams"></a>ODBCParams

| 字段名称                        | 类型                              | 说明                       |
|-----------------------------------|-----------------------------------|-----------------------------------|
| host                              | ``STRING``                        | ODBC 服务器主机名。             |
| path                              | ``STRING``                        | ODBC 服务器路径。                 |
| protocol                          | ``STRING``                        | ODBC 服务器协议。             |
| port                              | ``INT32``                         | ODBC 服务器端口                  |