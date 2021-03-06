### Mybatis 概述

MyBatis是一个Java持久化框架，它通过XML描述符或注解把对象与存储过程或SQL语句关联起来。
中文官方网址：http://www.mybatis.org/mybatis-3/zh/getting-started.html

### Mybatis 功能概况

与其他的对象关系映射框架不同，MyBatis并没有将Java对象与数据库表关联起来，而是将Java方法与SQL语句关联。MyBatis允许用户充分利用数据库的各种功能，例如存储过程、视图、各种复杂的查询以及某数据库的专有特性。如果要对遗留数据库、不规范的数据库进行操作，或者要完全控制SQL的执行，MyBatis是一个不错的选择。

与JDBC相比，MyBatis简化了相关代码：SQL语句在一行代码中就能执行。MyBatis提供了一个映射引擎，声明式的把SQL语句执行结果与对象树映射起来。通过使用一种内建的类XML表达式语言，或者使用Apache Velocity集成的插件，SQL语句可以被动态的生成。

MyBatis与Spring Framework和Google Guice集成，这使开发者免于依赖性问题。

MyBatis支持声明式数据缓存（declarative data caching）。当一条SQL语句被标记为“可缓存”后，首次执行它时从数据库获取的所有数据会被存储在一段高速缓存中，今后执行这条语句时就会从高速缓存中读取结果，而不是再次命中数据库。MyBatis提供了默认下基于Java HashMap的缓存实现，以及用于与OSCache、Ehcache、Hazelcast和Memcached连接的默认连接器。MyBatis还提供API供其他缓存实现使用。

### Mybatis 用法

SQL语句存储在XML文件或Java 注解中。一个MaBatis映射的示例（其中用到了Java接口和MyBatis注解）：

```
package org.mybatis.example;

public interface BlogMapper {
    @Select("select * from Blog where id = #{id}")
    Blog selectBlog(int id);
}
```

执行的示例：

```
BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = mapper.selectBlog(101);
```
SQL语句和映射也可以外化到一个XML文件中：
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="org.mybatis.example.BlogMapper">
    <select id="selectBlog" parameterType="int" resultType="Blog">
        select * from Blog where id = #{id}
    </select>
</mapper>
```
也可以使用MyBatis API执行语句：
```
Blog blog = session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
```
详细信息可以参考MyBatis网站所提供的用户手册。参见外部链接。

### 与Spring集成

MyBatis与Spring Framework集成。Spring Framework允许MyBatis参与Spring事务，创建了MyBatis映射器和会话，并把他们注入到其他bean中。

如下是一个基本的XML配置示例：创建了映射器，并注入到“BlogService”bean中。
```
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
</bean>

<bean id="blogMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="sqlSessionFactory" ref="sqlSessionFactory" />
    <property name="mapperInterface" value="org.mybatis.example.BlogMapper" />
</bean>

<bean id="blogService" class="org.mybatis.example.BlogServiceImpl">
    <property name="blogMapper" ref="blogMapper" />
</bean>
```
现在调用MyBatis只需要调用一个bean:
```
public class BlogServiceImpl implements BlogService {

    private BlogMapper blogMapper;

    public void setBlogMapper(BlogMapper blogMapper) {
        this.blogMapper = blogMapper;
    }

    public void doSomethingWithABlog(int blogId) {
        Blog blog = blogMapper.selectBlog(blogId);
        ...
    }
}
```

### Velocity语言

Velocity语言驱动程序允许用户使用Apache Velocity来快速生成动态SQL查询。
```
<select id="findPerson" lang="velocity">
  #set( $pattern = $_parameter.name + '%' )
  SELECT *
  FROM person
  WHERE name LIKE @{pattern, jdbcType=VARCHAR}
</select>
```
### MyBatis生成器

MyBatis提供了代码生成器。MyBatis生成器（MyBatis Generator）能对数据库表内省，生成执行的增删改查（CRUD）时所需的MyBatis代码。有相关的Eclipse插件可供使用。

### MyBatis Migrations

MyBatis Migrations 是一个Java控制台应用程序，它通过管理数据定义语言（DDL）文件来跟踪数据库模式的变更。

Migrations可以查询当前数据库的状态，应用或恢复对数据库模式的变更。它也有助于发现和解决由多个开发人员并行修改数据库模式的情况。
