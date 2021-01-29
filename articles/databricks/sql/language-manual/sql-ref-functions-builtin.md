---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 内置函数 - Azure Databricks
description: 了解 SQL Analytics 内置函数。
ms.openlocfilehash: 861f2bb0e62510bbfe254e839f309ce2e5fc6c02
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692207"
---
# <a name="built-in-functions"></a>内置函数

## <a name="aggregate-functions"></a>聚合函数

| 功能                                                      | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| any(expr)                                                     | 如果至少一个 ``expr`` 值为 true，则返回 true。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| approx_count_distinct(expr[,relativeSD])                      | 返回 HyperLogLog++ 估算的基数。 ``relativeSD`` 定义允许的最大估算错误。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| approx_percentile(col,percentage[,accuracy])                  | 按给定百分比返回数值列 ``col`` 的近似百分位值。 百分比的值必须介于 0.0 到 1.0 之间。 ``accuracy`` 参数（默认值：10000）是一个正值文本，它以内存为代价控制近似精度。 ``accuracy`` 的值越高，精度越高。``1.0/accuracy`` 是近似计算的相对错误。 当 ``percentage`` 为数组时，百分比数组的每个值必须介于 0.0 到 1.0 之间。 在这种情况下，会按给定百分比数组返回列 ``col`` 的大致百分位数组。 |
| avg(expr)                                                     | 返回从组的值计算出的平均值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| bit_or(expr)                                                  | 返回所有非 null 输入值的位或，如果没有输入值，则返回 null。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| bit_xor(expr)                                                 | 返回所有非 null 输入值的位异或，如果没有输入值，则返回 null。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| bool_and(expr)                                                | 如果所有 ``expr`` 值均为 true，则返回 true。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| bool_or(expr)                                                 | 如果至少一个 ``expr`` 值为 true，则返回 true。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| collect_list(expr)                                            | 收集并返回非唯一元素的列表。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| collect_set(expr)                                             | 收集并返回一组唯一元素。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| corr(expr1,expr2)                                             | 返回表示一组数字对之间的关联情况的皮尔逊系数。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| count(*)                                                      | 返回检索到的行的总数，包括那些包含 null 的行。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| count(expr[,expr…])                                           | 返回为其提供的表达式均为非 null 值的行的数目。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| count(DISTINCT expr[,expr…])                                  | 返回为其提供的表达式为唯一的非 null 值的行的数目。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| count_if(expr)                                                | 返回表达式的 ``TRUE`` 值的数目。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| count_min_sketch(col,eps,confidence,seed)                     | 返回具有给定 esp、置信度和种子的列的 Count-min sketch。 结果是一个字节数组，可以在使用之前将其反序列化为 ``CountMinSketch``。 Count-min sketch 是一个概率数据结构，用于通过子线性空间进行基数估算。                                                                                                                                                                                                                                                                                                                        |
| covar_pop(expr1,expr2)                                        | 返回一组数字对的总体协方差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| covar_samp(expr1,expr2)                                       | 返回一组数字对的样本协方差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| every(expr)                                                   | 如果所有 ``expr`` 值均为 true，则返回 true。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| first(expr[,isIgnoreNull])                                    | 返回一组行的第一个 ``expr`` 值。 如果 ``isIgnoreNull`` 为 true，则仅返回非 null 值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| first_value(expr[,isIgnoreNull])                              | 返回一组行的第一个 ``expr`` 值。 如果 ``isIgnoreNull`` 为 true，则仅返回非 null 值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| kurtosis(expr)                                                | 返回从组的值计算出的峰度值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| last(expr[,isIgnoreNull])                                     | 返回一组行的最后一个 ``expr`` 值。 如果 ``isIgnoreNull`` 为 true，则仅返回非 null 值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| last_value(expr[,isIgnoreNull])                               | 返回一组行的最后一个 ``expr`` 值。 如果 ``isIgnoreNull`` 为 true，则仅返回非 null 值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| max(expr)                                                     | 返回 ``expr`` 的最大值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| max_by(x,y)                                                   | 返回与 ``y`` 的最大值关联的 ``x`` 的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| mean(expr)                                                    | 返回从组的值计算出的平均值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| min(expr)                                                     | 返回 ``expr`` 的最小值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| min_by(x,y)                                                   | 返回与 ``y`` 的最小值关联的 ``x`` 的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| percentile(col,percentage[,frequency])                        | 按给定百分比返回数值列 ``col`` 的确切百分位值。 百分比的值必须介于 0.0 到 1.0 之间。 频率的值应为正整数。                                                                                                                                                                                                                                                                                                                                                                                                                     |
| percentile(col,array(percentage1[,percentage2]…)[,frequency]) | 按给定百分比返回数值列 ``col`` 的确切百分位值数组。 百分比数组的每个值必须介于 0.0 到 1.0 之间。 频率的值应为正整数。                                                                                                                                                                                                                                                                                                                                                                                                 |
| percentile_approx(col,percentage[,accuracy])                  | 按给定百分比返回数值列 ``col`` 的近似百分位值。 百分比的值必须介于 0.0 到 1.0 之间。 ``accuracy`` 参数（默认值：10000）是一个正值文本，它以内存为代价控制近似精度。 ``accuracy`` 的值越高，精度越高。``1.0/accuracy`` 是近似计算的相对错误。 当 ``percentage`` 为数组时，百分比数组的每个值必须介于 0.0 到 1.0 之间。 在这种情况下，会按给定百分比数组返回列 ``col`` 的大致百分位数组。 |
| skewness(expr)                                                | 返回从组的值计算出的偏度值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| some(expr)                                                    | 如果至少一个 ``expr`` 值为 true，则返回 true。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| std(expr)                                                     | 返回从组的值计算出的样本标准偏差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| stddev(expr)                                                  | 返回从组的值计算出的样本标准偏差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| stddev_pop(expr)                                              | 返回从组的值计算出的总体标准偏差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| stddev_samp(expr)                                             | 返回从组的值计算出的样本标准偏差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| sum(expr)                                                     | 返回从组的值计算出的总和值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| var_pop(expr)                                                 | 返回从组的值计算出的总体方差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| var_samp(expr)                                                | 返回从组的值计算出的样本方差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| variance(expr)                                                | 返回从组的值计算出的样本方差。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

### <a name="examples"></a>示例

```
-- any
SELECT any(col) FROM VALUES (true), (false), (false) AS tab(col);
+--------+
|any(col)|
+--------+
|    true|
+--------+

SELECT any(col) FROM VALUES (NULL), (true), (false) AS tab(col);
+--------+
|any(col)|
+--------+
|    true|
+--------+

SELECT any(col) FROM VALUES (false), (false), (NULL) AS tab(col);
+--------+
|any(col)|
+--------+
|   false|
+--------+

-- approx_count_distinct
SELECT approx_count_distinct(col1) FROM VALUES (1), (1), (2), (2), (3) tab(col1);
+---------------------------+
|approx_count_distinct(col1)|
+---------------------------+
|                          3|
+---------------------------+

-- approx_percentile
SELECT approx_percentile(10.0, array(0.5, 0.4, 0.1), 100);
+--------------------------------------------------+
|approx_percentile(10.0, array(0.5, 0.4, 0.1), 100)|
+--------------------------------------------------+
|                                [10.0, 10.0, 10.0]|
+--------------------------------------------------+

SELECT approx_percentile(10.0, 0.5, 100);
+-------------------------------------------------+
|approx_percentile(10.0, CAST(0.5 AS DOUBLE), 100)|
+-------------------------------------------------+
|                                             10.0|
+-------------------------------------------------+

-- avg
SELECT avg(col) FROM VALUES (1), (2), (3) AS tab(col);
+--------+
|avg(col)|
+--------+
|     2.0|
+--------+

SELECT avg(col) FROM VALUES (1), (2), (NULL) AS tab(col);
+--------+
|avg(col)|
+--------+
|     1.5|
+--------+

-- bit_or
SELECT bit_or(col) FROM VALUES (3), (5) AS tab(col);
+-----------+
|bit_or(col)|
+-----------+
|          7|
+-----------+

-- bit_xor
SELECT bit_xor(col) FROM VALUES (3), (5) AS tab(col);
+------------+
|bit_xor(col)|
+------------+
|           6|
+------------+

-- bool_and
SELECT bool_and(col) FROM VALUES (true), (true), (true) AS tab(col);
+-------------+
|bool_and(col)|
+-------------+
|         true|
+-------------+

SELECT bool_and(col) FROM VALUES (NULL), (true), (true) AS tab(col);
+-------------+
|bool_and(col)|
+-------------+
|         true|
+-------------+

SELECT bool_and(col) FROM VALUES (true), (false), (true) AS tab(col);
+-------------+
|bool_and(col)|
+-------------+
|        false|
+-------------+

-- bool_or
SELECT bool_or(col) FROM VALUES (true), (false), (false) AS tab(col);
+------------+
|bool_or(col)|
+------------+
|        true|
+------------+

SELECT bool_or(col) FROM VALUES (NULL), (true), (false) AS tab(col);
+------------+
|bool_or(col)|
+------------+
|        true|
+------------+

SELECT bool_or(col) FROM VALUES (false), (false), (NULL) AS tab(col);
+------------+
|bool_or(col)|
+------------+
|       false|
+------------+

-- collect_list
SELECT collect_list(col) FROM VALUES (1), (2), (1) AS tab(col);
+-----------------+
|collect_list(col)|
+-----------------+
|        [1, 2, 1]|
+-----------------+

-- collect_set
SELECT collect_set(col) FROM VALUES (1), (2), (1) AS tab(col);
+----------------+
|collect_set(col)|
+----------------+
|          [1, 2]|
+----------------+

-- corr
SELECT corr(c1, c2) FROM VALUES (3, 2), (3, 3), (6, 4) as tab(c1, c2);
+--------------------------------------------+
|corr(CAST(c1 AS DOUBLE), CAST(c2 AS DOUBLE))|
+--------------------------------------------+
|                          0.8660254037844387|
+--------------------------------------------+

-- count
SELECT count(*) FROM VALUES (NULL), (5), (5), (20) AS tab(col);
+--------+
|count(1)|
+--------+
|       4|
+--------+

SELECT count(col) FROM VALUES (NULL), (5), (5), (20) AS tab(col);
+----------+
|count(col)|
+----------+
|         3|
+----------+

SELECT count(DISTINCT col) FROM VALUES (NULL), (5), (5), (10) AS tab(col);
+-------------------+
|count(DISTINCT col)|
+-------------------+
|                  2|
+-------------------+

-- count_if
SELECT count_if(col % 2 = 0) FROM VALUES (NULL), (0), (1), (2), (3) AS tab(col);
+-------------------------+
|count_if(((col % 2) = 0))|
+-------------------------+
|                        2|
+-------------------------+

SELECT count_if(col IS NULL) FROM VALUES (NULL), (0), (1), (2), (3) AS tab(col);
+-----------------------+
|count_if((col IS NULL))|
+-----------------------+
|                      1|
+-----------------------+

-- covar_pop
SELECT covar_pop(c1, c2) FROM VALUES (1,1), (2,2), (3,3) AS tab(c1, c2);
+-------------------------------------------------+
|covar_pop(CAST(c1 AS DOUBLE), CAST(c2 AS DOUBLE))|
+-------------------------------------------------+
|                               0.6666666666666666|
+-------------------------------------------------+

-- covar_samp
SELECT covar_samp(c1, c2) FROM VALUES (1,1), (2,2), (3,3) AS tab(c1, c2);
+--------------------------------------------------+
|covar_samp(CAST(c1 AS DOUBLE), CAST(c2 AS DOUBLE))|
+--------------------------------------------------+
|                                               1.0|
+--------------------------------------------------+

-- every
SELECT every(col) FROM VALUES (true), (true), (true) AS tab(col);
+----------+
|every(col)|
+----------+
|      true|
+----------+

SELECT every(col) FROM VALUES (NULL), (true), (true) AS tab(col);
+----------+
|every(col)|
+----------+
|      true|
+----------+

SELECT every(col) FROM VALUES (true), (false), (true) AS tab(col);
+----------+
|every(col)|
+----------+
|     false|
+----------+

-- first
SELECT first(col) FROM VALUES (10), (5), (20) AS tab(col);
+-----------------+
|first(col, false)|
+-----------------+
|               10|
+-----------------+

SELECT first(col) FROM VALUES (NULL), (5), (20) AS tab(col);
+-----------------+
|first(col, false)|
+-----------------+
|             null|
+-----------------+

SELECT first(col, true) FROM VALUES (NULL), (5), (20) AS tab(col);
+----------------+
|first(col, true)|
+----------------+
|               5|
+----------------+

-- first_value
SELECT first_value(col) FROM VALUES (10), (5), (20) AS tab(col);
+-----------------------+
|first_value(col, false)|
+-----------------------+
|                     10|
+-----------------------+

SELECT first_value(col) FROM VALUES (NULL), (5), (20) AS tab(col);
+-----------------------+
|first_value(col, false)|
+-----------------------+
|                   null|
+-----------------------+

SELECT first_value(col, true) FROM VALUES (NULL), (5), (20) AS tab(col);
+----------------------+
|first_value(col, true)|
+----------------------+
|                     5|
+----------------------+

-- kurtosis
SELECT kurtosis(col) FROM VALUES (-10), (-20), (100), (1000) AS tab(col);
+-----------------------------+
|kurtosis(CAST(col AS DOUBLE))|
+-----------------------------+
|          -0.7014368047529618|
+-----------------------------+

SELECT kurtosis(col) FROM VALUES (1), (10), (100), (10), (1) as tab(col);
+-----------------------------+
|kurtosis(CAST(col AS DOUBLE))|
+-----------------------------+
|          0.19432323191698986|
+-----------------------------+

-- last
SELECT last(col) FROM VALUES (10), (5), (20) AS tab(col);
+----------------+
|last(col, false)|
+----------------+
|              20|
+----------------+

SELECT last(col) FROM VALUES (10), (5), (NULL) AS tab(col);
+----------------+
|last(col, false)|
+----------------+
|            null|
+----------------+

SELECT last(col, true) FROM VALUES (10), (5), (NULL) AS tab(col);
+---------------+
|last(col, true)|
+---------------+
|              5|
+---------------+

-- last_value
SELECT last_value(col) FROM VALUES (10), (5), (20) AS tab(col);
+----------------------+
|last_value(col, false)|
+----------------------+
|                    20|
+----------------------+

SELECT last_value(col) FROM VALUES (10), (5), (NULL) AS tab(col);
+----------------------+
|last_value(col, false)|
+----------------------+
|                  null|
+----------------------+

SELECT last_value(col, true) FROM VALUES (10), (5), (NULL) AS tab(col);
+---------------------+
|last_value(col, true)|
+---------------------+
|                    5|
+---------------------+

-- max
SELECT max(col) FROM VALUES (10), (50), (20) AS tab(col);
+--------+
|max(col)|
+--------+
|      50|
+--------+

-- max_by
SELECT max_by(x, y) FROM VALUES (('a', 10)), (('b', 50)), (('c', 20)) AS tab(x, y);
+------------+
|max_by(x, y)|
+------------+
|           b|
+------------+

-- mean
SELECT mean(col) FROM VALUES (1), (2), (3) AS tab(col);
+---------+
|mean(col)|
+---------+
|      2.0|
+---------+

SELECT mean(col) FROM VALUES (1), (2), (NULL) AS tab(col);
+---------+
|mean(col)|
+---------+
|      1.5|
+---------+

-- min
SELECT min(col) FROM VALUES (10), (-1), (20) AS tab(col);
+--------+
|min(col)|
+--------+
|      -1|
+--------+

-- min_by
SELECT min_by(x, y) FROM VALUES (('a', 10)), (('b', 50)), (('c', 20)) AS tab(x, y);
+------------+
|min_by(x, y)|
+------------+
|           a|
+------------+

-- percentile
SELECT percentile(col, 0.3) FROM VALUES (0), (10) AS tab(col);
+---------------------------------------+
|percentile(col, CAST(0.3 AS DOUBLE), 1)|
+---------------------------------------+
|                                    3.0|
+---------------------------------------+

SELECT percentile(col, array(0.25, 0.75)) FROM VALUES (0), (10) AS tab(col);
+-------------------------------------+
|percentile(col, array(0.25, 0.75), 1)|
+-------------------------------------+
|                           [2.5, 7.5]|
+-------------------------------------+

-- percentile_approx
SELECT percentile_approx(10.0, array(0.5, 0.4, 0.1), 100);
+--------------------------------------------------+
|percentile_approx(10.0, array(0.5, 0.4, 0.1), 100)|
+--------------------------------------------------+
|                                [10.0, 10.0, 10.0]|
+--------------------------------------------------+

SELECT percentile_approx(10.0, 0.5, 100);
+-------------------------------------------------+
|percentile_approx(10.0, CAST(0.5 AS DOUBLE), 100)|
+-------------------------------------------------+
|                                             10.0|
+-------------------------------------------------+

-- skewness
SELECT skewness(col) FROM VALUES (-10), (-20), (100), (1000) AS tab(col);
+-----------------------------+
|skewness(CAST(col AS DOUBLE))|
+-----------------------------+
|           1.1135657469022013|
+-----------------------------+

SELECT skewness(col) FROM VALUES (-1000), (-100), (10), (20) AS tab(col);
+-----------------------------+
|skewness(CAST(col AS DOUBLE))|
+-----------------------------+
|          -1.1135657469022011|
+-----------------------------+

-- some
SELECT some(col) FROM VALUES (true), (false), (false) AS tab(col);
+---------+
|some(col)|
+---------+
|     true|
+---------+

SELECT some(col) FROM VALUES (NULL), (true), (false) AS tab(col);
+---------+
|some(col)|
+---------+
|     true|
+---------+

SELECT some(col) FROM VALUES (false), (false), (NULL) AS tab(col);
+---------+
|some(col)|
+---------+
|    false|
+---------+

-- std
SELECT std(col) FROM VALUES (1), (2), (3) AS tab(col);
+------------------------+
|std(CAST(col AS DOUBLE))|
+------------------------+
|                     1.0|
+------------------------+

-- stddev
SELECT stddev(col) FROM VALUES (1), (2), (3) AS tab(col);
+---------------------------+
|stddev(CAST(col AS DOUBLE))|
+---------------------------+
|                        1.0|
+---------------------------+

-- stddev_pop
SELECT stddev_pop(col) FROM VALUES (1), (2), (3) AS tab(col);
+-------------------------------+
|stddev_pop(CAST(col AS DOUBLE))|
+-------------------------------+
|              0.816496580927726|
+-------------------------------+

-- stddev_samp
SELECT stddev_samp(col) FROM VALUES (1), (2), (3) AS tab(col);
+--------------------------------+
|stddev_samp(CAST(col AS DOUBLE))|
+--------------------------------+
|                             1.0|
+--------------------------------+

-- sum
SELECT sum(col) FROM VALUES (5), (10), (15) AS tab(col);
+--------+
|sum(col)|
+--------+
|      30|
+--------+

SELECT sum(col) FROM VALUES (NULL), (10), (15) AS tab(col);
+--------+
|sum(col)|
+--------+
|      25|
+--------+

SELECT sum(col) FROM VALUES (NULL), (NULL) AS tab(col);
+--------+
|sum(col)|
+--------+
|    null|
+--------+

-- var_pop
SELECT var_pop(col) FROM VALUES (1), (2), (3) AS tab(col);
+----------------------------+
|var_pop(CAST(col AS DOUBLE))|
+----------------------------+
|          0.6666666666666666|
+----------------------------+

-- var_samp
SELECT var_samp(col) FROM VALUES (1), (2), (3) AS tab(col);
+-----------------------------+
|var_samp(CAST(col AS DOUBLE))|
+-----------------------------+
|                          1.0|
+-----------------------------+

-- variance
SELECT variance(col) FROM VALUES (1), (2), (3) AS tab(col);
+-----------------------------+
|variance(CAST(col AS DOUBLE))|
+-----------------------------+
|                          1.0|
+-----------------------------+
```

## <a name="window-functions"></a>开窗函数

| 功能                                           | 说明                                                                                                                                                                                                                                                                                                                                                                                                       |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| cume_dist()                                        | 计算某个值相对于分区中所有值的位置。                                                                                                                                                                                                                                                                                                                                         |
| dense_rank()                                       | 计算某个值在值组中的排名。 结果是 1 加上之前分配的排名值。 与函数排名不同，dense_rank 不会在排名序列中产生空隙。                                                                                                                                                                                                                 |
| lag(input[,offset[,default]])                      | 返回窗口中当前行之前第 ``offset`` 行的 ``input`` 的值。 ``offset`` 的默认值为 1，``default`` 的默认值为 null。 如果第 ``offset`` 行的 ``input`` 的值为 null，则返回 null。 如果没有此类偏移行（例如，当偏移量为 1 时，窗口的第一行没有任何以前的行），则返回 ``default``。    |
| lead(input[,offset[,default]])                     | 返回窗口中当前行之后第 ``offset`` 行的 ``input`` 的值。 ``offset`` 的默认值为 1，``default`` 的默认值为 null。 如果第 ``offset`` 行的 ``input`` 的值为 null，则返回 null。 如果没有此类偏移行（例如，当偏移量为 1 时，窗口的最后一行没有任何后续行），则返回 ``default``。 |
| ntile(n)                                           | 将每个窗口分区的行分割为从 1 到至多 ``n`` 的 ``n`` 个 Bucket。                                                                                                                                                                                                                                                                                                                    |
| percent_rank()                                     | 计算某个值在一组值中的百分比排名。                                                                                                                                                                                                                                                                                                                                                  |
| rank()                                             | 计算某个值在值组中的排名。 结果是 1 加上前面的行数，或者等于当前行在分区中的顺序。 值将在序列中生成空隙。                                                                                                                                                                                                |
| row_number()                                       | 根据窗口分区中的行顺序，为每一行分配唯一的顺序编号（从 1 开始）。                                                                                                                                                                                                                                                                                |

## <a name="array-functions"></a>数组函数

| 功能                                           | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| array_contains(array,value)                        | 如果数组包含该值，则返回 true。                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| array_distinct(array)                              | 从数组中删除重复值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| array_except(array1,array2)                        | 返回存在于 array1 中但不存在于 array2 中的元素的数组，不包含重复项。                                                                                                                                                                                                                                                                                                                                                                                                                           |
| array_intersect(array1,array2)                     | 返回 array1 和 array2 的交集中的元素的数组，不包含重复项。                                                                                                                                                                                                                                                                                                                                                                                                              |
| array_join(array,delimiter[,nullReplacement])      | 使用分隔符连接给定数组的元素，并使用可选字符串替换数组中的 null 值。 如果没有为 nullReplacement 设置任何值，则会筛选掉任何 null 值。                                                                                                                                                                                                                                                                                                                               |
| array_max(array)                                   | 返回数组中的最大值。 将跳过 NULL 元素。                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| array_min(array)                                   | 返回数组中的最小值。 将跳过 NULL 元素。                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| array_position(array,element)                      | 以长整型返回数组的第一个元素的索引（从 1 开始）。                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| array_remove(array,element)                        | 删除数组中与 element 相等的所有元素。                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| array_repeat(element,count)                        | 返回一个数组，其中包含的 element 重复 count 次。                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| array_union(array1,array2)                         | 返回 array1 和 array2 的并集中的元素的数组，不包含重复项。                                                                                                                                                                                                                                                                                                                                                                                                                     |
| arrays_overlap(a1,a2)                              | 如果 a1 至少包含一个也存在于 a2 中的非 null 元素，则返回 true。 如果数组没有公共元素，均为非空数组，并且其中任何一个包含 null 元素，则返回 null 元素，否则返回 false。                                                                                                                                                                                                                                                                                  |
| arrays_zip(a1,a2,…)                                | 返回合并的结构数组，其中的第 N 个结构包含输入数组的所有的第 N 个值。                                                                                                                                                                                                                                                                                                                                                                                                        |
| concat(col1,col2,…,colN)                           | 返回将 col1、col2、...、colN 串联后的结果。                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| flatten(arrayOfArrays)                             | 将数组的数组转换为单个数组。                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| reverse(array)                                     | 返回一个反向字符串或一个包含逆序的元素的数组。                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| sequence(start,stop,step)                          | 生成一个数组，其中包含从 start 到 stop（含）的元素，这些元素按 step 递增。 返回的元素的类型与参数表达式的类型相同。 支持的类型为：字节、短整型、整型、长整型、日期、时间戳。 start 表达式和 stop 表达式必须解析为相同的类型。 如果 start 表达式和 stop 表达式解析为“日期”或“时间戳”类型，则 step 表达式必须解析为“间隔”类型；否则，step 表达式必须解析为与 start 表达式和 stop 表达式相同的类型。 |
| shuffle(array)                                     | 返回给定数组的随机排列。                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| slice(x,start,length)                              | 返回数组 x 的子集，该子集从索引 start 开始（数组索引从 1 开始，如果 start 为负数，则从索引 end 开始），其长度为指定的长度。                                                                                                                                                                                                                                                                                                                                                              |
| sort_array(array[,ascendingOrder])                 | 根据数组元素的自然顺序，按升序或降序对输入数组排序。 Null 元素将放置在按升序返回的数组的开头，或按降序返回的数组的末尾。                                                                                                                                                                                                                                                 |

### <a name="examples"></a>示例

```
-- array_contains
SELECT array_contains(array(1, 2, 3), 2);
+---------------------------------+
|array_contains(array(1, 2, 3), 2)|
+---------------------------------+
|                             true|
+---------------------------------+

-- array_distinct
SELECT array_distinct(array(1, 2, 3, null, 3));
+----------------------------------------------------+
|array_distinct(array(1, 2, 3, CAST(NULL AS INT), 3))|
+----------------------------------------------------+
|                                          [1, 2, 3,]|
+----------------------------------------------------+

-- array_except
SELECT array_except(array(1, 2, 3), array(1, 3, 5));
+--------------------------------------------+
|array_except(array(1, 2, 3), array(1, 3, 5))|
+--------------------------------------------+
|                                         [2]|
+--------------------------------------------+

-- array_intersect
SELECT array_intersect(array(1, 2, 3), array(1, 3, 5));
+-----------------------------------------------+
|array_intersect(array(1, 2, 3), array(1, 3, 5))|
+-----------------------------------------------+
|                                         [1, 3]|
+-----------------------------------------------+

-- array_join
SELECT array_join(array('hello', 'world'), ' ');
+----------------------------------+
|array_join(array(hello, world),  )|
+----------------------------------+
|                       hello world|
+----------------------------------+

SELECT array_join(array('hello', null ,'world'), ' ');
+--------------------------------------------------------+
|array_join(array(hello, CAST(NULL AS STRING), world),  )|
+--------------------------------------------------------+
|                                             hello world|
+--------------------------------------------------------+

SELECT array_join(array('hello', null ,'world'), ' ', ',');
+-----------------------------------------------------------+
|array_join(array(hello, CAST(NULL AS STRING), world),  , ,)|
+-----------------------------------------------------------+
|                                              hello , world|
+-----------------------------------------------------------+

-- array_max
SELECT array_max(array(1, 20, null, 3));
+---------------------------------------------+
|array_max(array(1, 20, CAST(NULL AS INT), 3))|
+---------------------------------------------+
|                                           20|
+---------------------------------------------+

-- array_min
SELECT array_min(array(1, 20, null, 3));
+---------------------------------------------+
|array_min(array(1, 20, CAST(NULL AS INT), 3))|
+---------------------------------------------+
|                                            1|
+---------------------------------------------+

-- array_position
SELECT array_position(array(3, 2, 1), 1);
+---------------------------------+
|array_position(array(3, 2, 1), 1)|
+---------------------------------+
|                                3|
+---------------------------------+

-- array_remove
SELECT array_remove(array(1, 2, 3, null, 3), 3);
+-----------------------------------------------------+
|array_remove(array(1, 2, 3, CAST(NULL AS INT), 3), 3)|
+-----------------------------------------------------+
|                                              [1, 2,]|
+-----------------------------------------------------+

-- array_repeat
SELECT array_repeat('123', 2);
+--------------------+
|array_repeat(123, 2)|
+--------------------+
|          [123, 123]|
+--------------------+

-- array_union
SELECT array_union(array(1, 2, 3), array(1, 3, 5));
+-------------------------------------------+
|array_union(array(1, 2, 3), array(1, 3, 5))|
+-------------------------------------------+
|                               [1, 2, 3, 5]|
+-------------------------------------------+

-- arrays_overlap
SELECT arrays_overlap(array(1, 2, 3), array(3, 4, 5));
+----------------------------------------------+
|arrays_overlap(array(1, 2, 3), array(3, 4, 5))|
+----------------------------------------------+
|                                          true|
+----------------------------------------------+

-- arrays_zip
SELECT arrays_zip(array(1, 2, 3), array(2, 3, 4));
+------------------------------------------+
|arrays_zip(array(1, 2, 3), array(2, 3, 4))|
+------------------------------------------+
|                      [[1, 2], [2, 3], ...|
+------------------------------------------+

SELECT arrays_zip(array(1, 2), array(2, 3), array(3, 4));
+-------------------------------------------------+
|arrays_zip(array(1, 2), array(2, 3), array(3, 4))|
+-------------------------------------------------+
|                             [[1, 2, 3], [2, 3...|
+-------------------------------------------------+

-- concat
SELECT concat('SQL', 'Analytics');
+----------------------+
|concat(SQL, Analytics)|
+----------------------+
|          SQLAnalytics|
+----------------------+

SELECT concat(array(1, 2, 3), array(4, 5), array(6));
+---------------------------------------------+
|concat(array(1, 2, 3), array(4, 5), array(6))|
+---------------------------------------------+
|                           [1, 2, 3, 4, 5, 6]|
+---------------------------------------------+

-- flatten
SELECT flatten(array(array(1, 2), array(3, 4)));
+----------------------------------------+
|flatten(array(array(1, 2), array(3, 4)))|
+----------------------------------------+
|                            [1, 2, 3, 4]|
+----------------------------------------+

-- reverse
SELECT reverse('SQL Analytics');
+----------------------+
|reverse(SQL Analytics)|
+----------------------+
|         scitylanA LQS|
+----------------------+

SELECT reverse(array(2, 1, 4, 3));
+--------------------------+
|reverse(array(2, 1, 4, 3))|
+--------------------------+
|              [3, 4, 1, 2]|
+--------------------------+

-- sequence
SELECT sequence(1, 5);
+---------------+
| sequence(1, 5)|
+---------------+
|[1, 2, 3, 4, 5]|
+---------------+

SELECT sequence(5, 1);
+---------------+
| sequence(5, 1)|
+---------------+
|[5, 4, 3, 2, 1]|
+---------------+

SELECT sequence(to_date('2018-01-01'), to_date('2018-03-01'), interval 1 month);
+---------------------------------------------------------------------------+
|sequence(to_date('2018-01-01'), to_date('2018-03-01'), INTERVAL '1 months')|
+---------------------------------------------------------------------------+
|                                                       [2018-01-01, 2018...|
+---------------------------------------------------------------------------+

-- shuffle
SELECT shuffle(array(1, 20, 3, 5));
+---------------------------+
|shuffle(array(1, 20, 3, 5))|
+---------------------------+
|              [5, 20, 3, 1]|
+---------------------------+

SELECT shuffle(array(1, 20, null, 3));
+-------------------------------------------+
|shuffle(array(1, 20, CAST(NULL AS INT), 3))|
+-------------------------------------------+
|                                [20, 3,, 1]|
+-------------------------------------------+

-- slice
SELECT slice(array(1, 2, 3, 4), 2, 2);
+------------------------------+
|slice(array(1, 2, 3, 4), 2, 2)|
+------------------------------+
|                        [2, 3]|
+------------------------------+

SELECT slice(array(1, 2, 3, 4), -2, 2);
+-------------------------------+
|slice(array(1, 2, 3, 4), -2, 2)|
+-------------------------------+
|                         [3, 4]|
+-------------------------------+

-- sort_array
SELECT sort_array(array('b', 'd', null, 'c', 'a'), true);
+---------------------------------------------------------+
|sort_array(array(b, d, CAST(NULL AS STRING), c, a), true)|
+---------------------------------------------------------+
|                                           [, a, b, c, d]|
+---------------------------------------------------------+
```

## <a name="map-functions"></a>映射函数

| 功能                                           | 说明                                                  |
|----------------------------------------------------|--------------------------------------------------------------|
| map_concat(map,…)                                  | 返回所有给定映射的并集。                     |
| map_entries(map)                                   | 返回给定映射中所有条目的无序数组。  |
| map_from_entries(arrayOfEntries)                   | 返回从给定的条目数组创建的映射。       |
| map_keys(map)                                      | 返回包含映射键的无序数组。   |
| map_values(map)                                    | 返回包含映射值的无序数组。 |

### <a name="examples"></a>示例

```
-- map_concat
SELECT map_concat(map(1, 'a', 2, 'b'), map(3, 'c'));
+--------------------------------------+
|map_concat(map(1, a, 2, b), map(3, c))|
+--------------------------------------+
|                  [1 -> a, 2 -> b, ...|
+--------------------------------------+

-- map_entries
SELECT map_entries(map(1, 'a', 2, 'b'));
+----------------------------+
|map_entries(map(1, a, 2, b))|
+----------------------------+
|            [[1, a], [2, b]]|
+----------------------------+

-- map_from_entries
SELECT map_from_entries(array(struct(1, 'a'), struct(2, 'b')));
+---------------------------------------------------+
|map_from_entries(array(struct(1, a), struct(2, b)))|
+---------------------------------------------------+
|                                   [1 -> a, 2 -> b]|
+---------------------------------------------------+

-- map_keys
SELECT map_keys(map(1, 'a', 2, 'b'));
+-------------------------+
|map_keys(map(1, a, 2, b))|
+-------------------------+
|                   [1, 2]|
+-------------------------+

-- map_values
SELECT map_values(map(1, 'a', 2, 'b'));
+---------------------------+
|map_values(map(1, a, 2, b))|
+---------------------------+
|                     [a, b]|
+---------------------------+
```

## <a name="date-and-timestamp-functions"></a>日期和时间戳函数

有关日期和时间戳格式的信息，请参阅[日期/时间模式](sql-ref-datetime-pattern.md)。

| 功能                                               | 说明                                                                                                                                                                                                                                                                                                                   |
|--------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| add_months(start_date,num_months)                      | 返回在 ``start_date`` 之后 ``num_months`` 的日期。                                                                                                                                                                                                                                                                 |
| current_date()                                         | 返回查询计算开始时的当前日期。                                                                                                                                                                                                                                                                    |
| current_timestamp()                                    | 返回查询计算开始时的当前时间戳。                                                                                                                                                                                                                                                               |
| date_add(start_date,num_days)                          | 返回在 ``start_date`` 之后 ``num_days`` 的日期。                                                                                                                                                                                                                                                                   |
| date_format(timestamp,fmt)                             | 将 ``timestamp`` 转换为一个字符串值，该值采用日期格式 ``fmt`` 所指定的格式。                                                                                                                                                                                                                               |
| date_part(field,source)                                | 提取部分日期/时间戳或间隔源。                                                                                                                                                                                                                                                                     |
| date_sub(start_date,num_days)                          | 返回在 ``start_date`` 之前 ``num_days`` 的日期。                                                                                                                                                                                                                                                                  |
| date_trunc(fmt,ts)                                     | 返回已截断到由格式模型 ``fmt`` 指定的单位的时间戳 ``ts``。                                                                                                                                                                                                                                         |
| datediff(endDate,startDate)                            | 返回从 ``startDate`` 到 ``endDate`` 的天数。                                                                                                                                                                                                                                                                 |
| dayofweek(date)                                        | 返回日期/时间戳的星期日期（1 = 星期日，2 = 星期一，...，7 = 星期六）。                                                                                                                                                                                                                                     |
| dayofyear(date)                                        | 返回日期/时间戳的年份日期。                                                                                                                                                                                                                                                                                |
| from_unixtime(unix_time,format)                        | 返回指定 ``format`` 的 ``unix_time``。                                                                                                                                                                                                                                                                            |
| from_utc_timestamp(timestamp,timezone)                 | 在给定时间戳（例如“2017-07-14 02:40:00.0”）的情况下，将其解释为 UTC 时间，并将该时间呈现为给定时区中的时间戳。 例如，“GMT+1”会生成“2017-07-14 03:40:00.0”。                                                                                                                        |
| hour(timestamp)                                        | 返回字符串/时间戳的小时部分。                                                                                                                                                                                                                                                                           |
| last_day(date)                                         | 返回日期所属月份的最后一天。                                                                                                                                                                                                                                                                  |
| make_date(year,month,day)                              | 从年、月和日字段创建日期                                                                                                                                                                                                                                                                                   |
| make_timestamp(year,month,day,hour,min,sec[,timezone]) | 从年、月、日、小时、分钟、秒和时区字段创建时间戳。                                                                                                                                                                                                                                                   |
| minute(timestamp)                                      | 返回字符串/时间戳的分钟部分。                                                                                                                                                                                                                                                                         |
| month(date)                                            | 返回日期/时间戳的月份部分。                                                                                                                                                                                                                                                                            |
| months_between(timestamp1,timestamp2[,roundOff])       | 如果 ``timestamp1`` 晚于 ``timestamp2``，则结果为正。 如果 ``timestamp1`` 和 ``timestamp2`` 是同一月的同一天，或者两者都是同一月的最后一天，则会忽略当天的时间。 否则，会根据每月 31 天计算差异并将其舍入到 8 位数，除非 roundOff = false。 |
| next_day(start_date,day_of_week)                       | 返回晚于 ``start_date`` 并已按指示命名的第一个日期。                                                                                                                                                                                                                                             |
| now()                                                  | 返回查询计算开始时的当前时间戳。                                                                                                                                                                                                                                                               |
| quarter(date)                                          | 返回日期所对应的年内季度，范围为 1 到 4。                                                                                                                                                                                                                                                                |
| second(timestamp)                                      | 返回字符串/时间戳的秒钟部分。                                                                                                                                                                                                                                                                         |
| to_date(date_str[,fmt])                                | 使用 ``fmt`` 表达式将 ``date_str`` 表达式解析为日期。 输入无效时返回 null。 默认情况下，它会根据强制转换规则将表达式转换为日期（如果省略 ``fmt``）                                                                                                                                           |
| to_timestamp(timestamp_str[,fmt])                      | 使用 ``fmt`` 表达式将 ``timestamp_str`` 表达式解析为时间戳。 输入无效时返回 null。 默认情况下，它会根据强制转换规则将表达式转换为时间戳（如果省略 ``fmt``）。                                                                                                                           |
| to_unix_timestamp(timeExp[,format])                    | 返回给定时间的 UNIX 时间戳。                                                                                                                                                                                                                                                                                 |
| to_utc_timestamp(timestamp,timezone)                   | 在给定时间戳（例如“2017-07-14 02:40:00.0”）的情况下，将其解释为给定时区的时间，并将该时间呈现为以 UTC 表示的时间戳。 例如，“GMT+1”会生成“2017-07-14 01:40:00.0”                                                                                                                         |
| trunc(datefmt)                                         | 返回 ``date``，其日期的时间部分已截断到格式模型 ``fmt`` 所指定的单位。                                                                                                                                                                                                                |
| unix_timestamp([timeExp[format]])                      | 返回当前时间或指定时间的 UNIX 时间戳。                                                                                                                                                                                                                                                                      |
| weekday(date)                                          | 返回日期/时间戳的星期日期（0 = 星期一，1 = 星期二，...，6 = 星期日）。                                                                                                                                                                                                                                      |
| weekofyear(date)                                       | 返回给定日期是一年中的第几周。 一周被视为从星期一开始，第 1 周的持续时间 > 3 天。                                                                                                                                                                                          |
| year(date)                                             | 返回日期/时间戳的年份部分。                                                                                                                                                                                                                                                                             |

### <a name="examples"></a>示例

```
-- add_months
SELECT add_months('2016-08-31', 1);
+---------------------------------------+
|add_months(CAST(2016-08-31 AS DATE), 1)|
+---------------------------------------+
|                             2016-09-30|
+---------------------------------------+

-- current_date
SELECT current_date();
+--------------+
|current_date()|
+--------------+
|    2020-06-06|
+--------------+

SELECT current_date;
+--------------+
|current_date()|
+--------------+
|    2020-06-06|
+--------------+

-- current_timestamp
SELECT current_timestamp();
+--------------------+
| current_timestamp()|
+--------------------+
|2020-06-06 14:00:...|
+--------------------+

SELECT current_timestamp;
+--------------------+
| current_timestamp()|
+--------------------+
|2020-06-06 14:00:...|
+--------------------+

-- date_add
SELECT date_add('2016-07-30', 1);
+-------------------------------------+
|date_add(CAST(2016-07-30 AS DATE), 1)|
+-------------------------------------+
|                           2016-07-31|
+-------------------------------------+

-- date_format
SELECT date_format('2016-04-08', 'y');
+---------------------------------------------+
|date_format(CAST(2016-04-08 AS TIMESTAMP), y)|
+---------------------------------------------+
|                                         2016|
+---------------------------------------------+

-- date_part
SELECT date_part('YEAR', TIMESTAMP '2019-08-12 01:00:00.123456');
+---------------------------------------------------------+
|date_part('YEAR', TIMESTAMP '2019-08-12 01:00:00.123456')|
+---------------------------------------------------------+
|                                                     2019|
+---------------------------------------------------------+

SELECT date_part('week', timestamp'2019-08-12 01:00:00.123456');
+---------------------------------------------------------+
|date_part('week', TIMESTAMP '2019-08-12 01:00:00.123456')|
+---------------------------------------------------------+
|                                                       33|
+---------------------------------------------------------+

SELECT date_part('doy', DATE'2019-08-12');
+-----------------------------------+
|date_part('doy', DATE '2019-08-12')|
+-----------------------------------+
|                                224|
+-----------------------------------+

SELECT date_part('SECONDS', timestamp'2019-10-01 00:00:01.000001');
+------------------------------------------------------------+
|date_part('SECONDS', TIMESTAMP '2019-10-01 00:00:01.000001')|
+------------------------------------------------------------+
|                                                    1.000001|
+------------------------------------------------------------+

SELECT date_part('days', interval 1 year 10 months 5 days);
+------------------------------------------------------+
|date_part('days', INTERVAL '1 years 10 months 5 days')|
+------------------------------------------------------+
|                                                     5|
+------------------------------------------------------+

SELECT date_part('seconds', interval 5 hours 30 seconds 1 milliseconds 1 microseconds);
+----------------------------------------------------------+
|date_part('seconds', INTERVAL '5 hours 30.001001 seconds')|
+----------------------------------------------------------+
|                                                 30.001001|
+----------------------------------------------------------+

-- date_sub
SELECT date_sub('2016-07-30', 1);
+-------------------------------------+
|date_sub(CAST(2016-07-30 AS DATE), 1)|
+-------------------------------------+
|                           2016-07-29|
+-------------------------------------+

-- date_trunc
SELECT date_trunc('YEAR', '2015-03-05T09:32:05.359');
+------------------------------------------------------------+
|date_trunc(YEAR, CAST(2015-03-05T09:32:05.359 AS TIMESTAMP))|
+------------------------------------------------------------+
|                                         2015-01-01 00:00:00|
+------------------------------------------------------------+

SELECT date_trunc('MM', '2015-03-05T09:32:05.359');
+----------------------------------------------------------+
|date_trunc(MM, CAST(2015-03-05T09:32:05.359 AS TIMESTAMP))|
+----------------------------------------------------------+
|                                       2015-03-01 00:00:00|
+----------------------------------------------------------+

SELECT date_trunc('DD', '2015-03-05T09:32:05.359');
+----------------------------------------------------------+
|date_trunc(DD, CAST(2015-03-05T09:32:05.359 AS TIMESTAMP))|
+----------------------------------------------------------+
|                                       2015-03-05 00:00:00|
+----------------------------------------------------------+

SELECT date_trunc('HOUR', '2015-03-05T09:32:05.359');
+------------------------------------------------------------+
|date_trunc(HOUR, CAST(2015-03-05T09:32:05.359 AS TIMESTAMP))|
+------------------------------------------------------------+
|                                         2015-03-05 09:00:00|
+------------------------------------------------------------+

SELECT date_trunc('MILLISECOND', '2015-03-05T09:32:05.123456');
+----------------------------------------------------------------------+
|date_trunc(MILLISECOND, CAST(2015-03-05T09:32:05.123456 AS TIMESTAMP))|
+----------------------------------------------------------------------+
|                                                  2015-03-05 09:32:...|
+----------------------------------------------------------------------+

-- datediff
SELECT datediff('2009-07-31', '2009-07-30');
+------------------------------------------------------------+
|datediff(CAST(2009-07-31 AS DATE), CAST(2009-07-30 AS DATE))|
+------------------------------------------------------------+
|                                                           1|
+------------------------------------------------------------+

SELECT datediff('2009-07-30', '2009-07-31');
+------------------------------------------------------------+
|datediff(CAST(2009-07-30 AS DATE), CAST(2009-07-31 AS DATE))|
+------------------------------------------------------------+
|                                                          -1|
+------------------------------------------------------------+

-- dayofweek
SELECT dayofweek('2009-07-30');
+-----------------------------------+
|dayofweek(CAST(2009-07-30 AS DATE))|
+-----------------------------------+
|                                  5|
+-----------------------------------+

-- dayofyear
SELECT dayofyear('2016-04-09');
+-----------------------------------+
|dayofyear(CAST(2016-04-09 AS DATE))|
+-----------------------------------+
|                                100|
+-----------------------------------+

-- from_unixtime
SELECT from_unixtime(0, 'yyyy-MM-dd HH:mm:ss');
+-----------------------------------------------------+
|from_unixtime(CAST(0 AS BIGINT), yyyy-MM-dd HH:mm:ss)|
+-----------------------------------------------------+
|                                  1970-01-01 00:00:00|
+-----------------------------------------------------+

-- from_utc_timestamp
SELECT from_utc_timestamp('2016-08-31', 'Asia/Seoul');
+-------------------------------------------------------------+
|from_utc_timestamp(CAST(2016-08-31 AS TIMESTAMP), Asia/Seoul)|
+-------------------------------------------------------------+
|                                          2016-08-31 09:00:00|
+-------------------------------------------------------------+

-- hour
SELECT hour('2009-07-30 12:58:59');
+--------------------------------------------+
|hour(CAST(2009-07-30 12:58:59 AS TIMESTAMP))|
+--------------------------------------------+
|                                          12|
+--------------------------------------------+

-- last_day
SELECT last_day('2009-01-12');
+----------------------------------+
|last_day(CAST(2009-01-12 AS DATE))|
+----------------------------------+
|                        2009-01-31|
+----------------------------------+

-- make_date
SELECT make_date(2013, 7, 15);
+----------------------+
|make_date(2013, 7, 15)|
+----------------------+
|            2013-07-15|
+----------------------+

SELECT make_date(2019, 13, 1);
+----------------------+
|make_date(2019, 13, 1)|
+----------------------+
|                  null|
+----------------------+

SELECT make_date(2019, 7, NULL);
+-------------------------------------+
|make_date(2019, 7, CAST(NULL AS INT))|
+-------------------------------------+
|                                 null|
+-------------------------------------+

SELECT make_date(2019, 2, 30);
+----------------------+
|make_date(2019, 2, 30)|
+----------------------+
|                  null|
+----------------------+

-- make_timestamp
SELECT make_timestamp(2014, 12, 28, 6, 30, 45.887);
+-----------------------------------------------------------------+
|make_timestamp(2014, 12, 28, 6, 30, CAST(45.887 AS DECIMAL(8,6)))|
+-----------------------------------------------------------------+
|                                             2014-12-28 06:30:...|
+-----------------------------------------------------------------+

SELECT make_timestamp(2014, 12, 28, 6, 30, 45.887, 'CET');
+----------------------------------------------------------------------+
|make_timestamp(2014, 12, 28, 6, 30, CAST(45.887 AS DECIMAL(8,6)), CET)|
+----------------------------------------------------------------------+
|                                                  2014-12-28 05:30:...|
+----------------------------------------------------------------------+

SELECT make_timestamp(2019, 6, 30, 23, 59, 60);
+-------------------------------------------------------------+
|make_timestamp(2019, 6, 30, 23, 59, CAST(60 AS DECIMAL(8,6)))|
+-------------------------------------------------------------+
|                                          2019-07-01 00:00:00|
+-------------------------------------------------------------+

SELECT make_timestamp(2019, 13, 1, 10, 11, 12, 'PST');
+------------------------------------------------------------------+
|make_timestamp(2019, 13, 1, 10, 11, CAST(12 AS DECIMAL(8,6)), PST)|
+------------------------------------------------------------------+
|                                                              null|
+------------------------------------------------------------------+

SELECT make_timestamp(null, 7, 22, 15, 30, 0);
+-------------------------------------------------------------------------+
|make_timestamp(CAST(NULL AS INT), 7, 22, 15, 30, CAST(0 AS DECIMAL(8,6)))|
+-------------------------------------------------------------------------+
|                                                                     null|
+-------------------------------------------------------------------------+

-- minute
SELECT minute('2009-07-30 12:58:59');
+----------------------------------------------+
|minute(CAST(2009-07-30 12:58:59 AS TIMESTAMP))|
+----------------------------------------------+
|                                            58|
+----------------------------------------------+

-- month
SELECT month('2016-07-30');
+-------------------------------+
|month(CAST(2016-07-30 AS DATE))|
+-------------------------------+
|                              7|
+-------------------------------+

-- months_between
SELECT months_between('1997-02-28 10:30:00', '1996-10-30');
+-------------------------------------------------------------------------------------------+
|months_between(CAST(1997-02-28 10:30:00 AS TIMESTAMP), CAST(1996-10-30 AS TIMESTAMP), true)|
+-------------------------------------------------------------------------------------------+
|                                                                                 3.94959677|
+-------------------------------------------------------------------------------------------+

SELECT months_between('1997-02-28 10:30:00', '1996-10-30', false);
+--------------------------------------------------------------------------------------------+
|months_between(CAST(1997-02-28 10:30:00 AS TIMESTAMP), CAST(1996-10-30 AS TIMESTAMP), false)|
+--------------------------------------------------------------------------------------------+
|                                                                          3.9495967741935485|
+--------------------------------------------------------------------------------------------+

-- next_day
SELECT next_day('2015-01-14', 'TU');
+--------------------------------------+
|next_day(CAST(2015-01-14 AS DATE), TU)|
+--------------------------------------+
|                            2015-01-20|
+--------------------------------------+

-- now
SELECT now();
+--------------------+
|               now()|
+--------------------+
|2020-06-06 14:00:...|
+--------------------+

-- quarter
SELECT quarter('2016-08-31');
+---------------------------------+
|quarter(CAST(2016-08-31 AS DATE))|
+---------------------------------+
|                                3|
+---------------------------------+

-- second
SELECT second('2009-07-30 12:58:59');
+----------------------------------------------+
|second(CAST(2009-07-30 12:58:59 AS TIMESTAMP))|
+----------------------------------------------+
|                                            59|
+----------------------------------------------+

-- to_date
SELECT to_date('2009-07-30 04:17:52');
+------------------------------+
|to_date('2009-07-30 04:17:52')|
+------------------------------+
|                    2009-07-30|
+------------------------------+

SELECT to_date('2016-12-31', 'yyyy-MM-dd');
+-----------------------------------+
|to_date('2016-12-31', 'yyyy-MM-dd')|
+-----------------------------------+
|                         2016-12-31|
+-----------------------------------+

-- to_timestamp
SELECT to_timestamp('2016-12-31 00:12:00');
+-----------------------------------+
|to_timestamp('2016-12-31 00:12:00')|
+-----------------------------------+
|                2016-12-31 00:12:00|
+-----------------------------------+

SELECT to_timestamp('2016-12-31', 'yyyy-MM-dd');
+----------------------------------------+
|to_timestamp('2016-12-31', 'yyyy-MM-dd')|
+----------------------------------------+
|                     2016-12-31 00:00:00|
+----------------------------------------+

-- to_unix_timestamp
SELECT to_unix_timestamp('2016-04-08', 'yyyy-MM-dd');
+-----------------------------------------+
|to_unix_timestamp(2016-04-08, yyyy-MM-dd)|
+-----------------------------------------+
|                               1460073600|
+-----------------------------------------+

-- to_utc_timestamp
SELECT to_utc_timestamp('2016-08-31', 'Asia/Seoul');
+-----------------------------------------------------------+
|to_utc_timestamp(CAST(2016-08-31 AS TIMESTAMP), Asia/Seoul)|
+-----------------------------------------------------------+
|                                        2016-08-30 15:00:00|
+-----------------------------------------------------------+

-- trunc
SELECT trunc('2019-08-04', 'week');
+-------------------------------------+
|trunc(CAST(2019-08-04 AS DATE), week)|
+-------------------------------------+
|                           2019-07-29|
+-------------------------------------+

SELECT trunc('2019-08-04', 'quarter');
+----------------------------------------+
|trunc(CAST(2019-08-04 AS DATE), quarter)|
+----------------------------------------+
|                              2019-07-01|
+----------------------------------------+

SELECT trunc('2009-02-12', 'MM');
+-----------------------------------+
|trunc(CAST(2009-02-12 AS DATE), MM)|
+-----------------------------------+
|                         2009-02-01|
+-----------------------------------+

SELECT trunc('2015-10-27', 'YEAR');
+-------------------------------------+
|trunc(CAST(2015-10-27 AS DATE), YEAR)|
+-------------------------------------+
|                           2015-01-01|
+-------------------------------------+

-- unix_timestamp
SELECT unix_timestamp();
+--------------------------------------------------------+
|unix_timestamp(current_timestamp(), yyyy-MM-dd HH:mm:ss)|
+--------------------------------------------------------+
|                                              1591452047|
+--------------------------------------------------------+

SELECT unix_timestamp('2016-04-08', 'yyyy-MM-dd');
+--------------------------------------+
|unix_timestamp(2016-04-08, yyyy-MM-dd)|
+--------------------------------------+
|                            1460073600|
+--------------------------------------+

-- weekday
SELECT weekday('2009-07-30');
+---------------------------------+
|weekday(CAST(2009-07-30 AS DATE))|
+---------------------------------+
|                                3|
+---------------------------------+

-- weekofyear
SELECT weekofyear('2008-02-20');
+------------------------------------+
|weekofyear(CAST(2008-02-20 AS DATE))|
+------------------------------------+
|                                   8|
+------------------------------------+

-- year
SELECT year('2016-07-30');
+------------------------------+
|year(CAST(2016-07-30 AS DATE))|
+------------------------------+
|                          2016|
+------------------------------+
```

## <a name="json-functions"></a>JSON 函数

| 功能                                           | 说明                                                                                                                                      |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| from_json(jsonStr, schema[, options])              | 返回具有给定 ``jsonStr`` 和 ``schema`` 的结构值。                                                                                |
| get_json_object(json_txt, path)                    | 从 ``path`` 提取 JSON 对象。                                                                                                            |
| json_tuple(jsonStr, p1, p2, …, pn)                 | 像函数 ``get_json_object`` 一样返回一个元组，但采用多个名称。 所有输入参数和输出列类型均为字符串。 |
| schema_of_json(json[, options])                    | 以 JSON 字符串的 DDL 格式返回架构。                                                                                                 |
| to_json(expr[, options])                           | 返回具有给定结构值的 JSON 字符串。                                                                                                 |

### <a name="examples"></a>示例

```
-- from_json
SELECT from_json('{"a":1, "b":0.8}', 'a INT, b DOUBLE');
+---------------------------+
|from_json({"a":1, "b":0.8})|
+---------------------------+
|                   [1, 0.8]|
+---------------------------+

SELECT from_json('{"time":"26/08/2015"}', 'time Timestamp', map('timestampFormat', 'dd/MM/yyyy'));
+--------------------------------+
|from_json({"time":"26/08/2015"})|
+--------------------------------+
|            [2015-08-26 00:00...|
+--------------------------------+

-- get_json_object
SELECT get_json_object('{"a":"b"}', '$.a');
+-------------------------------+
|get_json_object({"a":"b"}, $.a)|
+-------------------------------+
|                              b|
+-------------------------------+

-- json_tuple
SELECT json_tuple('{"a":1, "b":2}', 'a', 'b');
+---+---+
| c0| c1|
+---+---+
|  1|  2|
+---+---+

-- schema_of_json
SELECT schema_of_json('[{"col":0}]');
+---------------------------+
|schema_of_json([{"col":0}])|
+---------------------------+
|       array<struct<col:...|
+---------------------------+

SELECT schema_of_json('[{"col":01}]', map('allowNumericLeadingZeros', 'true'));
+----------------------------+
|schema_of_json([{"col":01}])|
+----------------------------+
|        array<struct<col:...|
+----------------------------+

-- to_json
SELECT to_json(named_struct('a', 1, 'b', 2));
+---------------------------------+
|to_json(named_struct(a, 1, b, 2))|
+---------------------------------+
|                    {"a":1,"b":2}|
+---------------------------------+

SELECT to_json(named_struct('time', to_timestamp('2015-08-26', 'yyyy-MM-dd')), map('timestampFormat', 'dd/MM/yyyy'));
+---------------------------------------------------------------------+
|to_json(named_struct(time, to_timestamp('2015-08-26', 'yyyy-MM-dd')))|
+---------------------------------------------------------------------+
|                                                 {"time":"26/08/20...|
+---------------------------------------------------------------------+

SELECT to_json(array(named_struct('a', 1, 'b', 2)));
+----------------------------------------+
|to_json(array(named_struct(a, 1, b, 2)))|
+----------------------------------------+
|                         [{"a":1,"b":2}]|
+----------------------------------------+

SELECT to_json(map('a', named_struct('b', 1)));
+-----------------------------------+
|to_json(map(a, named_struct(b, 1)))|
+-----------------------------------+
|                      {"a":{"b":1}}|
+-----------------------------------+

SELECT to_json(map(named_struct('a', 1),named_struct('b', 2)));
+----------------------------------------------------+
|to_json(map(named_struct(a, 1), named_struct(b, 2)))|
+----------------------------------------------------+
|                                     {"[1]":{"b":2}}|
+----------------------------------------------------+

SELECT to_json(map('a', 1));
+------------------+
|to_json(map(a, 1))|
+------------------+
|           {"a":1}|
+------------------+

SELECT to_json(array((map('a', 1))));
+-------------------------+
|to_json(array(map(a, 1)))|
+-------------------------+
|                [{"a":1}]|
+-------------------------+
```