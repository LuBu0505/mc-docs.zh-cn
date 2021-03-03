---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/26/2021
ms.author: v-junlch
ms.openlocfilehash: d182dff8cfd941575ce2512e90424402021b14ef
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697501"
---
现在可以使用新的 `msg` 参数，从函数代码写入到输出绑定。 将以下代码行添加到成功响应之前，以便将 `name` 的值添加到 `msg` 输出绑定。

```java
msg.setValue(name);
```

使用输出绑定时，无需使用 Azure 存储 SDK 代码进行身份验证、获取队列引用或写入数据。 Functions 运行时和队列输出绑定将为你执行这些任务。

`run` 方法现在应如以下示例所示：

```java
    @FunctionName("HttpExample")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request, 
            @QueueOutput(name = "msg", queueName = "outqueue", 
            connection = "AzureWebJobsStorage") OutputBinding<String> msg, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
            .body("Please pass a name on the query string or in the request body").build();
        } else {
            // Write the name to the message queue. 
            msg.setValue(name);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        }
    }
```
