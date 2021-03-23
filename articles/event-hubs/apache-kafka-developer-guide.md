---
title: 针对事件中心的 Apache Kafka 开发人员指南
description: 本文提供相关文章的链接，这些文章介绍了如何将 Kafka 应用程序与 Azure 事件中心集成。
ms.date: 03/11/2021
ms.topic: article
ms.openlocfilehash: f9088893f22637dfb3ebc118ed7093ecdc300d7e
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104767058"
---
# <a name="apache-kafka-developer-guide-for-azure-event-hubs"></a>针对 Azure 事件中心的 Apache Kafka 开发人员指南
本文提供相关文章的链接，这些文章介绍了如何将 Apache Kafka 应用程序与 Azure 事件中心集成。 

## <a name="overview"></a>概述
事件中心提供 Kafka 终结点，现有的基于 Kafka 的应用程序可将该终结点用作运行你自己的 Kafka 群集的替代方案。 事件中心可与许多现有 Kafka 应用程序配合使用。

## <a name="quickstarts"></a>快速入门
你可以在 GitHub 和此内容集中找到各种快速入门文章，快速熟悉用于 Kafka 的事件中心。

### <a name="quickstarts-in-github"></a>GitHub 中的快速入门
请参阅 **azure-event-hubs-for-kafka** 存储库中的以下快速入门教程： 

| 客户端语言/框架 | 说明 | 
| ------------------------- | ----------- | 
| [.NET](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/dotnet) | <p>本快速入门介绍如何使用通过 .Net Core 2.0 以 C# 编写的示例创建者和使用者来创建和连接事件中心 Kafka 终结点。</p><p>此示例基于 [Confluent 的 Apache Kafka .NET 客户端](https://github.com/confluentinc/confluent-kafka-dotnet)，经过修改后可与用于 Kafka 的事件中心配合使用。</p> | 
| [Java](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java) | 本快速入门介绍如何使用以 Java 编写的示例创建者和使用者来创建和连接事件中心 Kafka 终结点。 |
| [Node.js](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/node) | <p>本快速入门介绍如何使用以 Node 编写的示例创建者和使用者来创建和连接事件中心 Kafka 终结点。</p><p>此示例使用 [node-rdkafka](https://github.com/Blizzard/node-rdkafka) 库。 </p>| 
| [Python](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/python) | <p>本快速入门介绍如何使用以 Python 编写的示例创建者和使用者来创建和连接事件中心 Kafka 终结点。</p><p>此示例基于 [Confluent 的 Apache Kafka Python 客户端](https://github.com/confluentinc/confluent-kafka-python)，经过修改后可与用于 Kafka 的事件中心配合使用。</p>|
| [Go](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/go) | <p>本快速入门介绍如何使用以 Go 编写的示例创建者和使用者来创建和连接事件中心 Kafka 终结点。</p><p>此示例基于 [Confluent 的 Apache Kafka Golang 客户端](https://github.com/confluentinc/confluent-kafka-go)，经过修改后可与用于 Kafka 的事件中心配合使用。</p>| 
| [Sarama kafka Go](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/go-sarama-client) | 本快速入门介绍如何使用通过 [Sarama Kafka 客户端](https://github.com/Shopify/sarama)库以 Go 编写的示例创建者和使用者来创建和连接事件中心 Kafka 终结点。 |
| [Kafka](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/kafka-cli) | 本快速入门介绍如何使用 Apache Kafka 发行版附带的 CLI 来创建和连接事件中心 Kafka 终结点。| 
| [Kafkacat](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/kafkacat) | kafkacat 是一个非 JVM 的命令行使用者和创建者。它基于 librdkafka，因其速度快且资源占用量小而广受欢迎。 本快速入门包含一个示例配置和几个简单的示例 kafkacat 命令。 | 
 
 <!--
### Quickstarts in DOCS
See the quickstart: [Data streaming with Event Hubs using the Kafka protocol](event-hubs-quickstart-kafka-enabled-event-hubs.md) in this content set, which provides step-by-step instructions on how to stream into Event Hubs. You learn how to use your producers and consumers to talk to Event Hubs with just a configuration change in your applications. 
-->

## <a name="tutorials"></a>教程 

### <a name="tutorials-in-github"></a>GitHub 中的教程
请参阅 GitHub 中的以下教程：

| 教程 | 说明 | 
| ------------------------- | ----------- | 
| [Akka](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java) | 本教程演示如何在不更改协议客户端或运行自己的群集的情况下，将 Akka Streams 连接到已启用 Kafka 的事件中心。 有两个单独的使用 **Java** 和 **Scala** 编程语言的教程。 | 
| [“连接”](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect) | 本文档将详细介绍如何将 Kafka Connect 与 Azure 事件中心集成，以及如何部署基本的 FileStreamSource 和 FileStreamSink 连接器。 虽然这些连接器不是用于生产的，但它们可以用于演示端到端 Kafka Connect 方案，让 Azure 事件中心冒充 Kafka 中转站。| 
| [Filebeat](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/filebeat) | 本文档详述如何通过 Filebeat 的 Kafka 输出来集成 Filebeat 和事件中心。 | 
| [Flink](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/flink) | 本教程将演示如何在不更改协议客户端或运行自己的群集的情况下，将 Apache Flink 连接到已启用 Kafka 的事件中心。 | 
| [FluentD](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/fluentd) | 本文档详述如何使用 Fluentd 的 `out_kafka` 输出插件来集成 Fluentd 和事件中心。 |
| [互操作](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/interop) | 本教程介绍如何在使用不同协议的使用者与创建者之间交换事件。 |
| [Logstash](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/logstash) | 本教程详述如何将 Logstash 与使用 Logstash Kafka 输入/输出插件且支持 Kafka 的事件中心集成。 | 
| [MirrorMaker](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker) | 本教程介绍事件中心和 Kafka MirrorMaker 如何通过在事件中心服务中镜像 Kafka 输入流将现有 Kafka 管道集成到 Azure 中。 |
| [NiFi](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/nifi) | 本教程演示如何将 Apache NiFi 连接到事件中心命名空间。 | 
| [OAuth](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth) | 这两个快速入门介绍如何使用以 Go 和 Java 编程语言编写的示例创建者和使用者来创建和连接事件中心 Kafka 终结点。 |
| [Confluent 的架构注册表](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/schema-registry) | 本教程详述如何将架构注册表与用于 Kafka 的事件中心集成。 | 
| [Spark](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/spark) | 本教程将演示如何在不更改你的协议客户端或运行你自己的 Kafka 群集的情况下，将 Spark 应用程序连接到事件中心。 | 

<!--
### Tutorials in DOCS
Also, see the tutorial: [Process Apache Kafka for Event Hubs events using Stream analytics](event-hubs-kafka-stream-analytics.md) in this content set, which shows how to stream data into Event Hubs and process it with Azure Stream Analytics.

## How-to guides
See the following How-to guides in our documentation:

| Article | Description | 
| ------- | ----------- | 
| [Mirror a Kafka broker in an event hub](event-hubs-kafka-mirror-maker-tutorial.md) | Shows how to mirror a Kafka broker in an event hub using Kafka MirrorMaker. |
| [Connect Apache Spark to an event hub](event-hubs-kafka-spark-tutorial.md) | Walks you through connecting your Spark application to Event Hubs for real-time streaming. |
| [Connect Apache Flink to an event hub](event-hubs-kafka-flink-tutorial.md) | Shows you how to connect Apache Flink to an event hub without changing your protocol clients or running your own clusters. |
| [Integrate Apache Kafka Connect with a event hub (Preview)](event-hubs-kafka-connect-tutorial.md) | Walks you through integrating Kafka Connect with an event hub and deploying basic FileStreamSource and FileStreamSink connectors. |
| [Connect Akka Streams to an event hub](event-hubs-kafka-akka-streams-tutorial.md) | Shows you how to connect Akka Streams to an event hub without changing your protocol clients or running your own clusters. |
| [Use the Spring Boot Starter for Apache Kafka with Azure Event Hubs](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub) | Demonstrates how to configure a Java-based Spring Cloud Stream Binder created with the Spring Boot Initializer to use Apache Kafka with Azure Event Hubs. |
-->
## <a name="next-steps"></a>后续步骤
请查看 GitHub 存储库 [azure-event-hubs-for-kafka](https://github.com/Azure/azure-event-hubs-for-kafka) 中 quickstart 和 tutorials 文件夹下的示例。

另请参阅以下文章：

- [针对事件中心的 Apache Kafka 故障排除指南](apache-kafka-troubleshooting-guide.md)
- [常见问题解答 - 用于 Apache Kafka 的事件中心](apache-kafka-frequently-asked-questions.md)
- [针对事件中心的 Apache Kafka 迁移指南](apache-kafka-migration-guide.md)



