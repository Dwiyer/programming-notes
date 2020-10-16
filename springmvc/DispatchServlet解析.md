DispatchServlet解析

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