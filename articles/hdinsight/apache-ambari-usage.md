---
title: Azure HDInsight 中 Apache Ambari 的用法
description: 介绍如何在 Azure HDInsight 中使用 Apache Ambari。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
origin.date: 02/05/2020
ms.date: 03/02/2020
ms.openlocfilehash: 3fbde9aa4fd0ce472a69a8eee54e03f75f3d8cc8
ms.sourcegitcommit: b6fead1466f486289333952e6fa0c6f9c82a804a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2020
ms.locfileid: "96300927"
---
# <a name="apache-ambari-usage-in-azure-hdinsight"></a>Azure HDInsight 中 Apache Ambari 的用法

HDInsight 使用 Apache Ambari 来部署和管理群集。 Ambari 代理在每个节点（头节点、工作器节点、Zookeeper 和边缘节点（如果存在））上运行。 Ambari 服务器仅在头节点（hn0 或 hn1）上运行。 一次只能运行 Ambari 服务器的一个实例。 运行的实例数由 HDInsight 故障转移控制器控制。 当一个头节点由于重新启动或维护而关闭时，另一个头节点将成为活动头节点，并启动另一个头节点上的 Ambari 服务器。

所有群集配置应通过 [Ambari UI](./hdinsight-hadoop-manage-ambari.md) 完成，重启节点时会覆盖任何本地更改。

## <a name="failover-controller-services"></a>故障转移控制器服务

HDInsight 故障转移控制器还负责更新头节点主机的 IP 地址，该地址指向当前活动头节点。 所有 Ambari 代理配置为向头节点主机报告其状态和检测信号。 故障转移控制器是群集中每个节点上运行的一组服务，如果这些服务未运行，头节点故障转移可能无法正常进行，并且在尝试访问 Ambari 服务器时，会出现 HTTP 502 错误。

若要检查哪个头节点处于活动状态，方法之一是通过 SSH 连接到群集中的某个节点，然后运行 `ping headnodehost`，并将返回的 IP 与这两个头节点的 IP 进行比较。

如果故障转移控制器服务未运行，头节点故障转移可能不会正常进行，因而无法运行 Ambari 服务器。 若要检查故障转移控制器服务是否正在运行，请执行：

```bash
ps -ef | grep failover
```

## <a name="logs"></a>日志

在活动头节点上，可以检查位于以下位置的 Ambari 服务器日志：

```
/var/log/ambari-server/ambari-server.log
/var/log/ambari-server/ambari-server-check-database.log
```

在群集中的任一节点上，可以检查位于以下位置的 Ambari 代理日志：

```bash
/var/log/ambari-agent/ambari-agent.log
```

## <a name="service-start-sequences"></a>服务启动顺序

这是服务在引导时的启动顺序：

1. Hdinsight 代理启动故障转移控制器服务。
1. 故障转移控制器服务启动每个节点上的 Ambari 代理，并启动活动头节点上的 Ambari 服务器。

## <a name="ambari-database"></a>Ambari 数据库

HDInsight 幕后会在 SQL 数据库中创建数据库，用作 Ambari 服务器的数据库。 默认的[服务层级是 S0](../azure-sql/database/elastic-pool-scale.md)。

对于在创建时其工作器节点数超过 16 个的任何群集，数据库服务层级为 S2。

## <a name="takeaway-points"></a>要点

切勿手动启动/停止 Ambari 服务器或 Ambari 代理服务，除非你要尝试重启这些服务来解决某个问题。 若要强制故障转移，可以重新启动活动头节点。

切勿手动修改任何群集节点上的任何配置文件，应该让 Ambari UI 完成此类作业。

## <a name="next-steps"></a>后续步骤

* [使用 Apache Ambari Web UI 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari.md)
* [使用 Apache Ambari REST API 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari-rest-api.md)

[!INCLUDE [troubleshooting next steps](../../includes/hdinsight-troubleshooting-next-steps.md)]