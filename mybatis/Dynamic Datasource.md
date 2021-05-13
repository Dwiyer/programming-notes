#### 源码解析

------

##### 问题

1.DynamicDataSourceProperties是如何实例化到自动配置类的？



##### 主要组件

DynamicDataSourceProvider

*多数据源加载接口，默认从yml中读取多数据源配置*

DynamicDataSourceStrategy

DynamicRoutingDataSource, DynamicGroupDataSource

*注册自己的动态多数据源DataSource*

DynamicDataSourceAnnotationAdvisor

*AOP切面，对DS注解过的方法进行增强，达到切换数据源的目的*

DsProcessor

*@DS注解动态参数解析器链*

DynamicDataSourceAdvisor

*提供不使用注解而使用正则或spel来切换数据源方案（实验性功能）*



###### Creator



**DruidDataSourceCreator**



##### 流程分析

自动配置类DynamicDataSourceAutoConfiguration会加载yml配置，同时配置好各个组件。





#### 特性

------

1. 支持 **数据源分组** ，适用于多种场景 纯粹多库 读写分离 一主多从 混合模式。
2. 支持无数据源启动，支持配置懒启动数据源(3.3.2+)。
3. 支持数据库敏感配置信息 **加密** ENC()。
4. 支持每个数据库独立初始化表结构schema和数据库database。
5. 支持 **自定义注解** ，需继承DS(3.2.0+)。
6. 提供对Druid，Mybatis-Plus，P6sy，Jndi的快速集成。
7. 简化Druid和HikariCp配置，提供 **全局参数配置** 。配置一次，全局通用。
8. 提供 **自定义数据源来源** 方案。
9. 提供项目启动后 **动态增加移除数据源** 方案。
10. 提供Mybatis环境下的 **纯读写分离** 方案。
11. 提供使用 **spel动态参数** 解析数据源方案。内置spel，session，header，支持自定义。
12. 支持 **多层数据源嵌套切换** 。（ServiceA >>> ServiceB >>> ServiceC）。
13. 提供对shiro，sharding-jdbc,quartz等第三方库集成的方案,注意事项和示例。
14. 提供 **基于seata的分布式事务方案。** 附：不支持原生spring事务。
15. 提供 **本地多数据源事务方案。** 附：不支持原生spring事务(3.3.1+)。



#### 数据源分组

------

如果是多主多从，那么就用**数据组名称_xxx**，下划线前面的就是数据组名称，相同组名称的数据源会放在一个组下。切换数据源时，可以指定**具体数据源名称**，也可以指定**组名**然后会自动采用负载均衡算法切换。

















































