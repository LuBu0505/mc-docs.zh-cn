---
title: 在 Azure Database for MySQL 灵活服务器中进行备份和还原
description: 了解使用 Azure Database for MySQL 灵活服务器进行备份和还原的概念
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 09/21/2020
ms.date: 01/11/2021
ms.openlocfilehash: 5e918c0ea98be65dee8cf1596f76757ad1e0c7ec
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023400"
---
# <a name="backup-and-restore-in-azure-database-for-mysql-flexible-server-preview"></a>在 Azure Database for MySQL 灵活服务器（预览版）中进行备份和还原

> [!IMPORTANT] 
> Azure Database for MySQL 灵活服务器当前以公共预览版提供。

Azure Database for MySQL 灵活服务器可自动创建服务器备份，并安全地将它们存储在区域内的本地冗余存储中。 备份可以用来将服务器还原到某个时间点。 备份和还原是任何业务连续性策略的基本组成部分，因为它们可以保护数据免遭意外损坏或删除。

## <a name="backup-overview"></a>备份概述

灵活服务器获取数据文件的快照备份，并将它们存储在本地冗余存储中。 服务器还执行事务日志备份，并将它们存储到本地冗余存储中。 可以通过这些备份将服务器还原到所配置的备份保留期中的任意时间点。 默认的备份保留期为七天。 可以选择将数据库备份配置为 1 到 35 天。 所有备份都使用针对静态存储数据的 AES 256 位加密进行加密。

无法导出这些备份文件。 这些备份只能用于灵活服务器中的还原操作。 还可以使用 MySQL 客户端中的 [mysqldump](../concepts-migrate-dump-restore.md#dump-and-restore-using-mysqldump-utility) 来复制数据库。

## <a name="backup-frequency"></a>备份频率

灵活服务器上的备份是基于快照的。 第一次完整快照备份在创建服务器后立即进行计划。 第一次完整快照备份将作为服务器的基准备份保留。 后续快照备份仅为差异备份。

一天至少进行一次差异快照备份。 差异快照备份不按固定计划进行。 差异快照备份每 24 小时进行一次，除非自上次差异备份后 MySQL 中的二进制日志超过 50 GB。 一天内，最多允许有 6 张差异快照。 事务日志备份每五分钟进行一次。

## <a name="backup-retention"></a>备份保留

数据库备份存储在本地冗余存储 (LRS) 中，LRS 存储在一个区域中的三个副本中。 根据服务器上的备份保持期设置来保留备份。 可以选择 1 到 35 天的保持期，默认保持期为 7 天。 可以在服务器创建期间或以后通过使用 Azure 门户更新备份配置来设置保持期。

备份保持期控制执行时间点还原操作要回退的时间距离，因为它基于可用备份。 从恢复的角度来看，备份保留期也可以视为恢复时段。 在备份保留期间内执行时间点还原所需的所有备份都保留在备份存储中。 例如，如果备份保持期设置为 7 天，则可认为恢复时段是最近 7 天。 在这种情况下，将保留在过去 7 天内还原服务器所需的所有备份。 在备份保持期为 7 天的情况下，数据库快照和事务日志备份会存储过去 8 天内的内容（备份保持期前 1 天）。

## <a name="backup-storage-cost"></a>备份存储成本

灵活服务器最高提供 100% 的已预配服务器存储作为备份存储，不收取任何额外费用。 超出的备份存储使用量按每月每 GB 标准收费。 例如，如果为服务器配置了 250 GB 的存储空间，则可以为服务器备份提供 250 GB 的存储空间，而不另外收费。 如果每日备份使用量为 25GB，则最多可有 10 天的免费备份存储。 备份所消耗的存储量超过 250GB 将按照[定价模型](https://azure.cn/pricing/details/mysql/)收费。

可以使用 Azure 门户中提供的 Azure Monitor 中的[已使用的备份存储](../concepts-monitoring.md)指标来监视服务器使用的备份存储。 已使用的备份存储指标表示根据为服务器设置的备份保持期保留的所有数据库备份和日志备份占用的存储总和。 无论数据库的总大小如何，如果服务器上的事务性活动繁重，都会导致备份存储使用率增加。

控制备份存储成本的主要方法是设置适当的备份保持期。 可指定一个 1 到 35 天的保持期。

> [!IMPORTANT]
> 灵活服务器目前不支持异地冗余备份。

## <a name="point-in-time-restore"></a>时点还原

在 Azure Database for MySQL 灵活服务器中，执行时间点还原后，会通过灵活服务器在源服务器所在区域中的备份创建新的服务器。 它在创建时，使用原始服务器在计算层、vCore 数、存储大小、备份保持期和备份冗余选项方面的配置。 此外，还会从源服务器继承标记和设置，例如虚拟网络和防火墙。 还原完成后，可以更改还原服务器的计算和存储层、配置和安全设置。

> [!NOTE]
> 还原操作完成后，有两个服务器参数重置为默认值（而不是从主服务器复制）
> *   time_zone - 此值设置为默认值“SYSTEM”
> *   event_scheduler - 还原的服务器上的 event_scheduler 设置为“OFF”

多种情况下可以使用时间点还原。 常见的一些用例包括 -
-   当用户意外删除数据库中的数据时
-   用户删除重要表或数据库
-   用户应用程序因为自身缺陷意外以错误数据覆盖正确数据。

可通过 [Azure 门户](how-to-restore-server-portal.md)选择最新的还原点或自定义还原点。

-   **最新还原点**：最新的还原点可帮助你将服务器还原到上次在源服务器上执行的备份。 还原的时间戳也将在门户上显示。 此选项可用于快速将服务器还原到最新状态。
-   **自定义还原点**：通过此选项，可在为此灵活服务器定义的保持期内选择任何时间点。 此选项可用于在从用户错误中恢复的精确时间点还原服务器。

估计的恢复时间取决于几个因素，包括数据库大小、事务日志备份大小、SKU 的计算大小以及还原时间。 事务日志恢复是还原过程中最耗时的部分。 如果选择的还原时间更接近完整快照备份计划或差异快照备份计划，则还原速度更快，因为事务日志应用程序最小。 如需估计服务器的准确恢复时间，强烈建议在你的环境中测试它，因为它有太多的环境特定变量。

> [!IMPORTANT]
> 已删除的服务器 **无法** 还原。 如果删除服务器，则属于该服务器的所有数据库也会被删除且不可恢复。 为了防止服务器资源在部署后遭意外删除或意外更改，管理员可以利用[管理锁](../../azure-resource-manager/management/lock-resources.md)。

## <a name="perform-post-restore-tasks"></a>执行还原后任务

从“最新还原点”或“自定义还原点”恢复机制还原后，都应执行以下任务，然后用户和应用程序才能重新运行 ：

-   如果需要使用新的服务器来替换原始服务器，请将客户端和客户端应用程序重定向到新服务器。
-   对于要进行连接的用户，请确保设置适当的服务器级防火墙规则和虚拟网络规则。
-   确保设置适当的登录名和数据库级权限。
-   视情况配置警报。

## <a name="next-steps"></a>后续步骤

-   了解[业务连续性](./concepts-business-continuity.md)
-   了解[备份和恢复](./concepts-backup-restore.md)
