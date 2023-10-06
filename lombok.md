通过注解的形式自动生成构造器、getter/setter、equals、hashcode、toString等方法，并可以自动化生成日志变量，简化java开发、提高效率。
![[Pasted image 20231005124446.png]]
**使用步骤**
**1. 在pom.xml文件中引入依赖**
```xml
<!-- 在springboot的父工程中，已经集成了lombok并指定了版本号，故当前引入依赖时不需要指定version -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```
**2. 在实体类上添加注解**
在实体类上添加了@Data注解，那么这个类在编译时期，就会生成getter/setter、equals、hashcode、toString等方法。

Lombok的注意事项：
- Lombok会在编译时，会自动生成对应的java代码
- 在使用lombok时，还需要安装一个lombok的插件（新版本的IDEA中自带）

**3. 使用Lombok的`@Slf4j`注解来生成日志变量，并在类中使用它来记录日志**
在类上添加了`@Slf4j`注解。这将自动为该类生成一个名为`log`的Logger变量。
```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class MyLoggerExample {

    public static void main(String[] args) {
        log.trace("This is a trace message.");
        log.debug("This is a debug message.");
        log.info("This is an info message.");
        log.warn("This is a warning message.");
        log.error("This is an error message.");
    }
}

```

