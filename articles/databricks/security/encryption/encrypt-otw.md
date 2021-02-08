---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 10/22/2020
title: 加密群集工作器节点之间的流量 - Azure Databricks
description: 了解如何在 Azure Databricks 群集工作器节点之间以在线（简称 OTW）方式加密传输中的流量。 这也称为节点间加密。
ms.openlocfilehash: 8f65bac3cf6988570ebace5d771b871429be645e
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058665"
---
# <a name="encrypt-traffic-between-cluster-worker-nodes"></a>加密群集工作器节点之间的流量

> [!NOTE]
>
> 此功能并非适用于所有 Azure Databricks 订阅。 请联系 Microsoft 或 Databricks 客户代表，以申请访问权限。

用户查询和转换通常会通过加密通道发送到群集。 但是，在默认情况下，群集中工作器节点之间交换的数据是未加密的。 如果你的环境要求始终对数据进行加密，无论是静态加密还是传输中加密，都可以创建一个初始化脚本，该脚本将群集配置为通过 TLS 1.2 连接使用 AES 128 位加密来加密工作器节点之间的流量。

> [!NOTE]
>
> 尽管 AES 使加密例程能够利用硬件加速，但是与未加密的流量相比，仍然存在性能损失。 这种损失可能导致已加密群集上的查询花费更长的时间，具体取决于节点之间随机传输的数据量。

对工作器节点之间流量启用加密需要通过 init 脚本来设置 Spark 配置参数。 如果希望工作区中的所有群集都使用工作器到工作器加密，可以将[群集范围内的 init 脚本](../../clusters/init-scripts.md#cluster-scoped-init-script)用于单个群集，或者可以使用[全局 init](../../clusters/init-scripts.md#global-init-script) 脚本。 Init 脚本必须执行以下任务：

1. 获取 JKS 密钥存储文件和密码。
2. 设置 Spark 执行程序配置。
3. 设置 Spark 驱动程序配置。

> [!NOTE]
>
> 将为每个工作区动态生成用于启用 SSL/HTTPS 的 JKS 密钥存储文件。 该 JKS 密钥存储文件的密码采用硬编码，并且不是用来保护密钥存储的机密性的。

下面是一个示例 init 脚本，它实现这三个任务以生成群集加密配置。

```bash
#!/bin/bash

keystore_file="$DB_HOME/keys/jetty_ssl_driver_keystore.jks"
keystore_password="gb1gQqZ9ZIHS"

# Use the SHA256 of the JKS keystore file as a SASL authentication secret string
sasl_secret=$(sha256sum $keystore_file | cut -d' ' -f1)

spark_defaults_conf="$DB_HOME/spark/conf/spark-defaults.conf"
driver_conf="$DB_HOME/driver/conf/config.conf"

if [ ! -e $spark_defaults_conf ] ; then
    touch $spark_defaults_conf
fi
if [ ! -e $driver_conf ] ; then
    touch $driver_conf
fi

# Authenticate
echo "spark.authenticate true" >> $spark_defaults_conf
echo "spark.authenticate.secret $sasl_secret" >> $spark_defaults_conf

# Configure AES encryption
echo "spark.network.crypto.enabled true" >> $spark_defaults_conf
echo "spark.network.crypto.saslFallback false" >> $spark_defaults_conf

# Configure SSL
echo "spark.ssl.enabled true" >> $spark_defaults_conf
echo "spark.ssl.keyPassword $keystore_password" >> $spark_defaults_conf
echo "spark.ssl.keyStore $keystore_file" >> $spark_defaults_conf
echo "spark.ssl.keyStorePassword $keystore_password" >> $spark_defaults_conf
echo "spark.ssl.protocol TLSv1.2" >> $spark_defaults_conf
echo "spark.ssl.standalone.enabled true" >> $spark_defaults_conf
echo "spark.ssl.ui.enabled true" >> $spark_defaults_conf

head -n -1 ${DB_HOME}/driver/conf/spark-branch.conf > $driver_conf
echo " // Authenticate">> $driver_conf
echo " \"spark.authenticate\" = true" >> $driver_conf
echo " \"spark.authenticate.secret\" = \"$sasl_secret\"" >> $driver_conf
echo " // Configure AES encryption">> $driver_conf
echo " \"spark.network.crypto.enabled\" = true" >> $driver_conf
echo " \"spark.network.crypto.saslFallback\" = false" >> $driver_conf
echo " // Configure SSL">> $driver_conf
echo " \"spark.ssl.enabled\" = true" >> $driver_conf
echo " \"spark.ssl.keyPassword\" = \"$keystore_password\"" >> $driver_conf
echo " \"spark.ssl.keyStore\" = \"$keystore_file\"" >> $driver_conf
echo " \"spark.ssl.keyStorePassword\" = \"$keystore_password\"" >> $driver_conf
echo " \"spark.ssl.protocol\" = \"TLSv1.2\"" >> $driver_conf
echo " \"spark.ssl.standalone.enabled\" = true" >> $driver_conf
echo " \"spark.ssl.ui.enabled\" = true" >> $driver_conf
echo " }" >> $driver_conf
mv $driver_conf ${DB_HOME}/driver/conf/spark-branch.conf
```

驱动程序和工作器节点的初始化完成后，这些节点之间的所有流量都将加密。