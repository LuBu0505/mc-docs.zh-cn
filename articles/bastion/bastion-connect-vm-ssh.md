---
title: 使用 Azure Bastion 连接到 Linux VM
description: 本文介绍如何使用 Azure Bastion 连接到 Linux 虚拟机。
services: bastion
ms.service: bastion
ms.topic: how-to
origin.date: 02/12/2021
author: rockboyfor
ms.date: 03/01/2021
ms.testscope: yes
ms.testdate: 07/27/2020
ms.author: v-yeche
ms.openlocfilehash: 77c34e77f13d6dee9c9313027c43a1c6ad539c9c
ms.sourcegitcommit: e435672bdc9400ab51297134574802e9a851c60e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054360"
---
<!--Verified successfully on 09/07/2020-->
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion"></a>使用 Azure Bastion 通过 SSH 连接到 Linux 虚拟机

本文介绍如何在 Azure 虚拟网络中通过 SSH 安全无缝地连接到 Linux VM。 可直接从 Azure 门户连接到 VM。 使用 Azure Bastion 时，VM 不需要客户端、代理或其他软件。 有关 Azure Bastion 的详细信息，请参阅[概述](bastion-overview.md)。

可以使用 Azure Bastion 通过 SSH 连接到 Linux 虚拟机。 可以同时使用用户名/密码和 SSH 密钥进行身份验证。 可通过以下方法之一使用 SSH 密钥连接到 VM：

* 手动输入的私钥
* 包含私钥信息的文件

SSH 私钥必须采用以 `"-----BEGIN RSA PRIVATE KEY-----"` 开头并以 `"-----END RSA PRIVATE KEY-----"` 结尾的格式。

## <a name="prerequisites"></a>先决条件

请确保已为 VM 所在的虚拟网络设置 Azure Bastion 主机。 有关详细信息，请参阅[创建 Azure Bastion 主机](./tutorial-create-host-portal.md)。 在虚拟网络中预配和部署 Bastion 服务后，便可以使用它连接到此虚拟网络中的任何 VM。 

使用 Bastion 进行连接时，它假设你使用 RDP 连接到 Windows VM，使用 SSH 连接到 Linux VM。 若要了解如何连接到 Windows VM，请参阅[连接到 VM - Windows](bastion-connect-vm-rdp.md)。

### <a name="required-roles"></a>必需的角色

需要使用以下角色进行连接：

* 虚拟机上的读者角色
* NIC 上的读者角色（使用虚拟机的专用 IP）
* Azure Bastion 资源上的读者角色

### <a name="ports"></a>端口

若要通过 SSH 连接到 Linux VM，必须在 VM 上打开以下端口：

* 入站端口：SSH (22)

<a name="username"></a>
## <a name="connect-using-username-and-password"></a>连接：使用用户名和密码

1. 打开 [Azure 门户](https://portal.azure.cn)。 导航到要连接到的虚拟机，然后单击“连接”并从下拉列表中选择“Bastion”。

    :::image type="content" source="./media/bastion-connect-vm-ssh/connect.png" alt-text="屏幕截图显示了 Azure 门户中虚拟机的概览，其中已选择“连接”":::
1. 选择“Bastion”后，单击“使用 Bastion”。 如果未为虚拟网络预配 Bastion，请参阅[配置 Bastion](./quickstart-host-portal.md)。
1. 在“使用 Azure Bastion 连接”页上，输入“用户名”和“密码”  。

   :::image type="content" source="./media/bastion-connect-vm-ssh/password.png" alt-text="密码验证":::
1. 选择“连接”以连接到 VM。

<a name="privatekey"></a>
## <a name="connect-manually-enter-a-private-key"></a>连接：手动输入私钥

1. 打开 [Azure 门户](https://portal.azure.cn)。 导航到要连接到的虚拟机，然后单击“连接”并从下拉列表中选择“Bastion”。

    :::image type="content" source="./media/bastion-connect-vm-ssh/connect.png" alt-text="屏幕截图显示了 Azure 门户中虚拟机的概览，其中已选择“连接”":::
1. 选择“Bastion”后，单击“使用 Bastion”。 如果未为虚拟网络预配 Bastion，请参阅[配置 Bastion](./quickstart-host-portal.md)。
1. 在“使用 Azure Bastion 连接”页上，输入“用户名”和“SSH 私钥”  。

   :::image type="content" source="./media/bastion-connect-vm-ssh/ssh-private-key.png" alt-text="SSH 私钥身份验证":::
1. 将私钥输入到文本区域“SSH 私钥”中（或直接进行粘贴）。
1. 选择“连接”以连接到 VM。

<a name="ssh"></a>
## <a name="connect-using-a-private-key-file"></a>连接：使用私钥文件

1. 打开 [Azure 门户](https://portal.azure.cn)。 导航到要连接到的虚拟机，然后单击“连接”并从下拉列表中选择“Bastion”。

   :::image type="content" source="./media/bastion-connect-vm-ssh/connect.png" alt-text="屏幕截图显示了 Azure 门户中虚拟机的概览，其中已选择“连接”":::
1. 选择“Bastion”后，单击“使用 Bastion”。 如果未为虚拟网络预配 Bastion，请参阅[配置 Bastion](./quickstart-host-portal.md)。
1. 在“使用 Azure Bastion 连接”页上，输入“用户名”和“本地文件中的 SSH 私钥”  。

   :::image type="content" source="./media/bastion-connect-vm-ssh/private-key-file.png" alt-text="SSH 私钥文件":::

1. 通过浏览方式找到该文件，然后选择“打开”。
1. 选择“连接”以连接到 VM。 单击“连接”后，可连接到此虚拟机的 SSH 将直接在 Azure 门户中打开。 此连接通过 HTML5 在虚拟机的专用 IP 上使用 Bastion 服务的端口 443 进行。

<a name="akv"></a>
<!--Not Available on ## Connect: Using a private key stored in Azure Key Vault-->

## <a name="next-steps"></a>后续步骤

有关 Azure Bastion 的详细信息，请参阅 [Bastion 常见问题解答](bastion-faq.md)。

<!--Update_Description: update meta properties, wording update, update link-->