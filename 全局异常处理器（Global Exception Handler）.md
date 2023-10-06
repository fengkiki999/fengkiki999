
在三层构架项目中，出现了异常，该如何处理?
- 方案一：在所有Controller的所有方法中进行try…catch处理
    - 缺点：代码臃肿（不推荐）
- 方案二：全局异常处理器
    - 好处：简单、优雅（推荐）
![[Pasted image 20231005112400.png]]

定义全局异常处理器
1. 定义一个类，在类上加上一个注解`@RestControllerAdvice`，加上这个注解就代表我们定义了一个全局异常处理器。
2. 在全局异常处理器当中，需要定义一个方法来捕获异常，在这个方法上需要加上注解@ExceptionHandler。通过@ExceptionHandler注解当中的value属性来指定我们要捕获的是哪一类型的异常。
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    //处理异常
    @ExceptionHandler(Exception.class) //指定能够处理的异常类型
    public Result ex(Exception e){
        e.printStackTrace();//打印堆栈中的异常信息

        //捕获到异常之后，响应一个标准的Result
        return Result.error("对不起,操作失败,请联系管理员");
    }
}
```
@RestControllerAdvice = @ControllerAdvice + @ResponseBody
处理异常的方法返回值会转换为json后再响应给前端

全局异常处理器的使用，主要涉及到两个注解：
- @RestControllerAdvice //表示当前类为全局异常处理器
- @ExceptionHandler //指定可以捕获哪种类型的异常进行处理