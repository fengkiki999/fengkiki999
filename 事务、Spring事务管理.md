数据库阶段事务的操作主要有三步：
1. 开启事务（一组操作开始前，开启事务）：start transaction / begin ;
2. 提交事务（这组操作全部成功后，提交事务）：commit ;
3. 回滚事务（中间任何一个操作出现异常，回滚事务）：rollback ;
**Spring事务管理：** 
spring框架当中已经把事务控制的代码都封装好了。我们使用了spring框架，只需要通过一个简单的注解`@Transactional`就搞定了。
* @Transactional作用：当前业务实现类中的所有的方法，都添加了spring事务管理机制。就是在当前这个方法执行开始之前来开启事务，方法执行完毕之后提交事务。如果在这个方法执行的过程当中出现了异常，就会进行事务的回滚操作。
* @Transactional注解：我们一般会在业务层当中来控制事务，因为在业务层当中，一个业务功能可能会包含多个数据访问的操作。在业务层来控制事务，我们就可以将多个数据访问操作控制在一个事务范围内。

@Transactional注解书写位置：
- 方法 
    - 当前方法交给spring进行事务管理
- 类
    - 当前类中所有的方法都交由spring进行事务管理
- 接口
    - 接口下所有的实现类当中所有的方法都交给spring 进行事务管理

说明：可以在application.yml配置文件中开启事务管理日志，这样就可以在控制看到和事务相关的日志信息了
```yml
#spring事务管理日志
logging:
  level:
    org.springframework.jdbc.support.JdbcTransactionManager: debug
```

**事务进阶:** 
@Transactional注解当中的两个常见的属性：
1. 异常回滚的属性：rollbackFor
* 默认情况下，只有出现RuntimeException(运行时异常)才会回滚事务。
* 通过rollbackFor这个属性可以指定出现何种异常类型回滚事务。
*  `@Transactional(rollbackFor=Exception.class)`

2. 事务传播行为：propagation
对于这些事务传播行为，我们只需要关注两个就可以了
- REQUIRED（默认值）：表示有事务就加入，没有则新建事务。大部分情况下都用该传播行为。

- REQUIRES_NEW：不论是否有事务，都创建新事务 ，运行在一个独立的事务中。
- `@Transactional(propagation = Propagation.REQUIRES_NEW)`当我们不希望事务之间相互影响时，可以使用该传播行为。比如：下订单前需要记录日志，不论订单保存成功与否，都需要保证日志记录能够记录成功。
