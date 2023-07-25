# session

​    session是JSP九大内置对象之一，其他八个分别为：request、response、out、pageContext、application、page、config、exception，比较常用的有request、response、session、out这几个。这些对象可以在JSP页面中调用，而不需要事先定义。实际上这些内置对象都对应着某个Servlet类，在JSP被翻译成Servlet之后，这些内置对象会相应转换成对应的类实例。

​    session中文翻译为“会话”，是服务器的一个对象，跟cookie一样用于保存用户信息。因为cookie是客户端保存的数据，而这些数据又是跟用户强烈相关联的，显然保存在客户端这边就不太合适(太多，也占资源)，所以把数据都保存在服务器这边就比较的合适；保存的方式就是通过session的方式来进行保存的。

# cookie

​    cookie是浏览器提供的一种让程序员在本地存储数据的能力，让数据在客户端这边更持久化。Cookie里面存的是键值对的格式数据，键值对用“；”分割，键和值之间用“，”分割。浏览器里面存的Cookie都是从服务器的响应“报头”里面的 set-cookie 字段中来的，每个 set-cookie 字段里面都包含一个cookie 这样的键值对，浏览器拿到响应之后就会把 set-cookie中的内容保存到本地，而 set-cookie 就是程序员自己在服务器中构造填写的。

   经典cookie运用场景，保存用户登录信息：

![image-20220409153516176](https://img-blog.csdnimg.cn/img_convert/6decc66224feb9673f53d53c88ed4f08.png)



# session和cookie的区别

    1. cookie是客户端（浏览器）存储数据的一种机制，键值对结构，可以存储身份信息，也是可以存储关键的信息，由程序员自定义。cookie信息保存在本地的浏览器目录下，可以设置过期时间，过期后会被浏览器自动删除。
       2. session是服务器存储数据的一种机制，键值对结构的，主要用来存储身份相关的信息。session信息存储于服务器，服务器重新启动过后，session信息就会没有，需要重新登录获取信息。会话结束（如关闭浏览器）是session信息也会被销毁。session也可以设置过期时间，过期后会被服务器自动销毁。
       3. cookie和session经常在一起配合使用，但也不是必须配合。