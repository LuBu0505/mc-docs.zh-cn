---
title: Azure Monitor API 停用
description: 介绍旧版 Microsoft.operationalinsights 资源提供程序 API 的停用信息。
ms.subservice: logs
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 02/20/2021
ms.openlocfilehash: 4e06a9dc0d168af6ccf369dc44cdc927891b7694
ms.sourcegitcommit: b2daa3a26319be676c8e563a62c66e1d5e698558
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102205267"
---
# <a name="operationalinsights-api-version-retirement"></a>OperationalInsights API 版本停用
Microsoft 至少会在停用 API 前提前 12 个月发出通知，以便顺利转换为更高版本/受支持版本。 我们发布了 OperationalInsights 资源提供程序 API 新版本 (2020-08-01)，并将于 2024 年 2 月 29 日停用任何早期的 API 版本。

我们建议你立即开始使用 2020-08-01 版本，获享新功能的好处，例如[专用集群](../log-query/logs-dedicated-clusters.md)[客户管理的密钥](../logs/customer-managed-keys.md)和[数据导出](./logs-data-export.md)。 此外，新特性、功能和优化仅添加到最新的 API 中。

2024 年 2 月 29 日后，Azure Monitor 将不再支持 2020-08-01 之前的早期 API 版本。 如果不想升级，则 Azure Monitor 服务在 2024 年 2 月 29 日之前，将继续处理从早期版本发送的请求。

## <a name="migration-steps"></a>迁移步骤
根据所使用的配置方法，你应该在 REST 请求和资源管理器模板中更新新版本 。 请按照以下示例更新 API 版本：

1. REST API 请求在请求的 URL 中使用该 API 版本。 将该版本替换为最新版本 (2020-08-01)，如下面的示例中所示。

    ```rest
    https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Azure 资源管理器模板使用资源的 apiVersion 属性中的 API 版本。 将该版本替换为最新版本 (2020-08-01)，如下面的示例中所示。


    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string",
                "metadata": {
                "description": "Name of the workspace."
                }
            },
            "resources": [
            {
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('workspaceName')]",
                "apiVersion": "2020-08-01",
                "location": "chinanorth",
                "properties": {
                    "sku": {
                        "name": "pergb2018"
                    },
                    "retentionInDays": 30,
                    "features": {
                        "searchVersion": 1,
                        "legacy": 0,
                        "enableLogAccessUsingOnlyResourcePermissions": true
                    }
                }
            }
        ]
    }
    }
    ```


## <a name="next-steps"></a>后续步骤

- 请参阅 [OperationalInsights 工作区 API 参考](https://docs.microsoft.com/rest/api/loganalytics/workspaces)。

