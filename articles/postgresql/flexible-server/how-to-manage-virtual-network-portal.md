---
title: 管理虚拟网络 - Azure 门户 - Azure Database for PostgreSQL 灵活服务器
description: 使用 Azure 门户为 Azure Database for PostgreSQL 灵活服务器创建和管理虚拟网络
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: how-to
origin.date: 09/22/2020
ms.date: 01/11/2021
ms.openlocfilehash: 474b6bcef4f9c8f193072d676819c512d9d2f366
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023947"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-postgresql---flexible-server-using-the-azure-portal"></a>使用 Azure 门户为 Azure Database for PostgreSQL 灵活服务器创建和管理虚拟网络

> [!IMPORTANT]
> Azure Database for PostgreSQL 灵活服务器以预览版提供

Azure Database for PostgreSQL 灵活服务器支持两种类型的互斥网络连接方法来连接到灵活服务器。 这两个选项如下：

* 公共访问（允许的 IP 地址）
* 专用访问（VNet 集成）

本文将重点介绍使用 Azure 门户创建具有专用访问（VNet 集成）的 PostgreSQL 服务器。 通过专用访问（VNet 集成），可以将灵活服务器部署到你自己的 [Azure 虚拟网络](../../virtual-network/virtual-networks-overview.md)中。 Azure 虚拟网络提供专用的安全网络通信。 对于专用访问，与 PostgreSQL 服务器的连接仅限于你的虚拟网络。 若要了解详细信息，请参阅[专用访问（VNet 集成）](./concepts-networking.md#private-access-vnet-integration)。

在服务器创建过程中，可以将灵活服务器部署到虚拟网络和子网中。 部署灵活服务器后，无法将它移动到其他虚拟网络、子网或公共访问（允许的 IP 地址）。

## <a name="prerequisites"></a>先决条件
若要在虚拟网络中创建灵活服务器，你需要：
- [虚拟网络](../../virtual-network/quick-create-portal.md#create-a-virtual-network)
    > [!Note]
    > 虚拟网络和子网应与你的灵活服务器位于同一区域和订阅中。

-  [将子网委派](../../virtual-network/manage-subnet-delegation.md#delegate-a-subnet-to-an-azure-service)给 Microsoft.DBforPostgreSQL/flexibleServers。 该委派意味着只有 Azure Database for PostgreSQL 灵活服务器才能使用该子网。 不能在委派子网中使用其他 Azure 资源类型。

## <a name="create-azure-database-for-postgresql---flexible-server-in-an-already-existing-virtual-network"></a>在现有的虚拟网络中创建 Azure Database for PostgreSQL 灵活服务器

1. 在门户左上角选择“创建资源”  (+)。
2. 选择“数据库” > “用于 PostgreSQL 的 Azure 数据库”。 还可以在搜索框中输入“PostgreSQL”以查找该服务。
3. 选择“灵活服务器”作为部署选项。
4. 填写“基本”窗体。
5. 转到“网络”选项卡以配置连接到服务器的方式。
6. 在“连接方法”中，选择“专用访问（VNet 集成）” 。 转到“虚拟网络”，然后选择现有的“虚拟网络”和上面创建为先决条件一部分的“子网” 。
7. 选择“查看 + 创建”，查看你的灵活服务器配置。
8. 选择“创建”以预配服务器。 预配可能需要几分钟时间。

>[!Note]
> 将灵活服务器部署到虚拟网络和子网后，无法将它移动到公共访问（允许的 IP 地址）。
## <a name="next-steps"></a>后续步骤
- [使用 Azure CLI 创建和管理 Azure Database for PostgreSQL 灵活服务器虚拟网络](./how-to-manage-virtual-network-cli.md)。
- 详细了解 [Azure Database for PostgreSQL 灵活服务器中的网络](./concepts-networking.md)
- 详细了解 [Azure Database for PostgreSQL 灵活服务器虚拟网络](./concepts-networking.md#private-access-vnet-integration)。