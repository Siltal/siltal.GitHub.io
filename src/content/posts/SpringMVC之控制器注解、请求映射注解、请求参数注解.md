---
title: SpringMVC之控制器注解、请求映射注解、请求参数注解
published: 2022-03-02 
tags: [Java, SpringMVC, Http]
category: Java
draft: false
---


## SpringMVC之控制器注解、请求映射注解、请求参数注解

### 控制器注解 `@Controller`

这是一个普通的类

```java
class Normal {
    public String getHtml() {
        return "Hello World!";
    }
}
```

在类名前一行加上控制器注解
```java
@Controller
class Normal {
    public String getHtml() {
        return "Hello World!";
    }
}
```
SpringMVC 将此类识别为控制器。


### 请求映射注解 `@RequestMapping`

但这样只加一个控制器注解是不能正常使用的,程序需要知道它接收什么路径的请求，再调用这个方法 (getHtml)。
```java
@Controller
class Normal {
    @RequestMapping("/")
    public String getHtml() {
        return "Hello World!";
    }
}
```

此时我们可以访问 `http://localhost:8080/`
浏览器显示的却是
> **Whitelabel Error Page**
>
> This application has no explicit mapping for /error, so you are seeing this as a fallback.
>
> Wed Mar 02 17:14:22 CST 2022
> There was an unexpected error (type=Not Found, status=404).
> No message available

那是因为没有添加 `@ResponseBody` 注解，所以程序不能返回内容。
但此时方法体依然是**执行**了的。



### 响应体注解 `@ResponseBody`

如果你修改为
```java
@Controller
class Normal {
    @RequestMapping("/")
    public String getHtml() {
        System.out.println("test1");
    }
}
```
我们可以在控制台看到 `test1` ，可以证明方法是执行了的。

那好，我们再在方法前加上 `@ResponseBody`

```java
@Controller
class Normal {
    @RequestMapping("/")
    @ResponseBody
    public String getHtml() {
        return "Hello World!";
    }
}
```
再去访问 `http://127.0.0.1:8080/`，就可以看到浏览器显示的是 `Hello World!`，使用了改方法的返回值作为网页 (html)。

由于是网页，我们可以在其中构造（写入）html,css,js 等代码，但不在本文讨论范围内。



### 详细解释

我们可以注意到@RequestMapping注解中有一个参数,它指定了**当用户访问的路径是何值时**，**调用**被此 `@RequestMapping` 注解的**方法**。
它是一个默认参数，这个参数名是 value (或 path)   也就是说
```java
@RequestMapping("/a")
等价于
@RequestMapping(value = "/a")
等价于
@RequestMapping(path = "/a")
```



`@RequestMapping` 的 `value` 属性还可以是一个 `String[]` 类型的字符串列表

假设有如下代码
```java
@Controller
class Normal {
    @RequestMapping(value={
        "/",
        "/www",
        "/my"
    })
    @ResponseBody
    public String getHtml() {
        return "Hello World!";
    }
}
```
那么我们可以访问三个网址，指向同一个页面
```yml
http://127.0.0.1:8080/
http://127.0.0.1:8080/www/
http://localhost:8080/my/
```
> 127.0.0.1 == localhost


类似的，我们可以在类的前一行加上这个注解，这就相当于在每个方法前的"请求映射注解的路径"前加上类前注解的路径，比如
```java
@Controller
@RequestMapping("/home")
class Normal {
    @RequestMapping("/my")
    @ResponseBody
    public String getHtml() {
        return "Hello World!";
    }
}
```
这样我们需要访问 `http://127.0.0.1:8080/home/my` 才能访问到该网页。
> RequestMapping 的 value 值大小写敏感。



将以上三点组合起来...

```java
@Controller
@RequestMapping(path = {"A","B"})
public class MyController {
    @RequestMapping(value = {"1","2"})
    @ResponseBody
    public String getHtml() {
        System.out.println("1");
        return "Hello World!";
    }
}
```
则以下链接都可以访问到该网页,它是两个路径参数的有序组合
```yml
http://127.0.0.1:8080/A/1
http://127.0.0.1:8080/B/1
http://127.0.0.1:8080/A/2
http://127.0.0.1:8080/B/2
```

### @RequestMapping 的 params 参数

params参数用于缩小请求的匹配范围

例如，在访问同一路径时

```java
@RequestMapping(value = "/",params = "id")
```
意味着该参数 `id` 必须存在于请求中。

```java
@RequestMapping(value = "/",params = "id=114514")
```
意味着该参数 `id` 必须存在于请求中，并且值为 `114514` 。

> 注意：访问同一路径下，不能同时有两种歧义的判定

例如

```java
package io.github.siltal.web;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;


@Controller
public class MyController {
    static void log(Integer i) {
        log(String.valueOf(i));
    }

    static void log(String message) {
        System.out.println(message);
    }

    @RequestMapping(value = "/")
    @ResponseBody
    public String getHtml_default(String id) {
        //没有参数限制，不出现参数中不含键id即可进入此方法
        log("default");
        return "default";
    }
    @RequestMapping(value = "/",params = "id")
    @ResponseBody
    public String getHtml0(String id) {
        //参数中含有id就会进入此方法
        log("has 'id' parameter");
        return "has 'id' parameter";
    }


    @RequestMapping(value = "/",params = "id=1")
    @ResponseBody
    public String getHtml1(String id) {
        //参数中含有id并且值为1就会进入此方法,与参数中含有id就会进入方法 getHtml0 冲突
        log("parameter 'id' = 1 ");
        return "parameter 'id' = 1 ";
    }
    
}
```

### @RequestMapping 的 Method 
当@RequestMapping含有 `method` 属性，则该方法**只能**接收指定的 `Http Method` ，
可以在相同的映射路径值下，区分不同的 `method` 。
```java

@Controller
public class MyController {
    @RequestMapping(value="/",method = RequestMethod.GET)
    @ResponseBody
    public String getHtml(@RequestParam("uname") String username,@RequestParam("pwd")String password){
        return String.format("Username: [%s]<br>Password: [%s]",username,password);
    }
}
```
可以指定请求过滤以下请求方法
```java
public enum RequestMethod {
    GET,
    HEAD,
    POST,
    PUT,
    PATCH,
    DELETE,
    OPTIONS,
    TRACE;

    private RequestMethod() {
    }
}

```

### Http 请求方式 与 GET 请求的传参方式
什么是Http请求方式
HTTP 定义了一组请求方法，以表明要对给定资源执行的操作。指示针对给定资源要执行的期望动作。虽然他们也可以是名词, 但这些请求方法有时被称为HTTP动词。

[wikipeidia](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)

[MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

常用的 `GET` 方法，和 `POST` 方法：
 - GET:
   - 使用 `http://url[:port]/path/to/site?key1=value1&key2=value2` 的形式发送数据，数据长度不大，大约为 2000 个字符（取决于浏览器和服务器）
   - 暴露于URL的参数，不安全
 - POST:
   - POST的数据在 `RequestBody` 中，数据长度大，1MB 到 4GB（取决于浏览器和服务器）
   - 比GET数据安全一些，取决于请求体的数据加密


被 `@RequestMapping`与`@ResponseBody` 注解的方法处理传入的数据，并返回网页文档字符串
```java
@Controller
public class MyController {
    @RequestMapping("/")
    @ResponseBody
    public String getHtml(String username,String password){
        return String.format("你输入的用户名是%s<br>你输入的密码是%s",username,password);
    }
}
```
GET 参数中的字段名与方法的形参名相同,浏览器访问`http://127.0.0.1:8080/?username=45&password=114514`即可。

我们可以使用 `@RequestParam` 注解来给参数做映射，从GET参数的字段名映射到方法的形参名
```java
@Controller
public class MyController {
    @RequestMapping("/")
    @ResponseBody
    public String getHtml(@RequestParam("uname") String username,@RequestParam("pwd")String password){
        return String.format("Username: [%s]<br>Password: [%s]",username,password);
    }
}
```
则我们应该访问 `http://127.0.0.1:8080/?uname=45&pwd=114514` 


### @PathVariable
RESTful 风格
使用占位符获取参数


### 组合注解
一个注解，两个功能
```java
@Getmapping("/demo3_3")
等价于
@RequestMapping(value="/demo3",method = RequestMethod.GET)
```

类似的还有
```java
@RequestMapping
@DeleteMapping
@PutMapping
@GetMapping
@PatchMapping
@PostMapping
```






