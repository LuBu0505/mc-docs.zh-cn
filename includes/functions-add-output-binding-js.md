---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/26/2021
ms.author: v-junlch
ms.openlocfilehash: fe80f3ac2996bccf665230c47cf27c2d6c59b79c
ms.sourcegitcommit: 3f32b8672146cb08fdd94bf6af015cb08c80c390
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/03/2021
ms.locfileid: "101697503"
---
添加在 `context.bindings` 上使用 `msg` 输出绑定对象来创建队列消息的代码。 请在 `context.res` 语句之前添加此代码。

```javascript
        // Add a message to the Storage queue,
        // which is the name passed to the function.
        context.bindings.msg = (req.query.name || req.body.name);
```

此时，你的函数应如下所示：

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        // Add a message to the Storage queue,
        // which is the name passed to the function.
        context.bindings.msg = (req.query.name || req.body.name);
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```
