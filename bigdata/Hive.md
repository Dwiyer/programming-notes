> [Hive](https://link.jianshu.com/?t=http://lib.csdn.net/base/hive)是建立在[Hadoop](https://link.jianshu.com/?t=http://lib.csdn.net/base/hadoop)上的**数据仓库基础构架**。它提供了一系列的工具，可以用来**进行数据提取转化加载**（ETL ），这是一种可以**存储、查询和分析**存储在 Hadoop 中的大规模数据的机制。Hive 定义了简单的类SQL查询语言，称为 HQL，它允许熟悉SQL的用户**查询数据**。同时，这个语言也允许熟悉 MapReduce 开发者的开发**自定义**的 mapper 和 reducer 来**处理内建的 mapper 和 reducer 无法完成的复杂的分析工作**。

> Hive在hadoop生态圈中属于数据仓库的角色。他能够**管理**hadoop中的数据，同时可以**查询**hadoop中的数据。本质上讲，hive是一个**SQL解析引擎**。Hive可以把**SQL查询**转换为MapReduce中的job，然后在Hadoop上运行。



#### Hive的组成

------

**用户接口**：用户访问Hive的入口 
  主要有三个：CIL（命令行），JDBC/ODBC（java），WebUI（浏览器访问）

**元数据**：Hive的用户信息与表的MetaData 
 Hive将元数据存储在数据库中，目前只支持mysql，derby. 
 元数据包括：表名字，表的列和分区及其属性，表的数据所在目录。

**解释器，编译器，优化器**：分析翻译、编译、优化HQL的组件 
 完成HQL查询语句从词法分析、语法分析、编译、优化及查询计划的生成。生成的查询计划存储在HDFS中，并随后由MapReduce调用执行。

 Hive的数据存储在HDFS中，大部分查询有MapReduce完成。（包含*的查询，例如select* from不会生成mapreduce任务）

**Metastore** 
 Hive的Metastore组件是Hive元数据集中存放地。Metastore组件包括两个部分：Metastore服务和后台数据的存储。后台数据存储的介质就是关系数据库，例如Hive默认的嵌入式磁盘数据库derby，还有mysql数据库。Metastore服务是建立在后台数据存储介质之上，并且可以和Hive服务进行交互的服务组件，默认情况下，Metastore服务和Hive服务是安装在一起的，运行在同一个进程当中。我也可以把Metastore服务从Hive服务里剥离出来，Metastore独立安装在一个集群里，Hive远程调用Metastore服务，这样我们可以把元数据这一层放到防火墙之后，客户端访问Hive服务，就可以连接到元数据这一层，从而提供了更好的管理性和安全保障。使用远程的Metastore服务，可以让Metastore服务和hive服务运行在不同的进程里，这样也保证了Hive的稳定性，提升了Hive服务的效率。

**Driver** 
1）Driver调用编译器处理HiveQL字串 
2）编译器将HQL转化为策略（plan） 
3）策略由元数据操作和HDFS操作组成。元数据操作只包含DDL（create，drop,alter）语句，HDFS操作只包含LOAD语句。 
4）对插入和查询而言，策略由map-reduce任务中有向非循环图组成（DAG directed acyclic graph）。



Hive 的数据存储是建立在 Hadoop 文件系统之上的。Hive 本身没有专门的数据存储格式，也不能为数据建立索引，因此用户可以非常自由地组织 Hive 中的表，只需要在创建表的时候告诉 Hive 数据中的列分隔符就可以解析数据了。

Hive 中主要包括 4 种数据模型：`表（Table）`、`外部表（External Table）`、`分区（Partition）`以及 `桶（Bucket）`。

Hive 的表和数据库中的表在概念上没有什么本质区别，在 Hive 中每个表都有一个对应的存储目录。而外部表指向已经在 HDFS 中存在的数据，也可以创建分区。Hive 中的每个分区都对应数据库中相应分区列的一个索引，但是其对分区的组织方式和传统关系数据库不同。桶在指定列进行 Hash 计算时，会根据哈希值切分数据，使每个桶对应一个文件。

> 分区表是指在创建表时指定分区空间，即指定表内的某几个字段作为分区列。分区表实际就是对应分布式文件系统上的的独立的文件夹，该文件夹下是该分区所有数据文件。而分区可以理解为分类，通过分类把不同类型的数据放到不同的目录下。分类的标准就是分区字段，可以是一个，也可以是多个。
>
> 分区表的意义在于优化查询。查询表时通过where字句查询指定所需查询的分区，避免全表扫描，提高处理效率，降低计算费用。

由于 Hive 的元数据可能要面临不断地更新、修改和读取操作，所以它显然不适合使用 Hadoop 文件系统进行存储。目前 Hive 把元数据存储在 RDBMS 中，比如存储在 MySQL, Derby 中。











