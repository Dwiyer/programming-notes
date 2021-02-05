#### 连接查询

------

MySQL数据库支持如下的联接查询：

CROSS JOIN（交叉联接）

INNER JOIN（内联接）

OUTER JOIN（外联接）

其他



笛卡儿积、虚拟表、添加外部行



驱动表、被驱动表

```mysql
select * from t1 straight_join t2 on (t1.a=t2.a);
```

##### Index Nested-Loop Join

两个表使用 join 语句的话，需考虑连接表的大小，需要让小表做驱动表。这个结论的前提是“可以使用被驱动表的索引”。

##### Simple Nested-Loop Join

连接时被驱动表没有索引可用，被驱动表每一次连接查找都是全表扫描。

##### Block Nested-Loop Join

这时候，被驱动表上没有可用的索引，算法的流程是这样的：

把表 t1 的数据读入线程内存 **join_buffer** 中，由于我们这个语句中写的是 select *，因此是把整个表 t1 放入了内存；

扫描表 t2，把表 t2 中的每一行取出来，跟 join_buffer 中的数据做对比，满足 join 条件的，作为结果集的一部分返回。



- 如果可以使用 Index Nested-Loop Join 算法，也就是说可以用上被驱动表上的索引，其实是没问题的；
- 如果使用 Block Nested-Loop Join 算法，扫描行数就会过多。尤其是在大表上的 join 操作，这样可能要扫描被驱动表很多次，会占用大量的系统资源。所以这种 join 尽量不要用。
- 在使用 join 的时候，应该让小表做驱动表。



##### Multi-Range Read 优化

**MRR** 优化的设计思路。此时，语句的执行流程变成了这样：

- 根据索引 a，定位到满足条件的记录，将 id 值放入 read_rnd_buffer 中 ;
- 将 read_rnd_buffer 中的 id 进行递增排序；
- 排序后的 id 数组，依次到主键 id 索引中查记录，并作为结果返回。

这里，read_rnd_buffer 的大小是由 read_rnd_buffer_size 参数控制的。如果步骤 1 中，read_rnd_buffer 放满了，就会先执行完步骤 2 和 3，然后清空 read_rnd_buffer。之后继续找索引 a 的下个记录，并继续循环。

另外需要说明的是，如果你想要稳定地使用 MRR 优化的话，需要设置set optimizer_switch="mrr_cost_based=off"。（官方文档的说法，是现在的优化器策略，判断消耗的时候，会更倾向于不使用 MRR，把 mrr_cost_based 设置为 off，就是固定使用 MRR 了。）

**MRR 能够提升性能的核心**在于，这条查询语句在索引 a 上做的是一个范围查询（也就是说，这是一个多值查询），可以得到足够多的主键 id。这样通过排序以后，再去主键索引查数据，才能体现出“顺序性”的优势。MRR 能够提升性能的核心在于，这条查询语句在索引 a 上做的是一个范围查询（也就是说，这是一个多值查询），可以得到足够多的主键 id。这样通过排序以后，再去主键索引查数据，才能体现出“顺序性”的优势。MRR 能够提升性能的核心在于，这条查询语句在索引 a 上做的是一个范围查询（也就是说，这是一个多值查询），可以得到足够多的主键 id。这样通过排序以后，再去主键索引查数据，才能体现出“顺序性”的优势。



##### Batched Key Access

MySQL 在 5.6 版本后开始引入的 Batched Key Access(BKA) 算法了。这个 BKA 算法，其实就是对 NLJ 算法的优化。











