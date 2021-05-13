#### 数据源要素

------

JDBC

```properties
url: jdbc:oracle:thin:@10.22.13.56:1521/mirddb
username: midb01
password: midb01
driver-class-name: oracle.jdbc.driver.OracleDriver
```

MybatisSqlSessionFactoryBean配置SqlSessionFactory，而MybatisSqlSessionFactoryBean刚好有个方法就是
setPlugins：用于配置插件。



数据库连接池



MybatisPlus动态数据源的原理

- [dynamic-datasource ](https://dynamic-datasource.com/)- 基于 SpringBoot 的多数据源组件，功能强悍，支持 Seata 分布式事务。

数据源来源的默认实现是`YmlDynamicDataSourceProvider`，其从yaml或properties中读取信息并解析出所有数据源信息。









