---
title: mv-expand 运算符 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的 mv-expand 运算符。
services: data-explorer
author: orspod
ms.author: v-junlch
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
ms.date: 03/18/2021
ms.localizationpriority: high
ms.openlocfilehash: baad76cfc0f2cbeb10ca991ca52c53ff786a8d5a
ms.sourcegitcommit: 8b3a588ef0949efc5b0cfb5285c8191ce5b05651
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104766609"
---
# <a name="mv-expand-operator"></a>mv-expand 运算符

将多值动态数组或属性包扩展为多个记录。

`mv-expand` 可以说与聚合运算符相反，聚合运算符是将多个值打包成单个[动态](./scalar-data-types/dynamic.md)类型化数组或属性包，如 `summarize` ... `make-list()` 和 `make-series`。
（标量）数组或属性包中的每个元素都在运算符的输出中生成一个新记录。 输入中未扩展的所有列都将复制到输出中的所有记录中。

## <a name="syntax"></a>语法

*T* `| mv-expand ` [`bagexpansion=`(`bag` | `array`)] [`with_itemindex=`*IndexColumnName*] *ColumnName* [`to typeof(` *Typename*`)`] [`,` *ColumnName* ...] [`limit` *Rowlimit*]

*T* `| mv-expand ` [`bagexpansion=`(`bag` | `array`)] *Name* `=` *ArrayExpression* [`to typeof(`*Typename*`)`] [, [*Name* `=`] *ArrayExpression* [`to typeof(`*Typename*`)`] ...] [`limit` *Rowlimit*]

## <a name="arguments"></a>参数

* *ColumnName*、*ArrayExpression*：一个列引用或一个标量表达式，含 `dynamic` 类型的值，带有数组或属性包。 数组或属性包的各个顶级元素扩展为多个记录。<br>
  如果使用“ArrayExpression”且“Name”不等于任何输入列名称时，扩展的值将扩展为输出中的新列。
  否则，将替换现有的 ColumnName。

* *Name*：新列的名称。

* Typename：指示数组元素的基础类型，该类型将成为 `mv-expand` 运算符生成的列的类型。 应用类型的操作仅限强制转换，不包括分析或类型转换。 不符合声明类型的数组元素将成为 `null` 值。

* *RowLimit*：从每个原始行生成的最大行数。 默认值为 2147483647。 

  > [!NOTE]
  > `mvexpand` 是 `mv-expand` 运算符的已过时的旧形式。 旧版本的默认行限制为 128 行。

* *IndexColumnName：* 如果指定了 `with_itemindex`，输出将包括另一列（名为 IndexColumnName），其中包含最初扩展的集合中的项的索引（从 0 开始）。 

## <a name="returns"></a>返回

对于输入中的每个记录，运算符会在输出中返回零个、一个或多个记录，具体通过以下方式确定：

1. 未扩展的输入列在输出中显示其原始值。
   如果单个输入记录扩展为多个输出记录，会将其值复制到所有记录中。

1. 对于扩展的每个 ColumnName 或 ArrayExpression，则根据[以下](#modes-of-expansion)所述方式确定每个值的输出记录数量。 对于每个输入记录，将计算输出记录的最大数量。 将“并行”扩展所有数组或属性包，以便将缺少的值（如果有）替换为 null 值。

1. 如果动态值为 null，则为该值 (null) 生成单个记录。
   如果动态值为空数组或属性包，则不会为该值生成任何记录。
   否则，将生成多个记录，因为动态值中有元素。

扩展的列的类型为 `dynamic`，除非使用 `to typeof()` 子句显式指定其类型。

### <a name="modes-of-expansion"></a>扩展模式

支持两种模式的属性包扩展：

* `bagexpansion=bag` 或 `kind=bag`：将属性包扩展为单个条目属性包。 此模式是默认模式。
* `bagexpansion=array` 或 `kind=array`：将属性包扩展为双元素 `[`*key*`,`*value*`]` 数组结构，可允许统一访问键和值。 使用此模式还可以（例如）对属性名称运行非重复计数聚合。 

## <a name="examples"></a>示例

### <a name="single-column"></a>单个列

单个列的简单展开：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
 ```kusto
datatable (a:int, b:dynamic)[1,dynamic({"prop1":"a", "prop2":"b"})]
| mv-expand b 
```

|a|b|
|---|---|
|1|{"prop1":"a"}|
|1|{"prop2":"b"}|

### <a name="zipped-two-columns"></a>压缩的两个列

展开两个列时，将首先“压缩”适用的列，然后将其展开：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
datatable (a:int, b:dynamic, c:dynamic)[1,dynamic({"prop1":"a", "prop2":"b"}), dynamic([5, 4, 3])]
| mv-expand b, c
```

|a|b|c|
|---|---|---|
|1|{"prop1":"a"}|5|
|1|{"prop2":"b"}|4|
|1||3|

### <a name="cartesian-product-of-two-columns"></a>两个列的笛卡尔乘积

如果要在展开两列时获取笛卡尔乘积，请将其逐个展开：

<!-- csl: https://kuskusdfv3.kusto.chinacloudapi.cn/Kuskus -->
```kusto
datatable (a:int, b:dynamic, c:dynamic)
  [
  1,
  dynamic({"prop1":"a", "prop2":"b"}),
  dynamic([5, 6])
  ]
| mv-expand b
| mv-expand c
```

|a|b|c|
|---|---|---|
|1|{  "prop1": "a"}|5|
|1|{  "prop1": "a"}|6|
|1|{  "prop2": "b"}|5|
|1|{  "prop2": "b"}|6|

### <a name="convert-output"></a>转换输出

若要将 mv-expand 的输出强制为某个特定类型（默认为动态），请使用 `to typeof`：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
datatable (a:string, b:dynamic, c:dynamic)["Constant", dynamic([1,2,3,4]), dynamic([6,7,8,9])]
| mv-expand b, c to typeof(int)
| getschema 
```

ColumnName|ColumnOrdinal|DateType|ColumnType
-|-|-|-
a|0|System.String|string
b|1|System.Object|动态
c|2|System.Int32|int

请注意 `b` 列以 `dynamic` 的形式返回，而 `c` 以 `int` 的形式返回。

### <a name="using-with_itemindex"></a>使用 with_itemindex

通过 `with_itemindex` 展开数组：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
range x from 1 to 4 step 1
| summarize x = make_list(x)
| mv-expand with_itemindex=Index x
```

|x|索引|
|---|---|
|1|0|
|2|1|
|3|2|
|4|3|

## <a name="see-also"></a>另请参阅

* 如需更多示例，请参阅[为一段时间内的实时活动计数绘制图表](./samples.md#chart-concurrent-sessions-over-time)。
* [mv-apply](./mv-applyoperator.md) 运算符。
* [summarize make_list()](makelist-aggfunction.md)，这是 mv-expand 的反向函数。
* [bag_unpack()](bag-unpackplugin.md) 插件，用于将动态 JSON 对象扩展为使用属性包键的列。
