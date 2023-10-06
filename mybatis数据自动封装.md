- **实体类属性名和数据库表查询返回的字段名一致，mybatis会自动封装。**
- **如果实体类属性名和数据库表查询返回的字段名不一致，不能自动封装。**

解决方案：
1. 起别名
2. 结果映射
3. **开启驼峰命名**

**开启驼峰命名(推荐)**：如果字段名与属性名符合驼峰命名规则，mybatis会自动通过驼峰命名规则映射

> 驼峰命名规则： abc_xyz => abcXyz
> 
> - 表中字段名：abc_xyz
>     
> - 类中属性名：abcXyz
>     

**在application.properties中添加：**  
```properties
# 在application.properties中添加：
mybatis.configuration.map-underscore-to-camel-case=true
```

> 要使用驼峰命名前提是 实体类的属性 与 数据库表中的字段名严格遵守驼峰命名。

