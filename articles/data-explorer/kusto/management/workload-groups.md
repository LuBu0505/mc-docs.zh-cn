---
title: 工作负荷组 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的工作负荷组。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 02/07/2021
ms.openlocfilehash: 6cf5287d8212aec5f73064e5fd2e602747a8190f
ms.sourcegitcommit: 6fdfb2421e0a0db6d1f1bf0e0b0e1702c23ae6ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101090103"
---
# <a name="workload-groups-preview"></a>工作负荷组（预览）

工作负荷组充当具有相似分类标准的请求（查询和命令）的容器。 工作负荷允许对请求进行聚合监视，并定义请求的策略。 当请求的执行开始时，将对请求进行分类并将其分配给特定的工作负荷组。 然后，使用分配给工作负荷组的策略运行请求。

最多可以定义 10 个自定义工作负荷组，其中不包括两个内置的工作负荷组。

> [!NOTE]
> 不是查询或控制命令的请求不包括在工作负荷组的范围中。 例如：流式处理引入请求。

## <a name="built-in-workload-groups"></a>内置工作负荷组

预定义的工作负荷组有两个：[`internal` 工作负荷组](#internal-workload-group)和 [`default` 工作负荷组](#default-workload-group)。

### <a name="default-workload-group"></a>默认工作负荷组

当满足下列条件时，请求会分类到 `default` 组中：

* 没有用于对请求进行分类的标准。
* 曾经尝试将请求分类到不存在的组。
* 发生了常规性的分类错误。

可以执行以下操作：

* 更改用于路由这些请求的条件。
* 更改适用于 `default` 工作负荷组的策略。
* 将请求分类到 `default` 工作负荷组。

使用[监视建议](#monitoring)监视归类为内部工作负荷组的内容和对这些请求的统计信息。

### <a name="internal-workload-group"></a>内部工作负荷组

`internal` 工作负荷组中填入的是仅供内部使用的请求。

你无法：

* 更改用于路由这些请求的条件。
* 更改适用于 `internal` 工作负荷组的策略。
* 将请求分类到 `internal` 工作负荷组。

可以[监视](#monitoring)被分类到 `internal` 工作负荷组的内容以及对这些请求的统计信息。

## <a name="request-classification"></a>请求分类

根据用户定义的函数将传入的请求分类到工作负荷组。

通过定义[请求分类策略](request-classification-policy.md)来定义对请求进行分类的条件。

## <a name="workload-group-policies"></a>工作负荷组策略

可以为每个工作负荷组定义以下策略：

* [请求限制策略](request-limits-policy.md)
* [请求速率限制策略](request-rate-limit-policy.md)

## <a name="monitoring"></a>监视

[系统命令](systeminfo.md)指示已分类的工作负荷组请求类型。
使用这些命令按工作负荷组对已完成的请求聚合资源利用率。

使用 [`.show workload groups resources utilization` 命令](workload-groups-commands.md#show-workload-groups-resources-utilization)查看当前的资源使用率。


