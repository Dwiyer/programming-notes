#### 前置概念

------

Cost







#### 优化器

------

Oracle中的优化器是SQL分析和执行的优化工具，它负责生成、制定SQL的执行计划。

Oracle的优化器有两种：

- RBO（Rule-Based Optimization） 基于规则的优化器
- CBO（Cost-Based Optimization） 基于代价的优化器

RBO：

RBO有严格的使用规则，只要按照这套规则去写SQL语句，无论数据表中的内容怎样，也不会影响到你的执行计划；

换句话说，RBO对数据“不敏感”，它要求SQL编写人员必须要了解各项细则；

RBO一直沿用至ORACLE 9i，从ORACLE 10g开始，RBO已经彻底被抛弃。

CBO：

CBO是一种比RBO更加合理、可靠的优化器，在ORACLE 10g中完全取代RBO；

CBO通过计算各种可能的执行计划的“代价”，即COST，从中选用COST最低的执行方案作为实际运行方案；

它依赖数据库对象的统计信息，统计信息的准确与否会影响CBO做出最优的选择，也就是对数据“敏感”。





#### 执行计划

------



##### 表访问方式

- TABLE ACCESS FULL（全表扫描）
- TABLE ACCESS BY ROWID（通过ROWID的表存取）
- TABLE ACCESS BY INDEX SCAN（索引扫描）

索引扫描又分五种：

- INDEX UNIQUE SCAN（索引唯一扫描）
- INDEX RANGE SCAN（索引范围扫描）
- INDEX FULL SCAN（索引全扫描）
- INDEX FAST FULL SCAN（索引快速扫描）
- INDEX SKIP SCAN（索引跳跃扫描）

a) **INDEX UNIQUE SCAN（索引唯一扫描）**：

针对唯一性索引（UNIQUE INDEX）的扫描，每次至多只返回**一条**记录；

表中某字段存在 UNIQUE、PRIMARY KEY 约束时，Oracle常实现唯一性扫描；

b) **INDEX RANGE SCAN（索引范围扫描）**：

使用一个索引存取多行数据；

发生索引范围扫描的三种情况：

- 在唯一索引列上使用了范围操作符（如：>  <  <>  >=  <=  between）
- 在组合索引上，只使用部分列进行查询（查询时必须包含前导列，否则会走全表扫描）
- 对非唯一索引列上进行的任何查询

c) **INDEX FULL SCAN（索引全扫描）**：

进行全索引扫描时，查询出的数据都必须从索引中可以直接得到（注意全索引扫描只有在CBO模式下才有效）

d) **INDEX FAST FULL SCAN（索引快速扫描）**:

扫描索引中的所有的数据块，与 INDEX FULL SCAN 类似，但是一个显著的区别是它**不对查询出的数据进行排序**（即数据不是以排序顺序被返回）

e) **INDEX SKIP SCAN（索引跳跃扫描）**：

Oracle 9i后提供，有时候复合索引的前导列（索引包含的第一列）没有在查询语句中出现，oralce也会使用该复合索引，这时候就使用的INDEX SKIP SCAN;

什么时候会触发 INDEX SKIP SCAN 呢？

前提条件：表有一个复合索引，且在查询时有除了前导列（索引中第一列）外的其他列作为条件，并且优化器模式为CBO时

当Oracle发现前导列的唯一值个数很少时，会将每个唯一值都作为常规扫描的入口，在此基础上做一次查找，最后合并这些查询；

例如：

假设表emp有ename（雇员名称）、job（职位名）、sex（性别）三个字段，并且建立了如 create index idx_emp on emp (sex, ename, job) 的复合索引；

因为性别只有 '男' 和 '女' 两个值，所以为了提高索引的利用率，Oracle可将这个复合索引拆成 ('男', ename, job)，('女', ename, job) 这两个复合索引；

当查询 select * from emp where job = 'Programmer' 时，该查询发出后：

Oracle先进入sex为'男'的入口，这时候使用到了 ('男', ename, job) 这条复合索引，查找 job = 'Programmer' 的条目；

再进入sex为'女'的入口，这时候使用到了 ('女', ename, job) 这条复合索引，查找 job = 'Programmer' 的条目；

最后合并查询到的来自两个入口的结果集。



##### 表连接

**JOIN** 关键字用于将两张表作连接，一次只能连接两张表，JOIN 操作的各步骤一般是串行的（在读取做连接的两张表的数据时可以并行读取）；

表（row source）之间的连接顺序对于查询效率有很大的影响，对首先存取的表（驱动表）先应用某些限制条件（Where过滤条件）以得到一个较小的row source，可以使得连接效率提高。

驱动表（Driving Table）：

表连接时**首先存取**的表，又称外层表（Outer Table），这个概念用于 NESTED LOOPS（嵌套循环） 与 HASH JOIN（哈希连接）中；

如果驱动表返回较多的行数据，则对所有的后续操作有负面影响，故一般选择小表（应用Where限制条件后返回较少行数的表）作为驱动表。

匹配表（Probed Table）：

又称为内层表（Inner Table），从驱动表获取一行具体数据后，会到该表中寻找符合连接条件的行。故该表一般为大表（应用Where限制条件后返回较多行数的表）。



表连接方式：

- INNER JOIN（内连接）
- OUTER JOIN（外连接）



表连接算法：

- SORT MERGE JOIN（排序-合并连接）
- NESTED LOOPS（嵌套循环）
- HASH JOIN（哈希连接）
- CARTESIAN PRODUCT（笛卡尔积）

















