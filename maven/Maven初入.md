#### pom.xml

------

maven的核心就是pom.xml，使用maven是为了更好的帮项目管理包依赖。如果要引入一个jar包，需要在pom文件中加上<dependency></dependency>就可以依赖相应的jar包。

场景一，有两个web项目W1、W2，一个java项目J1，依赖同一个jar包：domain.jar。如果分别在各自pom文件中引入common.jar的依赖，那么当common.jar的版本发生变化时，三个项目的pom文件都要改，项目中依赖越多修改越多。此时就可以使用parent标签, 可以创建一个parent项目，打包类型为pom，parent项目中没有任何代码，只是管理多个项目之间公共的依赖。在parent项目的pom文件中定义对domain.jar的依赖，W1W2J1三个子项目中只需要定义<parent></parent>，parent标签中写上parent项目的pom坐标就可以引用到domain.jar了。

场景二，有一个springweb.jar，只有W1W2两个web项目需要，J1项目是java项目不需要，那么又要怎么去依赖。如果W1W2中分别定义对springweb.jar的依赖，当springweb.jar版本变化时修改起来不方便。解决办法是在parent项目的pom文件中使用<dependencyManagement></dependencyManagement>将springweb.jar管理起来，如果有哪个子项目要用，那么子项目在自己的pom文件中使用

<dependency>

  <groupId></groupId>

   <artifactId></artifactId>

</dependency>

标签中写上springweb.jar的坐标，不需要写版本号，可以依赖到这个jar包了。这样springweb.jar的版本发生变化时只需要修改parent中的版本就可以了。



#### module

------

Q：子 module 相关于放入 dependencyManagement 中管理了吗？因为实际中有一个场景，子项目 parent 于带有 module 的父项目，需要手动指定父项目中的 module，同时要指定版本号。

A：













