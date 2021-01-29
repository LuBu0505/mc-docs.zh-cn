---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: MongoDB - Azure Databricks
description: 了解 Azure Databricks SQL Analytics 数据源及其管理方式。
ms.openlocfilehash: 5079833ecd5518c9d1097263849592816e8b9640
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692206"
---
# <a name="mongodb"></a>MongoDB

若要设置 MongoDB 连接，请提供连接字符串和数据库名称 。

**连接字符串**

* 最简单的情况：``mongodb://username:password@hostname:port/dbname``
* 启用了 SSL 的情况：``mongodb://username:password@hostname:port/dbname?ssl=true``
* 启用了 SSL 且具有自签名证书（禁用证书验证）的情况：``mongodb://username:password@hostname:port/dbname?ssl=true&ssl_cert_reqs=CERT_NONE``

如果需要，可在查询字符串中传入其他连接选项。 请在[连接选项](https://docs.mongodb.com/manual/reference/connection-string /#connection-options)中查看完整详细信息。

你可能注意到了数据源配置中有一个单独的数据库名称字段，我们也会将它包含在连接字符串中。 通常，需要在 MLab 等共享主机上这样做。

## <a name="mongodb-atlas"></a>MongoDB Atlas

免费层帐户位于连接到 MongoDB Atlas 的共享环境，因此可能会出现问题。 为了获得最佳结果，请使用以下格式的连接字符串：

``mongodb+srv://<user>:<password>@<subdomain>.mongodb.net/<database>?retryWrites=true``

## <a name="troubleshooting"></a>疑难解答

**错误：“SSL 握手失败: [SSL:CERTIFICATE_VERIFY_FAILED] 证书验证失败”**

MongoDB 服务器使用自签名证书时，通常会发生这种情况。 你可切换为正确签名的证书，也可直接将 ``ssl_cert_reqs=CERT_NONE`` 选项添加到连接字符串。

将 MongoDB 查询编写为 JSON 对象。 在执行期间，SQL Analytics 会将其转换为 [`db.collection.find()`](https://docs.mongodb.com/manual/reference/method/db.collection.find/) 调用或 [`db.collection.aggregate()`](https://docs.mongodb.com/manual/reference/method/db.collection.aggregate/) 调用。 映射 JSON 对象并将其发送到 MongoDB 的方法如下：

| MongoDB 令牌                    | SQL Analytics 中的写入位置                         |
|----------------------------------|---------------------------------------------------------|
| ``db``                           | 在数据源设置屏幕上                         |
| ``collection``                   | 在查询对象中添加 ``collection`` 键           |
| ``query``                        | 在查询对象中添加 ``query`` 键                |
| ``projection``                   | 在查询对象中添加 ``fields`` 键               |
| ``.sort() method``               | 在查询对象中添加 ``sort`` 键                 |
| ``.skip() method``               | 在查询对象中添加 ``skip`` 键                 |
| ``.limit() method``              | 在查询对象中添加 ``limit`` 键                |
| ``db.collection.count()`` 方法 | 在查询对象中使用具有任意值的 ``count`` 键 |

将用于每个键的值作为参数按原样传递到 MongoDB。

## <a name="query-examples"></a>查询示例

### <a name="simple-query-example"></a>简单查询示例

```json
{
  "collection": "my_collection",
  "query": {
    "type": 1
  },
  "fields": {
    "_id": 1,
    "name": 2
  },
  "sort": [{
    "name": "date",
    "direction": -1
  }]
}
```

Javascript 中的等效查询将编写为：``db.my_collection.find({"type": 1}, {"_id": 1, "name": 2}).sort([{"name": "date","direction": -1}])``

### <a name="count-query-example"></a>计数查询示例

```json
{
  "collection": "my_collection",
  "count": true
}
```

### <a name="aggregation"></a>聚合

聚合使用的语法与 PyMongo 中使用的类似。 但为了支持正确排序，它在“$sort”操作时使用常规列表，该列表会在执行前转换为 SON（已排序的字典）对象。

聚合查询示例：

```json
{
  "collection": "things",
  "aggregate": [{
    "$unwind": "$tags"
  }, {
    "$group": {
      "_id": "$tags",
      "count": {
        "$sum": 1
      }
    }
  }, {
    "$sort": [{
      "name": "count",
      "direction": -1
    }, {
      "name": "_id",
      "direction": -1
    }]
  }]
}
```

### <a name="mongodb-extended-json-support"></a>MongoDB 扩展 JSON 支持

除了我们自己的扩展 ``$humanTime``，我们还支持 [MongoDB 扩展 JSON](https://docs.mongodb.com/manual/reference/mongodb-extended-json/)：

```json
{
  "collection": "date_test",
  "query": {
    "lastModified": {
      "$gt": {
        "$humanTime": "3 years ago"
      }
    }
  },
  "limit": 100
}
```

它接受与上述类似的可读字符串（“3 years ago”、“yesterday”等）或时间戳。

> [!NOTE]
>
> 由于 SQL Analytics 使用的格式与 MongoDB 所需的格式之间存在差异，因此在 MongoDB 中使用日期或日期/时间类型的[查询参数](../../user/queries/query-parameters.md)时，还需要 ``$humanTime`` 函数。
>
> 使用日期（或日期范围）参数时，请使用 ``$humanTime`` 对象对其进行包装：``{{param}}`` 会变为 ``{"$humanTime": "{{param}} 00:00"}``（使用日期参数时才需要后缀 `` 00:00``；如果使用日期时间参数，则应忽略该后缀）。

### <a name="mongodb-filtering"></a>MongoDB 筛选

可通过将添加了“::filter”关键字的列投影到末尾，向 Mongo 查询添加筛选器。

```json
{
  "collection": "zipcodes",
  "aggregate": [{
    "$project": {
      "_id": "$_id",
      "city": "$city",
      "loc": "$loc",
      "pop": "$pop",
      "state::filter": "$state"
    }
  }]
}
```

上例将显示一个“State”列，并允许对此列进行筛选。

## <a name="troubleshooting"></a>疑难解答

### <a name="sort-exceeded-memory-limit-of-104857600-bytes"></a>排序超出了 104857600 字节的内存限制

排序超出了 104857600 字节的内存限制，但未选择进行外部排序。 中止操作。 传递 ``allowDiskUse:true`` 以选择加入。

在 MongoDB 中，内存中排序限制为 100 M；若要执行大型排序，需启用 ``allowDiskUse`` 选项来将数据写入临时文件进行排序。

只需将选项添加到查询中即可启用 ``allowDiskUse`` 选项：

```json
{
  "allowDiskUse": true
}
```