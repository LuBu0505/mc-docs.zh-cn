---
title: 重启服务器 - Azure CLI - Azure Database for PostgreSQL - 单一服务器
description: 本文介绍了如何使用 Azure CLI 重启 Azure Database for PostgreSQL - 单一服务器
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: how-to
origin.date: 5/6/2019
ms.date: 01/11/2021
ms.custom: devx-track-azurecli
ms.openlocfilehash: 944a958986e9a21c0f050e8b6a723ba7c796f36e
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022575"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>使用 Azure CLI 重启 Azure Database for PostgreSQL - 单一服务器
本主题介绍如何重启 Azure Database for PostgreSQL 服务器。 出于维护原因，可能需要重启服务器，这会在服务器执行操作时导致短暂中断。

如果服务处于繁忙状态，则会阻止重启服务器。 例如，服务可以处理缩放 vCores 等先前请求的操作。
 
> [!NOTE] 
> 完成重启所需的时间取决于 PostgreSQL 恢复过程。 若要减少重启时间，建议在重启之前尽量减少服务器上发生的活动量。 你可能还需要增加检查点频率。 还可以调整与检查点相关的参数值，包括 `max_wal_size`。 还建议在重启服务器之前运行 `CHECKPOINT` 命令。

## <a name="prerequisites"></a>先决条件
若要完成本操作说明指南：
- 创建 [Azure Database for PostgreSQL 服务器](quickstart-create-server-up-azure-cli.md)。

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- 本文需要 Azure CLI 版本 2.0 或更高版本。

## <a name="restart-the-server"></a>重启服务器

使用以下命令重启服务器：

```azurecli
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>后续步骤

了解[如何在 Azure Database for PostgreSQL 中设置参数](howto-configure-server-parameters-using-cli.md)