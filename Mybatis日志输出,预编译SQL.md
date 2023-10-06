在Mybatis当中我们可以借助日志，查看到sql语句的执行、执行传递的参数以及执行结果。具体操作如下：
1. 打开application.properties文件
2. 开启mybatis的日志，并指定输出到控制台
```properties
#指定mybatis输出日志的位置, 输出控制台
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```
开启日志之后，我们再次运行单元测试，可以看到在控制台中，输出了以下的SQL语句信息：

![image-20220901164225644](assets/image-20220901164225644.png) 

> 但是我们发现输出的SQL语句：delete from emp where id = ?，我们输入的参数16并没有在后面拼接，id的值是使用?进行占位。那这种SQL语句我们称为**预编译SQL**

预编译SQL有两个优势：
1. 性能更高
2. 更安全(防止SQL注入)




