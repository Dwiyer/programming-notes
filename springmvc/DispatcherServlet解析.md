Q1：DispatcherServlet的启动入口在哪？



Q2：DispatcherServlet是如何与Spring Context联系起来的？



##### DispatcherServlet初始化

DispatcherServlet其实就是个Servlet（它继承自HttpServlet基类），同样也需要在你web应用的web.xml配置文件下声明。你需要在web.xml文件中把你希望DispatcherServlet处理的请求映射到对应的URL上去。这就是标准的Java EE Servlet配置；

a.DispatcherServlet如何与IoC容器、Servlet容器集成？
1>使用spring mvc 启动了两个context：***Root WebApplicationContext*** 和 ***Servlet WebapplicationContext***。
2>***ContextLoaderListener*** 创建基于web的应用根 applicationContext 并将它放入到ServletContext. applicationContext加载或者卸载spring管理的beans。在structs和spring mvc的控制层都是这样使用的。称之为Root WebapplicationContext。
3>DispatcherServlet创建自己的WebApplicationContext并管理这个WebApplicationContext里面的 handlers/controllers/view-resolvers。称之为Servlet WebapplicationContext。



在context成功的refresh过后，DispatcherServlet的`onRefresh` 方法就会被调用，然后它会调用`initStrategies` 方法。



##### DispatcherServlet继承关系

###### HttpServletBean



###### FrameworkServlet

LocaleContext  LocaleContextHolder <!--获取Locale-->

ServletRequestAttributes->RequestAttributes  RequestContextHoler <!--管理request和session属性-->

WebAsyncManager  WebAsyncUtils

*service()*

  *processRequest()*  <!--是FrameworkServlet类在处理请求中最核心的方法。对LocaleContext和RequestAttributes的设置及恢复-->

​    *initContextHolders()*    <!--设置LocaleContext和RequestAttributes-->

​    *doService()*  <!--模板方法，实际处理请求入口-->

​    *resetContextHolders()* <!--恢复LocaleContext和RequestAttributes-->

​    *publishRequestHandledEvent()* <!--发布ServletRequestHandledEvent类型的消息-->





##### DispatcherServlet执行流程解析

###### 1.初始化

```java
/**
	 * Initialize the strategy objects that this servlet uses.
	 * <p>May be overridden in subclasses in order to initialize further strategy objects.
	 */
	protected void initStrategies(ApplicationContext context) {
		initMultipartResolver(context);
		initLocaleResolver(context);
		initThemeResolver(context);
		initHandlerMappings(context);
		initHandlerAdapters(context);
		initHandlerExceptionResolvers(context);
		initRequestToViewNameTranslator(context);
		initViewResolvers(context);
		initFlashMapManager(context);
	}
```

单个resolver
initMultipartResolver，initLocaleResolver，initThemeResolver，initRequestToViewNameTranslator，initFlashMapManager 这五个初始化方法流程相同，都是使用
context.getBean(String name, Class<FlashMapManager> requiredType)的方式获取到相应的Resolver。



多个resolver

initHandlerMappings，initHandlerAdapters，initHandlerExceptionResolvers，initViewResolvers 获取方式相同，使用：
BeanFactoryUtils.beansOfTypeIncludingAncestors(ListableBeanFactory lbf, Class<HandlerMapping> type, boolean includeNonSingletons, boolean allowEagerInit)
的方式获取到相应的Resolver。

###### 2.提供服务

Servlet提供服务具体分4步：

1. 保存现场。保存request 熟悉的快照，以便能在必要时恢复。

2. 将框架需要的对象放入request中，以便view和handler使用。

3. 请求分发服务。（doDispatch）

4. 恢复现场。

*请求分发过程如下：*

1. 判断是否设置了multipart resolver，设置的话转换为multipart request，没有的话则继续下面的步骤。

2. 根据当前request，获取hangdler。

3. 根据当前request，获取HandlerAdapter。

4. 如果支持http请求头，处理 last-modified header请求头。

5. 应用已注册interceptor的preHandle方法

6. HandlerAdapter处理请求。

7. 设置默认视图。

8. 应用已注册interceptor的postHandle方法。

9. 处理异常或者视图渲染。





##### 静态资源处理





