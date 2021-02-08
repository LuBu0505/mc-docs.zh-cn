---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: Power BI - Azure Databricks
description: 了解如何将 Microsoft Power BI 与 Azure Databricks 配合使用。
ms.openlocfilehash: 354f94ba4eeb6f2e620285ad2714fe8c48a395e2
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058979"
---
# <a name="power-bi"></a>Power BI

> [!IMPORTANT]
>
> Power BI Desktop 2.85.681.0 及更高版本中集成的 Azure Databricks 连接器目前为[公共预览版](../../release-notes/release-types.md)。

Microsoft Power BI 是一种业务分析服务，它使用自助式商业智能功能提供交互式可视化效果，使最终用户能够自行创建报表和仪表板，而无需依赖于信息技术人员或数据库管理员。

将 Azure Databricks 作为数据源与 Power BI 配合使用时，可以将 Azure Databricks 的性能和技术优势带给所有业务用户（而不是仅带给数据科学家和数据工程师）。

可以使用内置 Azure Databricks 连接器将 Power BI Desktop 连接到 Azure Databricks 群集。 你还可以将 Power BI 报表发布到 Power BI 服务，让用户能够使用 SSO（传递用于访问报表的相同 Azure AD 凭据）访问基础 Azure Databricks 数据。

## <a name="requirements"></a>要求

Power BI Desktop 2.85.681.0 或更高版本。 [下载最新版本](https://www.microsoft.com/download/details.aspx?id=58494)。

## <a name="step-1-get-azure-databricks-connection-information"></a>步骤 1：获取 Azure Databricks 连接信息

1. 获取[个人访问令牌](jdbc-odbc-bi.md#authentication)。
2. 获取服务器[主机名、端口和 HTTP 路径](jdbc-odbc-bi.md#get-server-hostname-port-http-path-and-jdbc-url)。

## <a name="step-2-configure-azure-databricks-cluster-connection-in-power-bi"></a>步骤 2：在 Power BI 中配置 Azure Databricks 群集连接

1. 在 PowerBI Desktop 中，转到“获取数据 > Azure”，并选择“Azure Databricks”连接器。

   > [!div class="mx-imgBorder"]
   > ![“获取数据”列表中的 Databricks 连接器](../../_static/images/third-party-integrations/power-bi/power-bi-connector-get-data.png)

2. 单击“连接”。
3. 粘贴你在步骤 1 中检索到的“服务器主机名”和“HTTP 路径”。 

   > [!div class="mx-imgBorder"]
   > ![Databricks JDBC/ODBC 配置](../../_static/images/third-party-integrations/power-bi/power-bi-connection-config.png)

4. 选择你的数据连接模式。 有关导入和 DirectQuery 之间的差异的信息，请参阅[在 Power BI Desktop 中使用 DirectQuery](https://docs.microsoft.com/power-bi/connect-data/desktop-use-directquery)。
5. 单击“确定”。
6. 在身份验证提示下，选择向 Azure Databricks 进行身份验证的方式：
   * **Azure Active Directory**：使用 Azure 帐户凭据。 单击“登录”按钮。 在登录对话框中，输入 Azure 帐户用户名（电子邮件、电话号码或 Skype）。
   * 个人访问令牌：使用在步骤 1 中检索到的个人访问令牌。

   > [!NOTE]
   >
   > 没有为 Azure Databricks 启用用户名和密码身份验证。 建议使用 Azure Active Directory 身份验证。

7. 单击“连接”  。
8. 从 Power BI 导航器中选择要查询的 Azure Databricks 数据。

   > [!div class="mx-imgBorder"]
   > ![Power BI Navigator](../../_static/images/third-party-integrations/power-bi/power-bi-navigator.png)

## <a name="access-azure-databricks-using-the-power-bi-service"></a>使用 Power BI 服务访问 Azure Databricks

将报表发布到 Power BI 服务时，可以让用户使用 SSO 访问报表和基础 Azure Databricks 数据源：

1. 将 Power BI 报表从 Power BI Desktop 发布到 Power BI 服务。
2. 启用对报表和基础数据源的单一登录 (SSO) 访问。
   1. 在 Power BI 服务中，转到报表的基础 Azure Databricks 数据集，展开“数据源凭据”，然后单击“编辑凭据”。
   1. 在配置对话框中，选择“报表查看者只能使用直接查询通过其自己的 Power BI 标识访问此数据源”，然后单击“登录”。

   > [!div class="mx-imgBorder"]
   > ![为 Databricks 数据访问启用 SSO](../../_static/images/third-party-integrations/power-bi/enable-sso.png)

   选择此选项后，将使用 DirectQuery 处理对数据源的访问，并使用访问报表的用户的 Azure AD 标识对其进行管理。 如果未选择此选项，则只有你（作为发布了报表的用户）才能访问 Azure Databricks 数据源。