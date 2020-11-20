Spring MVC

### 容器启动

ServletContext
WEB容器在启动时，它会为每个WEB应用程序都创建一个对应的ServletContext对象，它代表当前web应用。 
在当前的web应用中所有servlet都共享一个ServletContext对象。 
ServletContext对象包含在ServletConfig对象中。 
ServletConfig对象在初始化Servlet 时由Web服务器提供给Servlet。 
由于一个WEB应用中的所有Servlet共享同一个ServletContext对象，因此Servlet对象之间可以通过ServletContext对象来实现通讯。 



###  处理请求

#### HttpServletBean



#### FrameworkServlet

LocaleContext  LocaleContextHolder <!--获取Locale-->

ServletRequestAttributes->RequestAttributes  RequestContextHoler <!--管理request和session属性-->

WebAsyncManager  WebAsyncUtils

*service()*

  *processRequest()*  <!--是FrameworkServlet类在处理请求中最核心的方法。对LocaleContext和RequestAttributes的设置及恢复-->

​    *initContextHolders()*    <!--设置LocaleContext和RequestAttributes-->

​    *doService()*  <!--模板方法，实际处理请求入口-->

​    *resetContextHolders()* <!--恢复LocaleContext和RequestAttributes-->

​    *publishRequestHandledEvent()* <!--发布ServletRequestHandledEvent类型的消息-->



#### DispatcherServlet

DispatcherServlet其实就是个Servlet（它继承自HttpServlet基类），同样也需要在你web应用的web.xml配置文件下声明。你需要在web.xml文件中把你希望DispatcherServlet处理的请求映射到对应的URL上去。这就是标准的Java EE Servlet配置；

a.DispatcherServlet如何与IoC容器、Servlet容器集成？
1>使用spring mvc 启动了两个context：***Root WebApplicationContext*** 和 ***Servlet WebapplicationContext***。
2>***ContextLoaderListener*** 创建基于web的应用根 applicationContext 并将它放入到ServletContext. applicationContext加载或者卸载spring管理的beans。在structs和spring mvc的控制层都是这样使用的。称之为Root WebapplicationContext。
3>DispatcherServlet创建自己的WebApplicationContext并管理这个WebApplicationContext里面的 handlers/controllers/view-resolvers。称之为Servlet WebapplicationContext。



#### Context Hierarchy

`DispatcherServlet` expects a `WebApplicationContext` (an extension of a plain `ApplicationContext`) for *its own configuration*（一个ApplicationConext就是一个Spring配置集合）. 

For many applications, having a single `WebApplicationContext` is simple and suffices. It is also possible to have a context hierarchy where one root `WebApplicationContext` is shared across multiple `DispatcherServlet` (or other `Servlet`) instances, each with its own child `WebApplicationContext` configuration. See [Additional Capabilities of the `ApplicationContext`](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#context-introduction) for more on the context hierarchy feature.

![mvc context hierarchy](https://docs.spring.io/spring-framework/docs/current/reference/html/images/mvc-context-hierarchy.png)





### 组件分析（Special Bean Type）

HandlerMapping
HandlerAdapter
HandlerExceptionResolver
ViewResolver
LocaleResolver & LocaleContextResolver
ThemeResolver
MultipartResolver
FlashMapManager

HttpMessageConverter

