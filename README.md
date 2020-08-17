# apache_calcite_demo
Apache Calcite 是一款开源SQL解析工具, 可以将各种SQL语句解析成抽象语法术AST(Abstract Syntax Tree), 之后通过操作AST就可以把SQL中所要表达的算法与关系体现在具体代码之中。

Calcite的生前为Optiq(也为Farrago), 为Java语言编写, 通过十多年的发展, 在2013年成为Apache旗下顶级项目，并还在持续发展中, 该项目的创始人为Julian Hyde, 其拥有多年的SQL引擎开发经验, 目前在Hortonworks工作, 主要负责Calcite项目的开发与维护。

目前, 使用Calcite作为SQL解析与处理引擎有Hive、Drill、Flink、Phoenix和Storm，可以肯定的是还会有越来越多的数据处理引擎采用Calcite作为SQL解析工具。

总结来说Calcite有以下主要功能：

* SQL 解析
* SQL 校验
* 查询优化
* SQL 生成器
* 数据连接

一般来说Calcite解析SQL有以下几步:

* Parser. 此步中Calcite通过Java CC将SQL解析成未经校验的AST
* Validate. 该步骤主要作用是校证Parser步骤中的AST是否合法,如验证SQL scheme、字段、函数等是否存在; SQL语句是否合法等. 此步完成之后就生成了RelNode树（关于RelNode树, 请参考下文）
* Optimize. 该步骤主要的作用优化RelNode树, 并将其转化成物理执行计划。主要涉及SQL规则优化如:基于规则优化(RBO)及基于代价(CBO)优化; Optimze 这一步原则上来说是可选的, 通过Validate后的RelNode树已经可以直接转化物理执行计划，但现代的SQL解析器基本上都包括有这一步，目的是优化SQL执行计划。此步得到的结果为物理执行计划。
* Execute，即执行阶段。此阶段主要做的是:将物理执行计划转化成可在特定的平台执行的程序。如Hive与Flink都在在此阶段将物理执行计划CodeGen生成相应的可执行代码。


Calcite主要有以下概念：

* Catelog: 主要定义SQL语义相关的元数据与命名空间。
* SQL parser: 主要是把SQL转化成AST.
* SQL validator: 通过Catalog来校证AST.
* Query optimizer: 将AST转化成物理执行计划、优化物理执行计划.
* SQL generator: 反向将物理执行计划转化成SQL语句.


更多参考资料：
*  https://zhuanlan.zhihu.com/p/67560995
