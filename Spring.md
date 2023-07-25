​    spring是为了解决企业级应用开发的复杂性而创建的，简化开发。可以这么理解：spring简化了Java ee复杂的架构和开发方式，而spring boot简化了配置，内置了servlet容器，集成了其他技术及框架（如Redis等）。

​    spring是一个轻量级的管理Java bean的容器，主要通过自身的IOC和AOP两大特性来管理Java bean及其特性（如生命周期等）。spring mvc则是spring的一个子框架，主要用于调用相对于的controller来处理前台发来的请求。可以这么理解：spring是管理service和dao的容器，而spring mvc是管理controller对象的容器，两者常常配合使用，spring管理dao层和service层的Java bean，而spring mvc管理的controller通过调用service层接口处理请求。

​    需要注意的是，dao，service和controller并不对应mvc架构中的三层。dao的service同属于mvc的model层（还有实体类也属于这一层），其中dao层直接与数据库交互，service层负责完成业务逻辑的设计，这样可以有效降低代码耦合性，方便拓展。controller可以看做对应mvc的controller层，而mvc的view层则是指前端的视图。一般在项目的流程为：Controller层调用Service层的方法，Service层调用Dao层中的方法，其中调用的参数是使用实体层进行传递的。

```Java
Controller-->service接口-->serviceImpl-->dao接口-->daoImpl-->mapper-->db
```

​    springboot则是把spring、spring mvc、spring data jpa等等的一些常用的常用的基础框架组合起来，提供默认的配置，然后提供可插拔的设计，就是各种starter，来方便开发者使用这一系列的技术。套用官方的一句话，spring 家族发展到今天，已经很庞大了，作为一个开发者，如果想要使用spring 家族一系列的技术，需要一个一个的搞配置，然后还有个版本兼容性问题，其实挺麻烦的，偶尔也会有小坑出现，容易影响开发进度。spring boot 就是来解决这个问题，提供了一个一站式解决方案，可以先不关心如何配置，可以快速的启动开发，进行业务逻辑编写，各种需要的技术可以加入starter配置直接使用，追求开箱即用的效果吧。

​    spring boot 可以支持你快速的开发出restful 风格的微服务架构，但是这还不够啊，还要实现云应用，于是有了spring cloud。spring Cloud是一个基于Spring Boot实现的云应用开发工具，它为基于JVM的云应用开发中的配置管理、服务发现、断路器、智能路由、微代理、控制总线、全局锁、决策竞选、分布式会话和集群状态管理等操作提供了一种简单的开发方式。

