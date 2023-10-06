Mybatis操作数据库的步骤：
1. 准备工作(创建springboot工程、数据库表user、实体类User)
2. 引入Mybatis的相关依赖（mybatis起步依赖，mysql驱动包依赖），**配置Mybatis(数据库连接信息)**
3. **编写SQL语句(注解/XML)**

**配置Mybatis:**
在springboot项目中，可以编写application.properties文件，配置数据库连接信息。我们要连接数据库，就需要配置数据库连接的基本信息，包括：driver-class-name、url 、username，password。
```xml
#驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#数据库连接的url
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
#连接数据库的用户名
spring.datasource.username=root
#连接数据库的密码
spring.datasource.password=1234
```

 **编写SQL语句(注解/XML)**
@Mapper注解：表示是mybatis中的Mapper接口
程序运行时：框架会自动生成接口的实现类对象(代理对象)，并给交Spring的IOC容器管理
默认我们在UserMapper接口上的@Select注解中编写SQL语句是没有提示的。如果想让idea给出提示，可以做如下配置：
![[Pasted image 20231005121700.png]]

还存在不识别表名(列名)的情况：产生原因：Idea和数据库没有建立连接，不识别表信息。 解决方案：在Idea中配置MySQL数据库连接。在配置的时候指定连接那个数据库，如下图所示连接的就是mybatis数据库。
![[Pasted image 20231005121750.png]]
