Spring MVC

#### 容器启动

ServletContext
WEB容器在启动时，它会为每个WEB应用程序都创建一个对应的ServletContext对象，它代表当前web应用。 
在当前的web应用中所有servlet都共享一个ServletContext对象。 
ServletContext对象包含在ServletConfig对象中。 
ServletConfig对象在初始化Servlet 时由Web服务器提供给Servlet。 
由于一个WEB应用中的所有Servlet共享同一个ServletContext对象，因此Servlet对象之间可以通过ServletContext对象来实现通讯。 



####  处理请求



#### Context Hierarchy

`DispatcherServlet` expects a `WebApplicationContext` (an extension of a plain `ApplicationContext`) for *its own configuration*（一个ApplicationConext就是一个Spring配置集合）. 

For many applications, having a single `WebApplicationContext` is simple and suffices. It is also possible to have a context hierarchy where one root `WebApplicationContext` is shared across multiple `DispatcherServlet` (or other `Servlet`) instances, each with its own child `WebApplicationContext` configuration. See [Additional Capabilities of the `ApplicationContext`](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#context-introduction) for more on the context hierarchy feature.

![mvc context hierarchy](https://docs.spring.io/spring-framework/docs/current/reference/html/images/mvc-context-hierarchy.png)





#### 执行流程

向服务器发送Http request请求，请求被**前端控制器（DispatcherServlet）**捕获。
前端控制器根据xml文件中的配置（或者注解）对请求的URL进行解析，得到请求资源标识符（URI）。然后根据该URI，调用**处理器映射器（HandlerMapping）**获得处理该请求的Handler以及Handler对应的拦截器，最后以 HandlerExecutionChain 对象的形式返回。
前端控制器根据获得的Handler，选择一个合适的**处理器适配器（HandlerAdapter）**去执行该Handler。
处理器适配器提取request中的模型数据，填充Handler入参，执行处理器（Handler）（也称之为Controller）.
Handler(Controller)执行完成后，向处理器适配器返回一个ModelAndView对象，处理器适配器再向前端控制器返回该ModelAndView对象（ModelAndView只是一个逻辑视图）。
根据返回的ModelAndView，前端控制器请求一个适合的视图解析器（ViewResolver）（必须是已经注册到Spring容器中的ViewResolver）去进行视图解析，然后视图解析器向前端控制器返回一个真正的视图View（jsp）。
前端控制器通过Model解析出ModelAndView中的参数进行解析，最终展现出完整的View并通过Http response返回给客户端。





#### 组件分析（Special Bean Type）

**HandlerMapping**

HandlerInterceptor

HandlerExecutionChain



**HandlerAdapter**



**HandlerExceptionResolver**

SimpleMappingExceptionResolver



**ViewResolver**

InternalResourceViewResolver

ModelAndView



**LocaleResolver** & **LocaleContextResolver**



**ThemeResolver**



**MultipartResolver**



**FlashMapManager**



**HttpMessageConverter**

