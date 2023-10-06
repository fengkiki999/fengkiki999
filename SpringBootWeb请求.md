![[Pasted image 20231005173841.png]]
BS架构：Browser/Server，浏览器/服务器架构模式。
在后端程序中，如何接收传递过来的普通参数数据呢？
我们在这里讲解两种方式：
1. **原始方式 。仅做了解 : 

在原始的Web程序当中，需要通过Servlet中提供的API：HttpServletRequest（请求对象），获取请求的相关信息。比如获取请求参数：
Tomcat接收到http请求时：把请求的相关信息封装到HttpServletRequest对象中
在Controller中，我们要想获取Request对象，可以直接在方法的形参中声明 HttpServletRequest 对象。然后就可以通过该对象来获取请求信息：
```java
@RestController
public class RequestController {
    //原始方式
    @RequestMapping("/simpleParam")
    public String simpleParam(HttpServletRequest request){
        // http://localhost:8080/simpleParam?name=Tom&age=10
        // 请求参数： name=Tom&age=10   （有2个请求参数）
        // 第1个请求参数： name=Tom   参数名:name，参数值:Tom
        // 第2个请求参数： age=10     参数名:age , 参数值:10

        String name = request.getParameter("name");//name就是请求参数名
        String ageStr = request.getParameter("age");//age就是请求参数名

        int age = Integer.parseInt(ageStr);//需要手动进行类型转换
        System.out.println(name+"  :  "+age);
        return "OK";
    }
}
```

## 2. SpringBoot方式

Controller类中接受请求路径重复时，可以在Controller类上面提炼重复的
@RequestMapping("/emps")

#### 1. 简单参数，参数名与形参变量名相同，定义同名的形参即可接收参数。
请求参数名和controller方法中的形参名不相同，怎么办？
解决方案：
在方法形参前面加上Spring提供的@RequestParam然后通过value属性执行请求参数名，从而完成映射。
```java
// http://localhost:8080/simpleParam?name=Tom&age=10
    // 第1个请求参数： name=Tom   参数名:name，参数值:Tom
    // 第2个请求参数： age=10     参数名:age , 参数值:10
    
    //springboot方式
    @RequestMapping("/simpleParam")
    public String simpleParam(String name , Integer age ){//形参名和请求参数名保持一致

// http://localhost:8080/simpleParam?name=Tom&age=20
    // 请求参数名：name

    //springboot方式
    @RequestMapping("/simpleParam")
    public String simpleParam(@RequestParam("name") String username , Integer age ){//请求参数名和形参名不相同


public String simpleParam(@RequestParam(name = "name", required = false) String username, Integer age){

```
@RequestParam中的**required属性**默认为true（默认值也是true），代表该请求参数必须传递，如果不传递将报错。如果该参数是可选的，可以将required属性设置为false。
**@RequestParam(defaultValue="默认值")   //设置请求参数默认值**

#### 2. 实体参数
在使用简单参数做为数据传递方式时，前端传递了多少个请求参数，后端controller方法中的形参就要书写多少个。如果请求参数比较多，通过上述的方式一个参数一个参数的接收，会比较繁琐。
我们可以考虑将请求参数封装到一个实体类对象中。 要想完成数据封装，需要遵守如下规则：**请求参数名与实体类的属性名相同**

**简单实体对象**：定义POJO实体类
**复杂实体对象**：在实体类中有一个或多个属性，也是实体对象类型的。
- **请求参数名与形参对象属性名相同，按照对象层次结构关系即可接收嵌套实体类属性参数。**


#### 3. 数组集合参数
数组集合参数的使用场景：在HTML的表单中，有一个表单项是支持多选的(复选框)，可以提交选择的多个值。

后端程序接收上述多个值的方式有两种：
1. 数组
2. 集合

数组参数：**请求参数名与形参数组名称相同且请求参数为多个，定义数组类型形参即可接收参数**
![[Pasted image 20231005175931.png]]
集合参数：**请求参数名与形参集合对象名相同且请求参数为多个，@RequestParam 绑定参数关系**
默认情况下，请求中参数名相同的多个值，是封装到数组。如果要封装到集合，要使用@RequestParam绑定参数关系
![[Pasted image 20231005180059.png]]

#### 4. 日期参数
因为日期的格式多种多样（如：2022-12-12 10:05:45 、2022/12/12 10:05:45），那么对于日期类型的参数在进行封装的时候，需要通过@DateTimeFormat注解，以及其pattern属性设置日期的格式。
![[Pasted image 20231005180309.png]]
- @DateTimeFormat注解的pattern属性中指定了哪种日期格式，前端的日期参数就必须按照指定的格式传递。
- 后端controller方法中，需要使用Date类型或LocalDateTime类型，来封装传递的参数。


#### 5. JSON参数
在学习前端技术时，我们有讲到过JSON，而在前后端进行交互时，如果是比较复杂的参数，前后端通过会使用JSON格式的数据进行传输。 （JSON是开发中最常用的前后端数据交互方式）

服务端Controller方法接收JSON格式数据：
- 传递json格式的参数，在Controller中会使用实体类进行封装。
- 封装规则：**JSON数据键名与形参对象属性名相同，定义POJO类型形参即可接收参数。需要使用 @RequestBody标识。**
![[Pasted image 20231005182638.png]]


#### 6. 路径参数
路径参数：
- 前端：通过请求URL直接传递参数
- 后端：使用{…}来标识该路径参数，需要使用@PathVariable获取路径参数
![[Pasted image 20231005182901.png]]
