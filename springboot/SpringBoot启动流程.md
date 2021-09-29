#### Java启动流程

------

fat jar

BOOT-INF

META-INF(MANIFEST.MF)

Manifest-Version: 1.0		#版本
Spring-Boot-Classpath-Index: BOOT-INF/classpath.idx		
Start-Class: com.zyy.gradletest1.GradleTest1Application	     #项目的启动器
Spring-Boot-Classes: BOOT-INF/classes/		#记录编译后文件存放地址
Spring-Boot-Lib: BOOT-INF/lib/						  #记录第三方jar包存放地址
Spring-Boot-Version: 2.3.0.RELEASE		        #SpringBoot版本
Main-Class: org.springframework.boot.loader.JarLauncher #标记了程序的入口,程序的入口定义类加载器去加载项目的启动器



JarLauncher





spring boot为什么要自定义类加载器

从上面的源码分析，可以看出，spring boot创建了自己的类加载器，叫LaunchedURLClassloader。

spring boot想完成一个jar包完成整个程序的运行。jar in jar，无法进行加载。所以spring boot引入自定义类加载器去解决那些不符合jar规格的类加载问题。

spring boot这种优雅的方式，将我们自己的类和第三方jar包全部分开放置，AppClassLoader加载符合规范的Spring Boot ClassLoader（LaunchedURLClassloader）后，整个后续的类加载操作都会由自定义类加载器去加载，完美得实现了jar包的嵌套，带来了太多的便利。























