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





#### 类组件

------

##### 方法参数

**HttpEntity**







##### 方法返回值

**ResponseEntity**

`ResponseEntity` is like [`@ResponseBody`](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-responsebody) but with status and headers. 

Spring MVC supports using a single value [reactive type](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-async-reactive-types) to produce the `ResponseEntity` asynchronously, and/or single and multi-value reactive types for the body.	









##### 配置类













