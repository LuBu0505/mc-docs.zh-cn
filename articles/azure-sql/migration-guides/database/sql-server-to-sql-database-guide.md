---
title: SQL Server 到 SQL 数据库：迁移指南
description: 根据本指南将 SQL Server 数据库迁移至 Azure SQL 数据库。
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: WenJason
ms.author: v-jay
ms.reviewer: MashaMSFT
origin.date: 11/06/2020
ms.date: 01/04/2021
ms.openlocfilehash: 8ea00c8f9f45ff5e647c7f285595f22414a69fe0
ms.sourcegitcommit: cf3d8d87096ae96388fe273551216b1cb7bf92c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/31/2020
ms.locfileid: "97830446"
---
# <a name="migration-guide-sql-server-to-sql-database"></a>迁移指南：将 SQL Server 到 SQL 数据库
[!INCLUDE[appliesto--sqldb](../../includes/appliesto-sqldb.md)]

本指南帮助你将 SQL Server 实例迁移至 Azure SQL 数据库。 

你可以迁移在本地或以下位置运行的 SQL Server： 

- 虚拟机上的 SQL Server  
- Amazon Web Services (AWS) EC2 
- Amazon 关系数据库服务 (AWS RDS) 
- 计算引擎 (Google Cloud Platform - GCP)  
- Cloud SQL for SQL Server (Google Cloud Platform - GCP) 

如需更多迁移信息，请参阅[迁移概述](sql-server-to-sql-database-overview.md)。 有关其他方案，请参阅[数据库迁移指南](/dms/)。

:::image type="content" source="media/sql-server-to-database-overview/migration-process-flow-small.png" alt-text="迁移流程":::

## <a name="prerequisites"></a>先决条件 

若要将 SQL Server 迁移到 Azure SQL 数据库，请确保满足以下先决条件： 

- 一个已选定的[迁移方法](sql-server-to-sql-database-overview.md#compare-migration-options)及相应的工具 
- 在可以连接到源 SQL Server 的计算机上安装的[数据迁移助手 (DMA)](https://www.microsoft.com/download/details.aspx?id=53595)
- 一个目标 [Azure SQL 数据库](../../database/single-database-create-quickstart.md)


## <a name="pre-migration"></a>迁移前

验证你的源环境是否受支持后，开始预迁移阶段。 发现所有现有数据源，评估迁移可行性，确定可能会妨碍迁移的任何阻碍性问题。 

### <a name="discover"></a>发现

在“发现”阶段，扫描网络以查明你的组织使用的所有 SQL Server 实例和功能。 

你可以使用  [Microsoft 评估和规划工具包（“MAP 工具包”）](https://www.microsoft.com/download/details.aspx?id=7826)来评估你当前的 IT 基础结构。 该工具包提供了功能强大的清单、评估和报告工具，可以简化迁移规划过程。 

若要详细了解可用于“发现”阶段的工具，请参阅[可用于数据迁移方案的服务和工具](../../../dms/dms-tools-matrix.md)。 

### <a name="assess"></a>评估 

发现数据源后，评估可迁移到 Azure SQL 数据库的任何本地 SQL Server 数据库，以确定迁移阻碍或兼容性问题。 

你可以使用数据迁移助手（4.1 及更高版本）来评估要获取的数据库： 

- [Azure 目标建议](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb)
- [Azure SKU 建议](https://docs.microsoft.com/sql/dma/dma-sku-recommend-sql-db)

若要使用“数据库迁移评估”来评估你的环境，请执行以下步骤： 

1. 打开[数据迁移助手 (DMA)](https://www.microsoft.com/download/details.aspx?id=53595)。 
1. 选择“文件”，然后选择“新建评估”。 
1. 指定一个项目名称，选择“SQL Server”作为源服务器类型，然后选择“Azure SQL 数据库”作为目标服务器类型。 
1. 选择要生成的评估报告的类型。 例如，数据库兼容性和功能奇偶一致性。 根据评估类型，源 SQL Server 上所需的权限可能有所不同。  在运行评估之前，DMA 会突出显示所选顾问所需的权限。
    - “功能奇偶一致性”类别提供了一套全面的建议、Azure 中可用的替代项以及缓解步骤来帮助你计划迁移项目 （需要 sysadmin 权限）。
    - “兼容性问题”类别标识了可能会阻止迁移的“部分支持或完全不支持”功能兼容性问题，以及用于解决这些问题的建议（需要 `CONNECT SQL`、`VIEW SERVER STATE` 和 `VIEW ANY DEFINITION` 权限）。
1. 指定 SQL Server 的源连接详细信息并连接到源数据库。
1. 选择“开始评估”。 
1. 完成此过程后，选择并查看针对妨碍迁移的问题和功能奇偶一致性问题的评估报告。 还可以将评估报告导出到文件，以便与组织中的其他团队或人员共享。 
1. 确定可以最大程度地减少迁移后工作的数据库兼容性级别。  
1. 确定适合你的本地工作负荷的最佳 Azure SQL 数据库 SKU。 

若要了解详细信息，请参阅[使用数据迁移助手进行 SQL Server 迁移评估](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)。

如果评估遇到多个阻碍，确认你的数据库未准备好进行 Azure SQL 数据库迁移，则还可以考虑：

- [Azure SQL 托管实例](../managed-instance/sql-server-to-managed-instance-overview.md)（如果有多个以实例为作用域的依赖项）



#### <a name="scaled-assessments-and-analysis"></a>规模化评估和分析
数据迁移助手支持执行规模化评估以及对评估报告进行合并以方便分析。 

如果你有多个服务器和数据库需要进行规模化评估和分析（用于提供更广泛的数据资产视图），请参阅以下链接来了解详细信息：

- [使用 PowerShell 执行规模化评估](https://docs.microsoft.com/sql/dma/dma-consolidatereports)
- [使用 Power BI 分析评估报告](https://docs.microsoft.com/sql/dma/dma-consolidatereports#dma-reports)

> [!IMPORTANT]
> 还可以使用 [DMA 命令行实用工具](https://docs.microsoft.com/sql/dma/dma-commandline)自动执行对多个数据库（尤其是大型数据库）运行的规模化评估，并将评估上传到 [Azure Migrate](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb#view-target-readiness-assessment-results) 以进一步进行分析和目标准备。

## <a name="migrate"></a>迁移

完成与预迁移阶段相关联的任务后，便可以执行架构和数据迁移。 

使用所选的[迁移方法](sql-server-to-sql-database-overview.md#compare-migration-options)迁移你的数据。 

本指南介绍了两个最常用的选项 - 数据迁移助手和 Azure 数据库迁移服务。 

### <a name="data-migration-assistant-dma"></a>数据迁移助手 (DMA)

若要使用 DMA 将数据库从 SQL Server 迁移到 Azure SQL 数据库，请执行以下步骤： 

1. 下载并安装[数据库迁移助手](https://www.microsoft.com/download/details.aspx?id=53595)。
1. 创建一个新项目，并选择“迁移”作为项目类型。
1. 将源服务器类型设置为“SQL Server”，将目标服务器类型设置为“Azure SQL 数据库”，选择“架构和数据”作为迁移范围，然后选择“创建”。
1. 在迁移项目中，指定源服务器详细信息（例如服务器名称）、用于连接到服务器的凭据，以及要迁移的源数据库。
1. 在目标服务器详细信息中，指定 Azure SQL 数据库服务器名称、用于连接到服务器的凭据，以及要迁移到的目标数据库。
1. 选择架构对象并将其部署到目标 Azure SQL 数据库。
1. 最后，选择“开始数据迁移”并监视迁移进度。

有关详细教程，请参阅[使用数据迁移助手将本地 SQL Server 或 Azure VM 上的 SQL Server 迁移到 Azure SQL 数据库](https://docs.microsoft.com/sql/dma/dma-migrateonpremsqltosqldb)。 


> [!NOTE]
> - 通过提供更多资源，在导入过程中将数据库扩展到更高的服务层级和更大的计算大小，以最大程度地提高导入速度。 然后，可以在导入成功后进行缩减。</br>
> - 导入后的数据库的兼容性级别基于源数据库的兼容性级别。 


### <a name="azure-database-migration-service-dms"></a>Azure 数据库迁移服务 (DMS)

若要使用 DMS 将数据库从 SQL Server 迁移到 Azure SQL 数据库，请执行以下步骤：

1. 在订阅中注册 Microsoft.DataMigration 资源提供程序（如果尚未这样做）。 
1. 在选定的所需位置创建 Azure 数据库迁移服务实例（最好与目标 Azure SQL 数据库位于同一区域）。 选择一个现有虚拟网络或新建一个虚拟网络来承载 DMS 实例。
1. 创建 DMS 实例后，创建一个新的迁移项目，将源服务器类型指定为“SQL Server”，将目标服务器类型指定为“Azure SQL 数据库”。 在迁移项目创建边栏选项卡中选择“脱机数据迁移”作为活动类型。
1. 在“迁移源”详细信息页上指定源 SQL Server 详细信息，在“迁移目标”详细信息页上指定目标 Azure SQL 数据库详细信息。
1. 映射用于迁移的源数据库和目标数据库，然后选择要迁移的表。
1. 复查迁移摘要，然后选择“运行迁移”。 然后，你可以监视迁移活动，并检查数据库迁移进度。

有关详细教程，请参阅[使用 DMS 将 SQL Server 迁移到 Azure SQL 数据库](../../../dms/tutorial-sql-server-to-azure-sql.md)。 

## <a name="data-sync-and-cutover"></a>数据同步和直接转换

使用将数据更改持续地从源复制/同步到目标的迁移选项时，源数据和架构可能会变化并偏离目标。 请确保在数据同步过程中捕获对源的所有更改，并在迁移过程中将其应用到目标。 

验证源和目标上的数据是否相同后，可以从源环境直接转换到目标环境。 请务必与业务/应用程序团队一起计划直接转换过程，以确保在直接转换时的最小中断不会影响业务连续性。 

> [!IMPORTANT]
> 若要详细了解使用 DMS 在迁移过程中进行直接转换的相关具体步骤，请参阅[执行迁移直接转换](../../../dms/tutorial-sql-server-azure-sql-online.md#perform-migration-cutover)。


## <a name="post-migration"></a>迁移后

成功完成迁移阶段后，执行一系列的迁移后任务，确保一切顺利高效地进行。 

迁移后阶段对于协调任何数据准确性问题、验证完整性以及解决工作负荷的性能问题至关重要。 

### <a name="remediate-applications"></a>修正应用程序 

将数据迁移到目标环境后，以前使用源的所有应用程序都需要开始使用目标。 在某些情况下，实现这一点需要对应用程序进行更改。

### <a name="perform-tests"></a>执行测试

数据库迁移的测试方法包括以下活动：

1. 开发验证测试：要测试数据库迁移，需要使用 SQL 查询。 必须创建针对源数据库和目标数据库运行的验证查询。 验证查询应涵盖已定义的范围。
1. 设置测试环境：测试环境应包含源数据库和目标数据库的副本。 请确保隔离测试环境。
1. 运行验证测试：针对源和目标运行验证测试，然后分析结果。
1. 运行性能测试：针对源和目标运行性能测试，然后分析和比较结果。

   > [!NOTE]
   > 为帮助开发和运行迁移后验证测试，请考虑使用合作伙伴 [QuerySurge](https://www.querysurge.com/company/partners/microsoft) 提供的数据质量解决方案。 


## <a name="leverage-advanced-features"></a>利用高级功能 

请确保利用 Azure SQL 数据库提供的基于云的高级功能，例如[内置高可用性](../../database/high-availability-sla.md)、[威胁检测](../../database/azure-defender-for-sql.md)以及[监视和优化工作负荷](../../database/monitor-tune-overview.md)。 

只有将[数据库兼容性级别](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database)更改为最新的兼容性级别 (150) 后，某些 SQL Server 功能才可用。 

若要了解详细信息，请参阅[在迁移后管理 Azure SQL 数据库](../../database/manage-data-after-migrating-to-database.md)


## <a name="next-steps"></a>后续步骤

- 有关在执行各种数据库和数据迁移方案及专门任务时可为你提供帮助的矩阵（包含 Azure 提供的与第三方提供的服务和工具），请参阅[数据迁移服务和工具](../../../dms/dms-tools-matrix.md)。

- 若要详细了解 SQL 数据库，请参阅：
    - [Azure SQL 数据库概述](../../database/sql-database-paas-overview.md)
   - [Azure 总拥有成本计算器](https://azure.cn/pricing/calculator/) 


- 若要详细了解云迁移的框架和采用周期，请参阅
   -  [适用于 Azure 的云采用框架](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [为迁移到 Azure 的工作负荷计算成本和调整大小的最佳做法](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- 若要评估应用程序访问层，请参阅 [Data Access Migration Toolkit（预览版）](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- 若要详细了解如何执行数据访问层 A/B 测试，请参阅[数据库实验助手](https://docs.microsoft.com/sql/dea/database-experimentation-assistant-overview)。
