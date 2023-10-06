AOP的作用：在不修改源代码的基础上对已有方法进行增强（解耦）。
1. 导入依赖：在pom.xml中导入AOP的依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
2. 编写AOP程序：针对于特定方法根据业务需要进行编程

切面所在的类，我们一般称为**切面类**（被`@Aspect`注解标识的类）
**切面：Aspect**，（切入点+通知）。通过切面就能够描述当前aop程序需要针对于哪个原始方法，在什么时候执行什么样的操作。
**切入点：PointCut**，匹配连接点的条件，通知仅会在切入点方法执行时被应用。切入点表达式来描述切入点。
**通知：Advice**，指哪些重复的逻辑，也就是共性功能（最终体现为一个方法）
```java
    //环绕通知
    @Around("pt()")//注解是切入点，下面的方法是通知。
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        log.info("around before ...");
        //调用目标对象的原始方法执行
        Object result = proceedingJoinPoint.proceed();
        //原始方法在执行时：发生异常,后续代码不在执行
        log.info("around after ...");
        return result;
    }
```
**连接点：JoinPoint**，可以被AOP控制的方法
**目标对象：Target**，通知所应用的方法对应的对象

Spring中AOP的通知类型：
- @Around：环绕通知，此注解标注的通知方法在目标方法前、后都被执行
- @Before：前置通知，此注解标注的通知方法在目标方法前被执行
- @After ：后置通知，此注解标注的通知方法在目标方法后被执行，无论是否有异常都会执行
- @AfterReturning ： 返回后通知，此注解标注的通知方法在目标方法后被执行，有异常不会执行
- @AfterThrowing ： 异常后通知，此注解标注的通知方法发生异常后执行
注意事项：
- @Around环绕通知需要自己调用 proceedingJoinPoint.proceed() 来让原始方法执行，其他通知不需要考虑目标方法执行。
- @Around环绕通知方法的返回值，必须指定为Object，来接收原始方法的返回值，否则原始方法执行完毕，是获取不到返回值的。
```java
Object result = proceedingJoinPoint.proceed();
```

Spring提供了`@PointCut`注解，将公共的切入点表达式抽取出来，需要时引用该切入点表达式即可。
```java
    //（公共的切入点表达式）
    @Pointcut("execution(* com.itheima.service.*.*(..))")
    private void pt(){}
    //环绕通知引用
    @Around("pt()")
    //外部引用，全类名.方法名()
    @Before("com.itheima.aspect.MyAspect1.pt()")
```

多个切面类中多个切入点都匹配到了同一个目标方法,控制通知的执行顺序：
使用Spring提供的`@Order`注解：`@Order(2)`  切面类的执行顺序（前置通知：数字越小先执行; 后置通知：数字越小越后执行）

**切入点表达式：** 决定项目中的哪些方法需要加入通知
- execution切入点表达式
    
    - 根据我们所指定的方法的描述信息来匹配切入点方法，这种方式也是最为常用的一种方式
    - 如果我们要匹配的切入点方法的方法名不规则，或者有一些比较特殊的需求，通过execution切入点表达式描述比较繁琐
    - `execution(访问修饰符?  返回值  包名.类名.?方法名(方法参数) throws 异常?)`,`?`前可以省略
    - `@Before("execution(void com.itheima.service.impl.DeptServiceImpl.delete(java.lang.Integer))")`
    -  `*` ：单个独立的任意符号，可以通配任意返回值、包名、类名、方法名、任意类型的一个参数，也可以通配包、类、方法名的一部分
    - `..` ：多个连续的任意符号，可以通配任意层级的包，或任意类型、任意个数的参数

- annotation 切入点表达式
    
    - 基于注解的方式来匹配切入点方法。这种方式虽然多一步操作，我们需要自定义一个注解，但是相对来比较灵活。我们需要匹配哪个方法，就在方法上加上对应的注解就可以了
    
    - //前置通知,自定义注解：MyLog,引用注解用全类名
    - ` @Before("@annotation(com.itheima.anno.MyLog)")`

在Spring中用JoinPoint抽象了连接点，用它可以**获得目标方法执行时的相关信息**，如目标类名、方法名、方法参数等。
- 对于@Around通知，获取连接点信息只能使用**ProceedingJoinPoint类型的形参**
- 对于其他四种通知，获取连接点信息只能使用**JoinPoint的形参**，它是ProceedingJoinPoint的父类型
