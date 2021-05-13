不同厂商的关系型数据库，提供的链接方式，驱动包，驱动类名都是不一样的，Java数据库连接API，JDBC是Java语言中用来规范客户端程序如何来访问数据库的应用程序接口，提供了诸如查询和更新数据库中数据的方法，且适配大部分关系型数据库。

JDBC 是 Java 编程语言的 API，用于定义客户端如何访问数据库。 它提供了查询和更新数据库中数据的方法。 JDBC 面向关系数据库。 从技术角度来看，API 是`java.sql`包中的一组类。 要将 JDBC 与特定数据库一起使用，我们需要该数据库的 JDBC 驱动程序。

核心要素：驱动包、驱动类名、URL格式、默认端口。

#### JDBC API

------

**DriverManager**(java.sql.DriverManager)

　　管理JDBC驱动程序的基本服务API。调用方法Class.forName，显式地加载驱动程序类，正好适用于动态数据源的业务场景，数据源类型未知情况。加载Driver类并在DriverManager类注册后，即可用来与数据库建立连接。

**DataSource**(javax.sql.DataSource)

　　DataSource接口，由驱动程序供应商实现，负责建立与数据库的连接，当在应用程序中访问数据库时，常用于获取操作数据的Connection对象。

**Connection**(java.sql.Connection)

　　Connection接口代表与特定的数据库的连接，要对数据库数据进行操作,首先要获取数据库连接，Connection实现就像在应用程序中与数据库之间开通了一条通道，通过DriverManager类或DataSource类都可获取Connection实例。

**Statement**

　　Java中JDBC下执行数据库操作的一个重要接口，在已经建立数据库连接的基础上，向数据库发送要执行的SQL语句。

**PreparedStatement**

　　继承Statement接口，且实现SQL预编译，可以提高批量处理效率。常应用于批量数据写入场景。

**ResultSet**

　　存储JDBC查询结果集的对象，ResultSet接口提供从当前行检索列值的方法。





































