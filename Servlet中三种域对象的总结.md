# 什么是域对象？

有的同学听到“域对象”这个词感到很高级，其实没那么复杂。**存储数据的区域**就称为“域对象”。

打个比喻：你家有一个池塘，池塘里面有鱼，有虾，有螃蟹等等。这些水产品就是数据，整个池塘就是“域对象”。你可以将一些鱼、虾放进池塘里，也可以从池塘里捞出来。这就类比于从域对象中存取数据。

域对象也有大有小，不同的域对象有着不同的作用范围。

Servlet有三个域对象，分别为：request，application( ServletContext )，session。下面我就来介绍下这三种作用域的作用范围。

------

# request

request对象是一个域对象，只要发送一个请求就会发送request对象。这个域对象的作用范围仅在本次请求中有效。即

- 何时创建：HTTP请求时
- 何时销毁：HTTP响应结束时
- 域的作用范围：一次请求中

在request域中存取数据：

```java
request.setAttribute("name","zhangsan");
request.getAttribute("name");
```

这个request域有什么用呢？

答：可以用在请求“转发”场景下。当前Servlet对象A收到一个HTTP请求时，转发到另一个Servlet对象B进行处理，此时B Servlet对象内的request、response还是A对象内那一对，请求没有改变，则request域就没有改变。在A中set的key-value对，在B中可以取出来使用。

# session

session表示会话，Tomcat会为每一个会话创建一个session对象。

session对象的生命周期：

session何时创建：用户第一次访问服务器时创建，值得注意的是：访问JSP、Servlet等程序时才会创建Session，只访问HTML、IMAGE等静态资源时并不会

session过期/失效：

1. 服务器会把长时间没有活动的session从服务器内存中清楚，此时Session便失效。Tomcat中Session的默认失效时间为20分钟。
2. 手动销毁session：session.invalidate();
3. Tomcat非正常关闭时。

Session作用范围：默认在一次会话中，即一次会话中的任何资源公用一个session对象。





# application(ServletContext)

ServletContext 代表一个web应用的环境对象。它的作用范围是整个web应用，即所有的web资源（比如Servlet对象、静态文件）都可以随意向ServletContext域中存取数据。

在ServletContext域中存取数据：

```java
ServletContext context1 = getServletContext();
context1.setAttribute("name","zhangsan");  //将键值对放入context域中

ServletContext context2 = getServletContext();
context2.getAttribute("name");   //从context域中取值
```

注意：服务器只会创建一个ServletContext对象，所以context1、context2所引用的是同一个context对象。