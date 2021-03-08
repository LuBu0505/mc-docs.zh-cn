---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 02/08/2021
title: 群集 - 提示和疑难解答 - Azure Databricks
ms.openlocfilehash: 757cf7fa9b093be1642231b6440357816485ff28
ms.sourcegitcommit: 136164cd330eb9323fe21fd1856d5671b2f001de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102196834"
---
# <a name="clusters-tips-and-troubleshooting"></a>群集：提示和疑难解答

这些文章有助于管理 Apache Spark 群集。

* [如何计算群集中的核心数](calculate-number-of-cores.md)
* [CPU 内核限制阻止创建群集](azure-core-limit.md)
* [IP 地址限制阻止创建群集](azure-ip-limit.md)
* [群集启动缓慢，缺少节点](azure-nodes-not-acquired.md)
* [无法应用更新的群集策略](edited-policy-not-applied.md)
* [群集无法启动](cluster-failed-launch.md)
* [作业因群集管理器核心实例请求限制而失败](cluster-manager-limit.md)
* [管理员用户无法重启群集来运行作业](cluster-restart-fails-admin-user.md)
* [由于 Ganglia 指标填充根分区，导致群集速度缓慢](slowdown-from-root-disk-fill.md)
* [设置执行程序日志级别](set-executor-log-level.md)
* [群集意外终止](termination-reasons.md)
* [如何在 Azure Databricks 群集上覆盖 ``log4j`` 配置](overwrite-log4j-logs.md)
* [添加配置设置会覆盖所有默认的 ``spark.executor.extraJavaOptions`` 设置](conf-overwrites-default-settings.md)
* [Apache Spark 执行程序内存分配](spark-executor-memory.md)
* [Apache Spark UI 显示的节点内存量少于总节点内存量](spark-shows-less-memory.md)
* [通过 SSH 连接到群集驱动程序节点](azure-ssh-cluster-driver-node.md)
* [将群集配置为使用自定义 NTP 服务器](use-internal-ntp.md)