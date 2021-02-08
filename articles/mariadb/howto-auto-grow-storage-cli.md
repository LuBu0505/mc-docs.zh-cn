---
title: 自动增长存储 - Azure CLI - Azure Database for MariaDB
description: 本文介绍如何使用 Azure CLI 在 Azure Database for MariaDB 中启用自动增长存储。
author: WenJason
ms.author: v-jay
ms.topic: how-to
origin.date: 3/18/2020
ms.date: 02/08/2021
ms.custom: devx-track-azurecli
ms.openlocfilehash: 9dffabcf3412b3e91ef50493500c1362a2ffe8e3
ms.sourcegitcommit: 20bc732a6d267b44aafd953516fb2f5edb619454
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99504004"
---
# <a name="auto-grow-azure-database-for-mariadb-storage-using-the-azure-cli"></a>使用 Azure CLI 自动增长 Azure Database for MariaDB 存储
本文介绍如何将 Azure Database for MariaDB 服务器存储配置为在不影响工作负荷的情况下增长。

[达到存储限制](concepts-pricing-tiers.md#reaching-the-storage-limit)的服务器将设置为只读。 如果启用了存储自动增长，则对于预配的存储大小小于 100 GB 的服务器，可用存储空间一旦小于 1 GB 或预配存储的 10%（以这二者中的较大值为准），预配的存储大小就会立即增加 5 GB。 对于预配的存储大小大于 100 GB 的服务器，可用存储空间小于预配的存储大小的 5% 时，预配的存储大小就会增加 5%。 [此处](concepts-pricing-tiers.md#storage)所指定的最大存储限制适用。

## <a name="prerequisites"></a>必备条件

若要完成本指南：

- 需要 [Azure Database for MariaDB 服务器](quickstart-create-mariadb-server-database-using-azure-cli.md)。

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- 本文需要 Azure CLI 版本 2.0 或更高版本。

## <a name="enable-mariadb-server-storage-auto-grow"></a>启用 MariaDB 服务器存储自动增长

使用以下命令在现有服务器上启用服务器自动增长存储：

```azurecli
az mariadb server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

使用以下命令创建新服务器时启用服务器自动增长存储：

```azurecli
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location chinaeast2 --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.3
```

## <a name="next-steps"></a>后续步骤

了解[如何基于指标创建警报](howto-alert-metric.md)。