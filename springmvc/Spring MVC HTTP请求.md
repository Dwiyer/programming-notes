#### 请求类型

------

##### HTTP请求参数

###### Query String Parameters

放置在url的末尾的普通key/value键值对



###### Request Payload(Request Body)

放置在HTTP请求体，json格式的键值对



###### Form Data





**contentType=application/json**

如果在Request Payload传递的json字符串（JSON.stringify(jsonData)），Controller需使用@RequestBody（使用json库进行解析）将json字符串转换为对象。



**contentType=application/x-www-form-urlencoded**

如果在Request Payload直接传递普通的键值对，则无需使用@RequestBody注解，Spring MVC可直接绑定对应参数。





使用Query String Parameters传递参数，也无需使用@RequestBody注解。