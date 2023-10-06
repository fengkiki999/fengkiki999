使用Spring提供的注解：@Component ，就可以实现类交给IOC容器管理

Spring框架为了更好的标识web应用程序开发当中，bean对象到底归属于哪一层，又提供了@Component的衍生注解：
- @Controller （标注在控制层类上）@RestController = @Controller + @ResponseBody
- @Service （标注在业务层类上）
- @Repository （标注在数据访问层类上）
@Component（不属于以上三类时，用此注解）

在IOC容器中，每一个Bean都有一个属于自己的名字，可以通过注解的value属性指定bean的名字。如果没有指定，默认为类名首字母小写。

**组件扫描**
问题：使用前面学习的四个注解声明的bean，一定会生效吗？
答案：不一定。（原因：bean想要生效，还需要被组件扫描）
- 使用四大注解声明的bean，要想生效，还需要被组件扫描注解@ComponentScan扫描
- @ComponentScan注解虽然没有显式配置，但是实际上已经包含在了引导类声明注解 @SpringBootApplication 中，==**默认扫描的范围是SpringBoot启动类所在包及其子包**==。

手动添加@ComponentScan注解，指定要扫描的包 （==仅做了解，不推荐==）
推荐做法（如下图）：
将我们定义的controller，service，dao这些包呢，都放在引导类所在包com.itheima的子包下，这样我们定义的bean就会被自动的扫描到
![[Pasted image 20231005190902.png]]


#### DI
@Autowired这个注解，完成依赖注入的操作。
@Autowired注解，默认是按照**类型**进行自动装配的（去IOC容器中找某个类型的对象，然后完成注入操作）
如果在IOC容器中，存在多个相同类型的bean对象，Spring提供了以下几种解决方案：
- @Primary 当存在多个相同类型的Bean注入时，加上@Primary注解，来确定默认的实现。
![[Pasted image 20231005191421.png]]
- @Qualifier 指定当前要注入的bean对象。 在@Qualifier的value属性中，指定注入的bean的名称。@Qualifier注解不能单独使用，必须配合@Autowired使用
![[Pasted image 20231005191515.png]]
- @Resource 按照bean的名称进行注入。通过name属性指定要注入的bean的名称。
![[Pasted image 20231005191600.png]]

