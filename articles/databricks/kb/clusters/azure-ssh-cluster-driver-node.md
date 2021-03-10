---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
ms.date: 02/08/2021
title: 通过 SSH 连接到群集驱动程序节点 - Azure Databricks
description: 如何通过 SSH 连接到 Azure 虚拟网络中的 Apache Spark 群集驱动程序节点
category: Clusters
author: xwang30
db-author: xin.wang@databricks.com
ms.openlocfilehash: 28e24f953bc1d9a9b80614b8464650380110f1f9
ms.sourcegitcommit: 136164cd330eb9323fe21fd1856d5671b2f001de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197029"
---
# <a name="ssh-to-the-cluster-driver-node"></a>通过 SSH 连接到群集驱动程序节点

本文介绍如何使用 SSH 连接到 Apache Spark 驱动程序节点，以便进行高级故障排除和自定义软件的安装。

> [!IMPORTANT]
>
> 仅当工作区在你的控制下部署到 Azure 虚拟网络 (VNet) 中时，你才能使用 SSH。 如果工作区未进行 VNet 注入，则 SSH 选项不会显示。

## <a name="configure-an-azure-network-security-group"></a>配置 Azure 网络安全组

与 VNet 关联的网络安全组必须允许 SSH 流量。 SSH 的默认端口为 2200。 如果使用的是自定义端口，则应在继续操作之前记下该端口。 还必须标识流量源。 它可以是单个 IP 地址，也可以是代表整个办公室的 IP 范围。

1. 在 Azure 门户中找到此网络安全组。 可以在公共子网中找到网络安全组名称。
2. 编辑入站安全规则，以便允许连接到 SSH 端口。 在此示例中，我们使用默认端口。

   > [!div class="mx-imgBorder"]
   > ![在“目标端口范围”字段中输入 SSH 端口](../clusters/../_static/images/clusters/azure-add-inbound-security-rule.png)

> [!NOTE]
>
> 请确保计算机和办公室防火墙规则允许你在用于 SSH 的端口上发送 TCP 流量。 如果在计算机或办公室防火墙上阻止 SSH 端口，则无法通过 SSH 连接到 Azure VNet。

## <a name="generate-ssh-key-pair"></a>生成 SSH 密钥对

1. 打开本地终端。
2. 通过运行以下命令创建 SSH 密钥对：

   ``ssh-keygen -t rsa -b 4096 -C``

> [!NOTE]
>
> 必须提供要将公钥和私钥保存到的目录的路径。 公钥是以扩展名 .pub 进行保存的。

## <a name="configure-a-new-cluster-with-your-public-key"></a>使用公钥配置新群集

1. 复制公钥文件的整个内容。
2. 打开群集配置页。
3. 单击“高级选项”。
4. 单击“SSH”选项卡。
5. 将公钥的整个内容粘贴到“公钥”字段中。

   > [!div class="mx-imgBorder"]
   > ![输入 SSH 公钥](../clusters/../_static/images/clusters/azure-cluster-ssh-tab.png)

6. 照常继续进行群集配置。

## <a name="configure-an-existing-cluster-with-your-public-key"></a>使用公钥配置现有群集

如果你有现有的群集，并且在群集创建过程中没有提供公钥，则可从笔记本中注入公钥。

1. 打开任何附加到群集的笔记本。
2. 将以下代码复制到笔记本中，并使用你的公钥对其进行更新，如下所示：

   ```scala
   val publicKey = "<put your public key here>"

   def addAuthorizedPublicKey(key: String): Unit = {
     val fw = new java.io.FileWriter("/home/ubuntu/.ssh/authorized_keys", /* append */ true)
     fw.write("\n" + key)
     fw.close()
   }
   addAuthorizedPublicKey(publicKey)
   ```

3. 运行代码块以注入公钥。

## <a name="ssh-into-the-spark-driver"></a>通过 SSH 连接到 Spark 驱动程序

1. 打开群集配置页。
2. 单击“高级选项”。
3. 单击“SSH”选项卡。
4. 记下“驱动程序主机名”。
5. 打开本地终端。
6. 运行以下命令（替换主机名和私钥文件路径）：

   ``ssh ubuntu@<hostname> -p 2200 -i <private-key-file-path>``