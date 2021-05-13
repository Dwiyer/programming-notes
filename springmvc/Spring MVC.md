Spring MVC

### 容器启动

ServletContext
WEB容器在启动时，它会为每个WEB应用程序都创建一个对应的ServletContext对象，它代表当前web应用。 
在当前的web应用中所有servlet都共享一个ServletContext对象。 
ServletContext对象包含在ServletConfig对象中。 
ServletConfig对象在初始化Servlet 时由Web服务器提供给Servlet。 
由于一个WEB应用中的所有Servlet共享同一个ServletContext对象，因此Servlet对象之间可以通过ServletContext对象来实现通讯。 



###  处理请求



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

