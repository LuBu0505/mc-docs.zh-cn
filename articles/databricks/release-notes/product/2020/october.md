---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/01/2020
title: 2020 年 10 月 - Azure Databricks
description: Azure Databricks 新功能和改进的 2020 年 10 月发行说明。
ms.openlocfilehash: 35e7f399a797c9ffe76c9324db630fdd6f42d202
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058713"
---
# <a name="october-2020"></a>2020 年 10 月

这些功能和 Azure Databricks 平台的改进已于 2020 年 10 月发布。

> [!NOTE]
>
> 发布分阶段进行。 在初始发布日期后，可能最长需要等待一周，你的 Azure Databricks 帐户才会更新。

## <a name="new-azure-databricks-power-bi-connector-available-in-the-online-power-bi-service-public-preview"></a>联机 Power BI 服务中提供的新的 Azure Databricks Power BI 连接器（公共预览版）

2020 年 10 月 28 日

9 月份在 Power BI Desktop 中发布的新的 Azure Databricks [Power BI 连接器](september.md#powerbi-connector)（公共预览版）现在也可在 Power BI 服务（也称为联机 Power BI）中使用了。

通过此原生连接器，你可以使用 Azure Active Directory 凭据从 Power BI Desktop 连接到 Azure Databricks，以及使用 SSO 连接到 Power BI 服务。 此原生连接器支持 [DirectQuery](https://docs.microsoft.com/power-bi/connect-data/desktop-directquery-about)，允许你直接在 Azure Databricks 中访问数据，查询最新数据和实施数据湖安全控制。 如果你使用 Power BI 服务，则 SSO 可以确保所有报表用户都使用其自己的凭据访问 Azure Databricks - 无需在 Power BI 中重复进行安全控制。 此连接器基于 Databricks ODBC 驱动程序，该驱动程序经过了优化，可以加快结果的传输。

有关详细信息，请参阅 [Power BI](../../../integrations/bi/power-bi.md)。

## <a name="databricks-runtime-74-beta"></a>Databricks Runtime 7.4（Beta 版本）

**2020 年 10 月 21 日**

Databricks Runtime 7.4、Databricks Runtime 7.4 ML 和用于基因组学的 Databricks Runtime 7.4 现已作为 Beta 版本发布。

有关信息，请参阅 [Databricks Runtime 7.4](../../runtime/7.4.md)、[用于机器学习的 Databricks Runtime 7.4](../../runtime/7.4ml.md) 和[用于基因组学的 Databricks Runtime 7.4](../../runtime/7.4genomics.md) 提供的完整发行说明。

## <a name="use-customer-managed-keys-for-dbfs-root-ga"></a>为 DBFS 根使用客户管理的密钥 (GA)

**2020 年 10 月 21 日**

现在，在 Azure Key Vault 中使用你自己的加密密钥来加密 DBFS 存储帐户的功能已正式发布。 该功能需要 [Azure Databricks Premium 计划](https://databricks.com/product/azure-pricing)。 请参阅[为 DBFS 根配置客户管理的密钥](../../../security/keys/customer-managed-keys-dbfs/index.md)。

## <a name="expanded-experiment-access-control-acls"></a>扩展的试验访问控制 (ACL)

2020 年 10 月 20-27 日：版本 3.31

现在已为所有部署启用随 Azure Databricks 平台版本 3.29（2020 年 9 月 23-29 日）引入的[扩展试验权限](september.md#artifact-access-control-lists-acls-in-mlflow)。

有关详细信息，请参阅 [MLflow 项目权限](../../../security/access-control/workspace-acl.md#mlflow-artifact-permissions)。

## <a name="high-fidelity-import-and-export-of-jupyter-notebook-ipynb-files"></a>以高保真的形式导入和导出 Jupyter 笔记本 (ipynb) 文件

2020 年 10 月 20-27 日：版本 3.31

Azure Databricks 现在能够以 Jupyter 笔记本 (ipynb) 文件格式高保真地导入和导出笔记本。 导入最初从 Azure Databricks 导出的 Jupyter 笔记本文件时，会保留结果和仪表板。 以前只能通过 DBC 外部格式实现此操作。 升级到 Jupyter 笔记本处理后，Azure Databricks 笔记本现在与 GitHub、nbformat、nbdime 和 nbconvert 等工具兼容。 请参阅[笔记本外部格式](../../../notebooks/notebooks-manage.md#notebook-external-formats)。

## <a name="scim-api-improvement-both-indirect-and-direct-groups-returned-in-user-record-response"></a>SCIM API 改进：用户记录响应中既返回间接组，也返回直接组

2020 年 10 月 20-27 日：版本 3.31

SCIM API 返回的用户记录现在符合 SCIM RFC-7643 标准，因为响应列出了用户所属的组。 SCIM RFC-7643 定义了两种类型的组成员资格：直接和间接。 API 过去仅返回直接成员资格。 它现在同时返回直接和间接成员资格。 它还包含一个新的 ``type`` 字段，用于区分这两者。 请参阅 [SCIM API](../../../dev-tools/api/latest/scim/index.md)。

## <a name="azure-request-id-is-now-included-in-exceptions"></a>Azure 请求 ID 现包含在异常消息中

2020 年 10 月 20-27 日：版本 3.31

以下日志中现在包含有助于进行故障排除的 Azure 请求 ID (``x-ms-client-request-id``)：

* 群集事件
* Azure 引发异常时的服务日志
* ``AzureLaunchException`` 的使用情况日志

## <a name="databricks-runtime-65-series-support-ends"></a>Databricks Runtime 6.5 系列支持结束

**2020 年 10 月 14 日**

对 Databricks Runtime 6.5、用于机器学习的 Databricks Runtime 6.5 以及用于基因组学的 Databricks Runtime 6.5 的支持已于 10 月 14 日结束。 请参阅 [Databricks 运行时支持生命周期](../../runtime/databricks-runtime-ver.md#runtime-support)。

## <a name="secure-cluster-connectivity-no-public-ips-is-public-preview"></a>安全群集连接（无公共 IP）为公共预览版

2020 年 10 月 13 日：版本 3.30

安全群集连接允许你启动所有节点都只有专用 IP 地址的群集，从而增强安全性。 可以为新工作区启用安全群集连接。 如果你要迁移具有公共 IP 的工作区，则应创建允许进行安全群集连接的新工作区，并将资源迁移到新工作区。 有关详细信息，请联系你的 Microsoft 或 Databricks 帐户团队。

有关详细信息，请参阅[安全群集连接](../../../security/secure-cluster-connectivity.md)。

## <a name="scim-api-improvement-ref-field-response"></a>SCIM API 改进：``$ref`` 字段响应

**2020 年 10 月 7 日至 13 日：版本 3.30**

在 SCIM 响应中，``$ref`` 字段过去会在 URI 中返回一个内部主机名。 SCIM 响应现在会根据 SCIM RFC-7463 的规定在 ``$ref`` 字段中返回一个相对 URI。

## <a name="diagnostic-audit-logs-are-now-delivered-with-low-latency"></a>诊断（审核）日志现在以低延迟方式提供

**2020 年 10 月 7 日至 13 日：版本 3.30**

诊断审核日志现在以低延迟方式传送，在 Azure 商业区域中，可在 15 分钟内传送 99% 的日志。 无需执行任何操作即可实现低延迟传送。 请参阅[诊断日志传送](../../../administration-guide/account-settings/azure-diagnostic-logs.md#diagnostic-log-delivery)。

## <a name="databricks-runtime-73-73-ml-and-73-genomics-declared-long-term-support-lts"></a>Databricks Runtime 7.3、7.3 ML 和 7.3 基因组学声明的长期支持 (LTS)

2020 年 10 月 8 日

我们已声明 Databricks Runtime 7.3、用于机器学习的 Databricks Runtime 7.3 以及用于基因组学的 Databricks Runtime 7.3 为长期支持 (LTS) 版本。

Azure Databricks 为 [LTS 版本](../../runtime/databricks-runtime-ver.md#runtime-support)提供整整两年的支持。 对这些版本的支持将持续至 2022 年 9 月 24 日。

有关这些 Databricks Runtime 版本的详细信息，请参阅 [Databricks Runtime 7.3 LTS](../../runtime/7.3.md)、[用于机器学习的 Databricks Runtime 7.3 LTS](../../runtime/7.3ml.md) 和[用于基因组学的 Databricks Runtime 7.3 LTS](../../runtime/7.3genomics.md) 的发行说明。

## <a name="render-images-at-higher-resolution-using-matplotlib"></a>使用 matplotlib 在更高的分辨率下呈现图像

**2020 年 10 月 7 日至 13 日：版本 3.30**

现在可以在 Python 笔记本中以标准分辨率（也称为视网膜分辨率）两倍的分辨率呈现 matplotlib 图像，为高分辨率屏幕用户提供更好的可视化效果体验。 请参阅[以更高的分辨率呈现图像](../../../notebooks/visualizations/matplotlib.md#render-images-at-higher-resolution)。

## <a name="use-the-databricks-cli-or-the-databricks-api-to-create-azure-key-vault-backed-secret-scopes"></a>使用 Databricks CLI 或 Databricks API 创建由 Azure Key Vault 提供支持的机密范围

**2020 年 10 月 7 日至 13 日：版本 3.30**

现在可以使用 Databricks CLI 或 Databricks REST API 创建由 Azure Key Vault 提供支持的机密范围。 请参阅[使用 Databricks CLI 创建 Azure Key Vault 支持的机密范围](../../../security/secrets/secret-scopes.md#create-an-azure-key-vault-backed-secret-scope-using-the-databricks-cli)和[创建机密范围](../../../dev-tools/api/latest/secrets.md#secretsecretservicecreatescope)。

## <a name="use-azure-ad-tokens-to-authenticate-to-the-databricks-cli"></a>使用 Azure AD 令牌向 Databricks CLI 进行身份验证

**2020 年 10 月 7 日至 13 日：版本 3.30**

现在可以使用 Azure Active Directory 令牌向 Databricks CLI 进行身份验证。 请参阅[设置身份验证](../../../dev-tools/cli/index.md#set-up-authentication)。

## <a name="new-azure-regions-public-preview"></a>新的 Azure 区域（公共预览版）

2020 年 10 月 1 日

Azure Databricks 目前在“瑞士北部”、“中国东部 2”和“中国北部 2”区域以公共预览版形式提供。