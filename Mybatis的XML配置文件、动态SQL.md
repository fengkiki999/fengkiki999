Mybatis的开发有两种方式：
1. 注解，**是来完成一些简单的增删改查功能。**
2. XML，**实现复杂的SQL功能。**

在Mybatis中使用**XML映射文件**方式开发，需要符合一定的规范：
1. XML映射文件的名称与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下（**同包同名**）
2. XML映射文件的**namespace属性**与Mapper接口全限定名一致
3. XML映射文件中sql语句的**id与Mapper接口中的方法名一致，并保持返回类型一致。**
![[Pasted image 20231005140927.png]]
- resultType属性，指的是查询返回的单条记录所封装的类型。




## XML配置文件实现
第1步：创建XML映射文件，参考上面三个规范。

第2步：编写XML映射文件
xml映射文件中的dtd约束，直接从mybatis官网复制即可
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">
 
</mapper>
```
配置：XML映射文件的namespace属性为Mapper接口全限定名
配置：XML映射文件中sql语句的id与Mapper接口中的方法名一致，并保持返回类型一致

## **动态SQL**
`<if>`：用于判断条件是否成立。使用test属性进行条件判断，如果条件为true，则拼接SQL。
使用`<set>`标签代替SQL语句中的set关键字
使用`<where>`标签代替SQL语句中的where关键字
`<set>`：动态的在SQL语句中插入set关键字，并会删掉额外的逗号。（用于update语句中）
`<where>`只会在子元素有内容的情况下才插入where子句，而且会自动去除子句的开头的AND或OR
```xml
<select id="list" resultType="com.itheima.pojo.Emp">
        select * from emp
        <where>
             <!-- if做为where标签的子元素 -->
             <if test="name != null">
                 and name like concat('%',#{name},'%')
             </if>
             <if test="gender != null">
                 and gender = #{gender}
             </if>
             <if test="begin != null and end != null">
                 and entrydate between #{begin} and #{end}
             </if>
        </where>
        order by update_time desc
</select>

 <!--更新操作-->
    <update id="update">
        update emp
        <!-- 使用set标签，代替update语句中的set关键字 -->
        <set>
            <if test="username != null">
                username=#{username},
            </if>
            <if test="name != null">
                name=#{name},
            </if>
            <if test="gender != null">
                gender=#{gender},
            </if>
            <if test="image != null">
                image=#{image},
            </if>
            <if test="job != null">
                job=#{job},
            </if>
            <if test="entrydate != null">
                entrydate=#{entrydate},
            </if>
            <if test="deptId != null">
                dept_id=#{deptId},
            </if>
            <if test="updateTime != null">
                update_time=#{updateTime}
            </if>
        </set>
        where id=#{id}
    </update>
```

## **foreach**
SQL语句：
```mysql
delete from emp where id in (1,2,3);
```
Mapper接口：
```java
@Mapper
public interface EmpMapper {
    //批量删除
    public void deleteByIds(List<Integer> ids);
}
```
使用`<foreach>`遍历deleteByIds方法中传递的参数ids集合
![[Pasted image 20231005161908.png]]
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">
    <!--删除操作-->
    <delete id="deleteByIds">
        delete from emp where id in
        <foreach collection="ids" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>
</mapper> 
```

## 动态SQL-sql&include
xml映射文件中配置的SQL，有时可能会存在很多重复的片段，此时就会存在很多冗余的代码
我们可以对重复的代码片段进行抽取，将其通过`<sql>`标签封装到一个SQL片段，然后再通过`<include>`标签进行引用。
- `<sql>`：定义可重用的SQL片段
- `<include>`：通过属性refid，指定包含的SQL片段

SQL片段： 抽取重复的代码
```xml
<sql id="commonSelect">
 	select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp
</sql>
```
然后通过`<include>` 标签在原来抽取的地方进行引用。操作如下：
```xml
<select id="list" resultType="com.itheima.pojo.Emp">
    <include refid="commonSelect"/>
    <where>
        <if test="name != null">
            name like concat('%',#{name},'%')
        </if>
        <if test="gender != null">
            and gender = #{gender}
        </if>
        <if test="begin != null and end != null">
            and entrydate between #{begin} and #{end}
        </if>
    </where>
    order by update_time desc
</select>
```


![[Pasted image 20231005162519.png]]
