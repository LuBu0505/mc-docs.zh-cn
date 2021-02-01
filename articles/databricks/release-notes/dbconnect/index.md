---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 01/20/2021
title: Databricks Connect 发行说明 - Azure Databricks
description: Databricks Connect 版本和维护更新的发行说明
ms.openlocfilehash: 3abb84804639417cab804532adc7625f7d698976
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99061197"
---
# <a name="databricks-connect-release-notes"></a>Databricks Connect 发行说明

此页列出了为 [Databricks Connect](../../dev-tools/databricks-connect.md)的版本和维护更新。

Databricks Connect 的主要和次要包版本必须始终与 Databricks Runtime 版本匹配。 Databricks 建议始终使用与 Databricks Runtime 版本相匹配的 Databricks Connect 的最新补丁版本。 例如，使用 Databricks Runtime 7.3 群集时，请使用最新的 ``databricks-connect==7.3.*`` 包。

## <a name="databricks-connect-for-databricks-runtime-73-lts"></a>适用于 Databricks Runtime 7.3 LTS 的 Databricks Connect

### <a name="databricks-connect-737"></a>Databricks Connect 7.3.7

**2021 年 1 月 20 日**

* Databricks Connect 客户端更新以支持 Databricks Runtime 7.3 2021 年 1 月 20 日维护版本

### <a name="databricks-connect-736"></a>Databricks Connect 7.3.6

**2021 年 1 月 12 日**

* [ES-59054] 通过 Databricks Connect 修复 Delta 优化表命令
* [ES-53497] 修复 Databricks 容器服务 (DCS) 上的 Databricks Connect

### <a name="databricks-connect-735"></a>Databricks Connect 7.3.5

**2020年 12 月 9 日**

Databricks Connect 7.3 现已正式发布。

* [ES-49865] 防止在将 Databricks Connect 用于 SBT 时由于依赖库自动同步而发生 500 内部服务器错误。

### <a name="databricks-connect-734-beta"></a>Databricks Connect 7.3.4 Beta 版本

**2020 年 11 月 24 日**

* [ES-44382] 将 ``spark.databricks.service.allowTerminatedClusterStart=false`` 设置为阻止 Databricks Connect 启动已终止的 Databricks Runtime 群集。
* 修复了在禁用 ``INFO`` 日志记录级别的情况下客户端无法与服务器正确同步状态的错误。

### <a name="databricks-connect-733-beta"></a>Databricks Connect 7.3.3 Beta 版本

**2020 年 11 月 3 日**

适用于 Databricks Runtime 7.3 的初始 Databricks Connect 版本。 此版本包括：

* 支持 Azure Active Directory 凭据直通。 通过 Databricks Connect，现在可以对标准群集使用 Azure Active Directory 凭据直通。 这样就可以通过 Databricks Connect 使用对 Azure Databricks 进行身份验证所用的 Azure Active Directory 标识对 [Azure Data Lake Storage Gen1](../../data/data-sources/azure/azure-datalake.md#adls-gen1) 和 [Azure Data Lake Storage Gen2](../../data/data-sources/azure/azure-datalake-gen2.md#adls-gen2) 自动进行身份验证。

* 支持 Delta Lake 按时间顺序查看。
* 使用 DBUtils 时，在 Databricks Connect 客户端与 Databricks Runtime 作业或笔记本之间轻松转换。 请参阅[访问 DBUtils](../../dev-tools/databricks-connect.md#access-dbutils)。

#### <a name="known-issues"></a>已知问题：

Databricks Runtime 版本验证在此 beta 版本中被禁用。 请确保将此客户端连接到 7.3 Databricks Runtime 群集。

## <a name="databricks-connect-for-databricks-runtime-71"></a>适用于 Databricks Runtime 7.1 的 Databricks Connect

### <a name="databricks-connect-7113"></a>Databricks Connect 7.1.13

**2021 年 1 月 20 日**

* Databricks Connect 客户端更新以支持 Databricks Runtime 7.1 2021 年 1 月 20 日维护版本

### <a name="databricks-connect-7112"></a>Databricks Connect 7.1.12

**2021 年 1 月 12 日**

* [ES-59054] 通过 Databricks Connect 修复 Delta 优化表命令
* [ES-53497] 修复 Databricks 容器服务 (DCS) 上的 Databricks Connect

### <a name="databricks-connect-7111"></a>Databricks Connect 7.1.11

**2020年 12 月 9 日**

* [ES-49865] 防止在将 Databricks Connect 用于 SBT 时由于依赖库自动同步而发生 500 内部服务器错误。

### <a name="databricks-connect-7110"></a>Databricks Connect 7.1.10

**2020 年 11 月 24 日**

* 防止状态轮询重新启动群集。
* [ES-43656] 修复与 Azure Data Lake Storage Gen2 支持相关的序列化错误。
* [ES-30594] 允许更大的 HTTP 标头并扩大 Databricks Connect 中的缓冲区。
* [ES-34186] 避免发生与日志记录相关的内存不足异常。
* [ES-39493] 防止 ``databricks-connect test``中出现递归加载错误。
* [ES-44382] 将 ``spark.databricks.service.allowTerminatedClusterStart=false`` 设置为阻止 Databricks Connect 启动已终止的 Databricks Runtime 群集。
* 修复了在禁用 INFO 日志记录级别的情况下客户端无法与服务器正确同步状态的错误。
* 一些小问题修复。

### <a name="databricks-connect-711"></a>Databricks Connect 7.1.1

**2020 年 9 月 23 日**

* [ES-32536] 修复与嵌套时区感知表达式的序列化相关的问题。
* [ES-33705] 支持 DButils 中的 Azure Data Lake Storage Gen2 文件系统。

### <a name="databricks-connect-710"></a>Databricks Connect 7.1.0

**2020 年 8 月 13 日**

适用于 Databricks Runtime 7.1 的初始 Databricks Connect 版本。

* 支持新的 Spark 3.0 DataSource V2 API。
* 支持在单个 Databricks Connect 应用程序中使用多个 SparkSession。
* 优化客户端和服务器之间的 Databricks Connect 状态同步。 当服务器处于空闲状态时，请避免过多轮询状态更新。

## <a name="databricks-connect-for-databricks-runtime-64"></a>适用于 Databricks Runtime 6.4 的 Databricks Connect

### <a name="databricks-connect-6422"></a>Databricks Connect 6.4.22

**2021 年 1 月 12 日**

* [ES-59054] 通过 Databricks Connect 修复 Delta 优化表命令
* [ES-53497] 修复 Databricks 容器服务 (DCS) 上的 Databricks Connect

### <a name="databricks-connect-6421"></a>Databricks Connect 6.4.21

**2020年 12 月 9 日**

* [ES-25890] 修复以下错误：在某些情况下，数据帧 cache() 无效并再次执行查询。
* [ES-26196] 支持在联接中使用 NATURAL 和 USING 语法。
* [ES-33475][ES-34186] 避免发生与日志记录相关的内存不足异常。
* [ES-39493] 防止 databricks-connect 测试中出现递归加载错误。
* [ES-43656] 修复与 Azure Data Lake Storage Gen2 支持相关的序列化错误。
* [ES-30594] 允许更大的 HTTP 标头并扩大 Databricks Connect 中的缓冲区。
* 一些小问题修复

### <a name="databricks-connect-642"></a>Databricks Connect 6.4.2

**2020 年 11 月 5 日**

* [ES-47216] 通过 Databricks Runtime 6.4 2020 年 11 月 3 日的更新，修复了版本检查兼容性问题。

### <a name="databricks-connect-641"></a>Databricks Connect 6.4.1

**2020年 4 月 8 日**

次要修复和更新。

### <a name="databricks-connect-640"></a>Databricks Connect 6.4.0

**2020 年 3 月 7 日**

适用于 Databricks Runtime 6.4 的初始 Databricks Connect 版本。