## 可计算列（Computed Column）简介

**可计算列**允许您将数据的抽取／转换／重定义等操作预先定义在模型中，增强数据语义层模型，将查询运行时计算转换为 Cube 预计算。充分利用 KAP 的预计算能力，进一步提升查询效率。可计算列可以支持 Hive UDF，从而重用已有业务代码。


### 创建可计算列

KAP 允许您为每个模型中定义各自的可计算列。每个可计算列基于模型中的某个表，是该表上若干列（一个或者多个）的运算的结果。例如模型中有一个事实表`kylin_sales`，`kylin_sales`上有以下列：`price` (交易的单价)，`item_count`（交易数量）和`part_dt` （交易时间），您可以在`kylin_sales`上定义两个可计算列：`total_amount = price * item_count` 和 `deal_year = year(part_dt)`。这样，在创建 Cube 的时候，您不仅可以选择原来的 price/item_count/part_dt 作为 Cube 中的维度或者度量，还能选择 total_amount/deal_year 作为 Cube 的维度或者度量。

点击下图箭头所指的按钮，就可以根据提示创建可计算列，

![](images/computed_column_cn.1.png)

其中需要填写：

![](images/computed_column_cn.2.png)

+ **列**：定义可计算列的名称
+ **表达式**：可计算列的表达式定义。注意：只允许使用当前表上的列进行计算，不支持跨表的表达式
+ **数据类型**：定义可计算列的类型

在模型中定义完可计算列后，需要在创建 Cube 添加维度/度量的时候选入可计算列，可计算列被预计算后，才能体现性能优势。

![](images/computed_column_cn.3.png)


### 显式 vs. 隐式查询

在一个表上创建了可计算列后，逻辑上这个可计算列就被拼接到了这个表的列列表中。您可以像查询普通的列一样查询这个列（能够被查询的前提是这个某个 Ready 状态的 **Cube**/**TableIndex** 包含了该列，或者启用了 **Query Pushdown**）。在上面的`kylin_sales`例子中，如果您创建并构建了一个包含`sum(total_amount)`度量的Cube，您可以直接查询`select sum(total_amount) from kylin_sales`。我们将这种查询方式称为可计算列的**显式查询**。

或者，您也可以假装表上没有可计算列，直接使用可计算列背后的表达式进行查询，接着上面的例子，您可以查询`select sum(price * item_count) from kylin_sales`。KAP 会分析到`price * item_count`可以由可计算列`total_amount`替代，且`sum(total_amount)`已经在某个 Cube 中被预计算完毕，为了更好的性能，KAP 会将您原始查询翻译为`select sum(total_amount) from kylin_sales`，以求更佳的性能。我们将这种查询方式称为可计算列的**隐式查询**。

隐式查询默认没有被开启，为了开启它，您需要在`KYLIN_HOME/conf/kylin.properties`中添加`kylin.query.transformers=io.kyligence.kap.query.util.ConvertToComputedColumn` 



## 可计算列使用的一些规则

· KAP 2.4中，仅支持在事实表上定义可计算列（暂不支持跨表定义或在维度表上定义）。

· 用户可以在多个模型上定义不同的可计算列。

· 同一个项目下，可计算列的名字和表达式（计算逻辑）是一一对应的。即在不同模型下可以重复创建同名的可计算列，只要同名可计算列对应的表达式也完全相同。

· 同一项目下，可计算列的名字不允许和数据源中的列名重复。



## 高级函数的使用

可计算列的计算是直接下沉到数据源进行处理的，而当前Hive是KAP的默认数据源，因此可计算列的表达式定义默认需要以hive SQL的语法为准。

欲在可计算列中使用更多的函数，请在下面链接中参考Hive SQL函数的使用规范：
https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-StringFunctions

可计算列函数的具体使用案例请参考Kyilgence官网的技术博客：http://kyligence.io/zh/2017/07/17/7172/



