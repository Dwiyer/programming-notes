**HandlerMapping**

HandlerMappings 定义*request*和*handler*之间的映射。它的官方文档这样描述：

```
Interface HandlerMapping

All Known Implementing Classes: 
AbstractControllerUrlHandlerMapping, AbstractDetectingUrlHandlerMapping, AbstractHandlerMapping, AbstractHandlerMethodMapping, AbstractUrlHandlerMapping, BeanNameUrlHandlerMapping, ControllerBeanNameHandlerMapping, ControllerClassNameHandlerMapping, DefaultAnnotationHandlerMapping, RequestMappingHandlerMapping, RequestMappingInfoHandlerMapping, SimpleUrlHandlerMapping 
Functional Interface: 
This is a functional interface and can therefore be used as the assignment target for a lambda expression or method reference. 

--------------------------------------------------------------------------------

public interface HandlerMappingInterface to be implemented by objects that define a mapping between requests and handler objects. 
This class can be implemented by application developers, although this is not necessary, as BeanNameUrlHandlerMapping and SimpleUrlHandlerMapping are included in the framework. The former is the default if no HandlerMapping bean is registered in the application context. 

HandlerMapping implementations can support mapped interceptors but do not have to. A handler will always be wrapped in a HandlerExecutionChain instance, optionally accompanied by some HandlerInterceptor instances. The DispatcherServlet will first call each HandlerInterceptor's preHandle method in the given order, finally invoking the handler itself if all preHandle methods have returned true. 

The ability to parameterize this mapping is a powerful and unusual capability of this MVC framework. For example, it is possible to write a custom mapping based on session state, cookie state or many other variables. No other MVC framework seems to be equally flexible. 

Note: Implementations can implement the Ordered interface to be able to specify a sorting order and thus a priority for getting applied by DispatcherServlet. Non-Ordered instances get treated as lowest priority.
```

1.初始化HandlerMapping

2.获取HandlerExecutionChain



SimpleUrlHandlerMapping

可设置url与controller的映射关系（mappings）

BeanNameUrlHandlerMapping



DefaultAnnotationHandlerMapping 

注解映射器:org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping 