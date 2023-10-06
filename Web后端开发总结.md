所谓控制反转，指的是将对象创建的控制权由应用程序自身交给外部容器，这个容器就是我们常说的IOC容器或Spring容器。

而DI依赖注入指的是容器为程序提供运行时所需要的资源。

![[Pasted image 20231006121255.png]]
我们在学习这些web后端开发技术的时候，我们都是基于主流的SpringBoot进行整合使用的。而SpringBoot又是用来简化开发，提高开发效率的。像过滤器、拦截器、IOC、DI、AOP、事务管理等这些技术到底是哪个框架提供的核心功能？
![[Pasted image 20231006121302.png]]
Filter过滤器、Cookie、 Session这些都是传统的JavaWeb提供的技术。
JWT令牌、阿里云OSS对象存储服务，是现在企业项目中常见的一些解决方案。
IOC控制反转、DI依赖注入、AOP面向切面编程、事务管理、全局异常处理、拦截器等，这些技术都是 Spring Framework框架当中提供的核心功能。
Mybatis就是一个持久层的框架，是用来操作数据库的。


在Spring框架的生态中，对web程序开发提供了很好的支持，如：全局异常处理器、拦截器这些都是Spring框架中web开发模块所提供的功能，而Spring框架的web开发模块，我们也称为：SpringMVC
![[Pasted image 20231006122028.png]]
SpringMVC不是一个单独的框架，它是Spring框架的一部分，是Spring框架中的**web开发模块**，是用来简化原始的Servlet程序开发的。

外界俗称的SSM，就是由：SpringMVC、Spring Framework、Mybatis三块组成。

基于传统的SSM框架进行整合开发项目会比较繁琐，而且效率也比较低，所以在现在的企业项目开发当中，基本上都是直接基于SpringBoot整合SSM进行项目开发的。