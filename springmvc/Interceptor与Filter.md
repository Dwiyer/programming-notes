Interceptor与Filter

SpringMVC 中的Interceptor 拦截请求是通过HandlerInterceptor 来实现的。在SpringMVC 中定义一个Interceptor 非常简单，主要有两种方式，第一种方式是要定义的Interceptor类要实现了Spring 的HandlerInterceptor 接口，或者是这个类继承实现了HandlerInterceptor 接口的类，比如Spring 已经提供的实现了HandlerInterceptor 接口的抽象类HandlerInterceptorAdapter ；第二种方式是实现Spring的WebRequestInterceptor接口，或者是继承实现了WebRequestInterceptor的类。

HandlerInterceptor 接口中定义了三个方法，我们就是通过这三个方法来对用户的请求进行拦截处理的。

  （1 ）preHandle (HttpServletRequest request, HttpServletResponse response, Object handle) 方法。该方法将在请求处理之前进行调用。SpringMVC 中的Interceptor 是链式的调用的，在一个应用中或者说是在一个请求中可以同时存在多个Interceptor 。每个Interceptor 的调用会依据它的声明顺序依次执行，而且最先执行的都是Interceptor 中的preHandle 方法，所以可以在这个方法中进行一些前置初始化操作或者是对当前请求的一个预处理，也可以在这个方法中进行一些判断来决定请求是否要继续进行下去。该方法的返回值是布尔值Boolean 类型的，当它返回为false 时，表示请求结束，后续的Interceptor 和Controller 都不会再执行；当返回值为true 时就会继续调用下一个Interceptor 的preHandle 方法，如果已经是最后一个Interceptor 的时候就会是调用当前请求的Controller 方法。

  （2 ）postHandle (HttpServletRequest request, HttpServletResponse response, Object handle, ModelAndView modelAndView) 方法，由preHandle 方法的解释我们知道这个方法包括后面要说到的afterCompletion 方法都只能是在当前所属的Interceptor 的preHandle 方法的返回值为true 时才能被调用。postHandle 方法，顾名思义就是在当前请求进行处理之后，也就是Controller 方法调用之后执行，但是它会在DispatcherServlet 进行视图返回渲染之前被调用，所以我们可以在这个方法中对Controller 处理之后的ModelAndView 对象进行操作。postHandle 方法被调用的方向跟preHandle 是相反的，也就是说先声明的Interceptor 的postHandle 方法反而会后执行，这和Struts2 里面的Interceptor 的执行过程有点类型。Struts2 里面的Interceptor 的执行过程也是链式的，只是在Struts2 里面需要手动调用ActionInvocation 的invoke 方法来触发对下一个Interceptor 或者是Action 的调用，然后每一个Interceptor 中在invoke 方法调用之前的内容都是按照声明顺序执行的，而invoke 方法之后的内容就是反向的。

  （3 ）afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handle, Exception ex) 方法，该方法也是需要当前对应的Interceptor 的preHandle 方法的返回值为true 时才会执行。顾名思义，该方法将在整个请求结束之后，也就是在DispatcherServlet 渲染了对应的视图之后执行。这个方法的主要作用是用于进行资源清理工作的。 我们的系统日志的拦截在这个方法中，可以记录日志的相关的参数，检测方法的执行。

```
public interface HandlerInterceptor
Workflow interface that allows for customized handler execution chains. Applications can register any number of existing or custom interceptors for certain groups of handlers, to add common preprocessing behavior without needing to modify each handler implementation.
A HandlerInterceptor gets called before the appropriate HandlerAdapter triggers the execution of the handler itself. This mechanism can be used for a large field of preprocessing aspects, e.g. for authorization checks, or common handler behavior like locale or theme changes. Its main purpose is to allow for factoring out repetitive handler code.

In an async processing scenario, the handler may be executed in a separate thread while the main thread exits without rendering or invoking the postHandle and afterCompletion callbacks. When concurrent handler execution completes, the request is dispatched back in order to proceed with rendering the model and all methods of this contract are invoked again. For further options and details see org.springframework.web.servlet.AsyncHandlerInterceptor

Typically an interceptor chain is defined per HandlerMapping bean, sharing its granularity. To be able to apply a certain interceptor chain to a group of handlers, one needs to map the desired handlers via one HandlerMapping bean. The interceptors themselves are defined as beans in the application context, referenced by the mapping bean definition via its "interceptors" property (in XML: a <list> of <ref>).

HandlerInterceptor is basically similar to a Servlet 2.3 Filter, but in contrast to the latter it just allows custom pre-processing with the option of prohibiting the execution of the handler itself, and custom post-processing. Filters are more powerful, for example they allow for exchanging the request and response objects that are handed down the chain. Note that a filter gets configured in web.xml, a HandlerInterceptor in the application context.

As a basic guideline, fine-grained handler-related preprocessing tasks are candidates for HandlerInterceptor implementations, especially factored-out common handler code and authorization checks. On the other hand, a Filter is well-suited for request content and view content handling, like multipart forms and GZIP compression. This typically shows when one needs to map the filter to certain content types (e.g. images), or to all requests.
```

```
public interface Filter
A filter is an object that performs filtering tasks on either the request to a resource (a servlet or static content), or on the response from a resource, or both. 

Filters perform filtering in the doFilter method. Every Filter has access to a FilterConfig object from which it can obtain its initialization parameters, a reference to the ServletContext which it can use, for example, to load resources needed for filtering tasks.

Filters are configured in the deployment descriptor of a web application

Examples that have been identified for this design are
1) Authentication Filters 
2) Logging and Auditing Filters 
3) Image conversion Filters 
4) Data compression Filters 
5) Encryption Filters 
6) Tokenizing Filters 
7) Filters that trigger resource access events 
8) XSL/T filters 
9) Mime-type chain Filter
```

interceptor 和filter的概念相似，但主要不同点有：

web应用的过滤请求，仅使用web应用；

interceptor应用于特定组别的handler，可以web应用也可以企业应用；

从google找到的资料：http://www.linkedin.com/groups/what-is-difference-between-interceptor-3983267.S.5844715100472107010

Filter is used only in web applications whereas interceptor can be used with web as well as enterprise applications. Life cycle methods of both, also differs. The Interceptor stack fires on requests in a configured package while filters only apply to their mapped URL's. 

Example: 

A Servlet Filter is used in the web layer only, you can't use it outside of a 
web context. Interceptors can be used anywhere. 

The interceptor stack fires on every request. 
Filters only apply to the urls for which they are defined. 

Filters can be used when you want to modify any request or response parameters like headers. For example you would like to add a response header "Powered By Surya" to each generated response. Instead of adding this header in each resource method you would use a response filter to add this header. 

There are filters on the server side and the client side. 

In Summary: 

Filters: 

(1)Based on Servlet Specification 
(2)Executes on the pattern matches on the request. 
(3) Not configurable method calls. 


Interceptors: 
(1)Based on Struts2. 
(2)Executes for all the request qualifies for a front controller( A Servlet filter ).And can be configured to execute additional interceptor for a particular action execution. 
(3)Methods in the Interceptors can be configured whether to execute or not by means of excludemethods or includeMethods





