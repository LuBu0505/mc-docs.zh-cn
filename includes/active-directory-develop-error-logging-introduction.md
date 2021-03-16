---
author: mmacy
ms.author: v-junlch
ms.date: 02/22/2021
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: a2abe15353103985290f45287b5fa04dcd74ff8b
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751880"
---
Microsoft 身份验证库 (MSAL) 应用生成日志消息，这些消息可以用来诊断问题。 应用可以通过数行代码配置日志记录，并可对详细程度以及是否记录个人和组织数据进行自定义控制。 建议创建 MSAL 日志记录回调，并提供一种方式来让用户在遇到身份验证问题时提交日志。

## <a name="logging-levels"></a>日志记录级别

MSAL 提供多个日志记录详细级别：

- 错误：指示出现问题并已生成错误。 用于调试并确定问题。
- 警告：不一定会出现错误或故障，只是为了诊断和指出问题。
- 信息：MSAL 将要记录的事件可为用户提供信息，不一定用于调试。
- 详细：默认。 MSAL 将记录库行为的完整详细信息。

## <a name="personal-and-organizational-data"></a>个人和组织数据

默认情况下，MSAL 记录器不捕获任何高度敏感的个人或组织数据。 该库提供相关选项，允许你自行决定是否记录个人和组织数据。

以下各节将详细介绍应用程序的 MSAL 错误日志记录。
