# Servlet

## 1. Servlet是什么

​    Servlet是Java Servlet的简称，称为小服务程序或服务连接器，是用Java编写的服务器端程序，运行于Servlet容器中(比如Tomcat，提供Servlet代码的运行环境，起到web服务器的作用)，具有独立于平台和协议的特性，主要功能在于交互式地浏览和生成数据，生成动态Web内容，即处理前台的请求并返回数据。狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口)的类，一般情况下，Servlet指后者。从原理上讲，Servlet可以响应任何类型的请求，但绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器。

​     service() 方法是 Servlet 的核心，每当一个客户请求一个HttpServlet 对象时，该对象的service() 方法就要被调用，而且传递给这个方法一个请求对象(ServletRequest)和一个响应对象(ServletResponse)作为参数，Servlet提供这两个参数获取前端传来的参数并返回对应的数据。在 HttpServlet 中已存在 service() 方法，默认的服务功能是调用与HTTP请求的方法相应的do功能。例如，如果HTTP请求方法为GET，则缺省情况下就调用 doGet() 。Servlet应该为Servlet支持的HTTP方法覆盖do功能。因为HttpServlet.service()方法会检查请求方法是否调用了适当的处理方法，不必要覆盖service()方法。只需覆盖相应的do方法就可以了。

## 2. Servlet的生命周期

​    Servlet生命周期指从创建直到毁灭的整个过程，分为三个步骤：初始化------>处理请求------>销毁。创建Servlet对象后---------->**初始化**（调用 init ()方法）--------->**处理/响应客户端的请求**（调用 service()方法）--------->**销毁**（调用 destroy()方法，最后由 JVM 的垃圾回收器进行垃圾回收）

#### 1. 初始化阶段

​    当客户端向Servlet容器发出 HTTP 请求要求访问Servlet时，Servlet容器首先会解析请求，检查内存中是否已经有了该Servlet对象，如果有，则直接使用该 Servlet 对象，如果没有，则加载该Servlet到内存并创建实例对象，然后通过调用 init()方法实现Servlet的初始化工作。

​    需要注意的是，在Servlet的整个生命周期内，它的init()方法只能被调用一次。

#### 2. 运行阶段（处理请求）

​    这是 Servlet 生命周期中最重要的阶段，在这个阶段中，Servlet 容器会为这个请求创建代表 HTTP 请求的 ServletRequest 对象和代表 HTTP 响应的 ServletResponse 对象，然后将它们作为参数传递给 Servlet 的 service() 方法。

​    service() 方法从 ServletRequest 对象中获得客户请求信息并处理该请求，通过 ServletResponse 对象生成响应结果。在 Servlet 的整个生命周期内，对于 Servlet 的每一次访问请求，Servlet 容器都会调用一次 Servlet 的 service() 方法，并且创建新的 ServletRequest 和 ServletResponse 对象，也就是说，service() 方法在 Servlet 的整个生命周期中会被调用多次。

#### 3. 销毁阶段

​    当服务器关闭或 Web 应用被移除出容器时，Servlet 随着 Web 应用的关闭而销毁。在销毁 Servlet 之前，Servlet 容器会调用 Servlet 的 destroy() 方法，以便让 Servlet 对象释放它所占用的资源。在 Servlet 的整个生命周期中，destroy() 方法也只能被调用一次。

​    需要注意的是，Servlet 对象一旦创建就会驻留在内存中等待客户端的访问，直到服务器关闭或 Web 应用被移除出容器时，Servlet 对象才会销毁。

![img](https://img.php.cn/upload/image/859/931/869/1677130717818216.png)

# Controller

## 1. 什么是Controller

​    这里的Controller特指Spring框架下的由Spring IOC管理的，负责处理前台请求、接收返回数据及视图跳转的一类bean，在Spring框架中处于MVC三层架构的控制器层，起到与Servlet类似的作用。

## 2. Controller是Servlet吗

​    很多人在学习和使用Controller的时候会认为它是一个servlet，但其实并不是！

​    DisPatcherServlet是Spring中唯一的Servlet，Servlet将所有请求都转发到DisPatcherServlet，然后DisPatcherServlet分发请求通过HandlerMapping(处理器映射器)和HandlerAdapter(处理器适配器)找到具体的Controller。因此可见，Controller只是一个普通的JavaBean，只是在Spring框架下通过注解及DisPatcherServlet的调度令它能够实现与Servlet相似的功能，同时由于只用专注于业务层代码，Controller要比Servlet更加自由方便。

​    Controller的具体执行流程看下面两张图，也是Spring框架处理请求的流程：

![img](https://img-blog.csdnimg.cn/20210505134350250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDkwMjQ4OQ==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/ae7c5ce916524819ad62ab9bf1705508.png)