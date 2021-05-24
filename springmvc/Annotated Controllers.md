#### 注解

------

**@RequestMapping**

It has various attributes to match by URL, HTTP method, request parameters, headers, and media types. You can use it at the class level to express *shared mappings* or at the method level to narrow down to a specific endpoint mapping.



##### 方法注解

**@SessionAttributes**

`@SessionAttributes` is used to store model attributes in the HTTP Servlet session between requests. It is a type-level annotation that declares the session attributes used by a specific controller. This typically lists the names of model attributes or types of model attributes that should be transparently stored in the session for subsequent requests to access.

**@SessionAttribute**

If you need access to pre-existing session attributes that are managed globally (that is, outside the controller — for example, by a filter) and may or may not be present, you can use the `@SessionAttribute` annotation on a method parameter.

**@RequestAttribute**

Similar to `@SessionAttribute`, you can use the `@RequestAttribute` annotations to access pre-existing request attributes created earlier (for example, by a Servlet `Filter` or `HandlerInterceptor`).

**@RequestBody**

You can use the `@RequestBody` annotation to have the request body read and deserialized into an `Object` through an [`HttpMessageConverter`](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#rest-message-conversion). 

You can use the [Message Converters](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config-message-converters) option of the [MVC Config](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config) to configure or customize message conversion.

You can use `@RequestBody` in combination with `javax.validation.Valid` or Spring’s `@Validated` annotation, both of which cause Standard Bean Validation to be applied. By default, validation errors cause a `MethodArgumentNotValidException`, which is turned into a 400 (BAD_REQUEST) response. Alternatively, you can handle validation errors locally within the controller through an `Errors` or `BindingResult` argument.

@**RequestParam**

该注解有三个主要的作用：给变量取别名；设置变量是否必填；给变量设置默认值；

@**PathVariable**



**@ResponseBody**

Writes directly to the response body versus view resolution and rendering with an HTML template.

You can use the `@ResponseBody` annotation on a method to have the return serialized to the response body through an[HttpMessageConverter](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#rest-message-conversion). 

`@ResponseBody` is also supported at the class level, in which case it is inherited by all controller methods. This is the effect of `@RestController`, which is nothing more than a meta-annotation marked with `@Controller` and `@ResponseBody`.

You can use `@ResponseBody` with reactive types. See [Asynchronous Requests](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-async) and [Reactive Types](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-async-reactive-types) for more details.

You can use the [Message Converters](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config-message-converters) option of the [MVC Config](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config) to configure or customize message conversion.

You can combine `@ResponseBody` methods with JSON serialization views. See [Jackson JSON](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-jackson) for details.



**@ModelAttribute**

You can use the `@ModelAttribute` annotation:

- On a [method argument](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-modelattrib-method-args) in `@RequestMapping` methods to create or access an `Object` from the model and to bind it to the request through a `WebDataBinder`.
- As a method-level annotation in `@Controller` or `@ControllerAdvice` classes that help to initialize the model prior to any `@RequestMapping` method invocation.
- On a `@RequestMapping` method to mark its return value is a model attribute.

This section discusses `@ModelAttribute` methods — the second item in the preceding list. A controller can have any number of `@ModelAttribute` methods. All such methods are invoked before `@RequestMapping` methods in the same controller. A `@ModelAttribute` method can also be shared across controllers through `@ControllerAdvice`. See the section on [Controller Advice](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-controller-advice)for more details.

`@ModelAttribute` methods have flexible method signatures. They support many of the same arguments as `@RequestMapping`methods, except for `@ModelAttribute` itself or anything related to the request body.



**@InitBinder**

`@Controller` or `@ControllerAdvice` classes can have `@InitBinder` methods that initialize instances of `WebDataBinder`, and those, in turn, can:

- Bind request parameters (that is, form or query data) to a model object.
- Convert String-based request values (such as request parameters, path variables, headers, cookies, and others) to the target type of controller method arguments.
- Format model object values as `String` values when rendering HTML forms.

`@InitBinder` methods can register controller-specific `java.beans.PropertyEditor` or Spring `Converter` and `Formatter`components. In addition, you can use the [MVC config](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config-conversion) to register `Converter` and `Formatter` types in a globally shared `FormattingConversionService`.

`@InitBinder` methods support many of the same arguments that `@RequestMapping` methods do, except for `@ModelAttribute`(command object) arguments. Typically, they are declared with a `WebDataBinder` argument (for registrations) and a `void` return value. 



@**ExceptionHandler**

`@Controller` and [@ControllerAdvice](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-controller-advice) classes can have `@ExceptionHandler` methods to handle exceptions from controller methods.

The exception may match against a top-level exception being propagated (e.g. a direct `IOException` being thrown) or against a nested cause within a wrapper exception (e.g. an `IOException` wrapped inside an `IllegalStateException`). As of 5.3, this can match at arbitrary cause levels, whereas previously only an immediate cause was considered.

For matching exception types, preferably declare the target exception as a method argument, as the preceding example shows. When multiple exception methods match, a root exception match is generally preferred to a cause exception match. More specifically, the `ExceptionDepthComparator` is used to sort exceptions based on their depth from the thrown exception type.

Alternatively, the annotation declaration may narrow the exception types to match, You can even use a list of specific exception types with a very generic argument signature.



#### 类组件

------

##### 方法参数

**HttpEntity**







##### 方法返回值

**ResponseEntity**

`ResponseEntity` is like [`@ResponseBody`](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-responsebody) but with status and headers. 

Spring MVC supports using a single value [reactive type](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-async-reactive-types) to produce the `ResponseEntity` asynchronously, and/or single and multi-value reactive types for the body.	









##### 配置类





#### 参数映射

------

HTTP 请求中，Query String Parameters 参数可以直接映射到控制器方法中的基本类型参数，POJO对象类型参数



##### 简单类型绑定

简单类型的绑定中，方法形参中的参数名要和前台传进来的名一样才能完成参数的绑定。那有人要问了，如果有特殊需求（比如更好的可读性？），这里定义的参数名就是不一样，可以使用注解@RequestParam对简单的类型进行参数绑定



##### 对象类型绑定

对于基本类型，前台页面传进来的name要和要封装的pojo对象属性名一致，然后就可以将该pojo作为形参放到controller的方法中

对于日期等特殊类型，需要定义转换器Converter



##### 嵌套对象类型绑定

使用点字符路径形式作为参数键值传入绑定



##### 集合类型绑定



##### Map类型绑定



