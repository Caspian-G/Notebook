# Add MyBatis

needed

```
<denpendency> mybatis-spring-boot-starter
<version> 3.0.4

<denpendency> mysql-connector-java
<version> 8.0.33
```

**注**  <scope> runtime  //声明该依赖在编译时不需要 在运行时才需要 保证java代码与database驱动解耦

#### 数据库连接配置

###### application.yml

```
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo?serverTimeZone=UTC  //此处demo为database名
    username: ...
    password: ...
    driver-class-name: com.mysql.cj.jdbc.Driver  //驱动
```

#### 实体类用于对应数据库结构

```
public class User{  //User为例
	private int id;
	private String username;
	private String password;
	...getter&setter;
	...toString();
}
```

#### 法一 纯interface

###### *Mapper.class

```
@Mapper
public interface *Mapper{
	@Select("select * from t_user")  //Select注解为例
	List<User> findAll();
}
```

###### 使用bean

```
@Autowired
private UserMapper userMapper;  //UserMapper为例
public ... ...(...){
	...
	List<User> list = userMapper.findAll();
	...
}
```

#### 法二 interface+xml

###### *Mapper.class

```
@Mapper
public interface *Mapper{
	List<User> findAll();
}
```

###### application.yml 添加配置

```
mybatis:
  mapper-locations: classpath:mapper/*Mapper.xml  #通配符标记多文件
  type-aliases-package: com.example.demo.entities  #实体类软件包路径
  #config-location: 指定mybatis的核心配置文件
```

###### *Mapper.xml

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.demo.mapper.UserXmlMapper">
    <select id="findAll" resultType="com.example.demo.entities.User">
        select * from t_user
    </select>
</mapper>
```

**注** namespace里为Mapper接口路径(必须是全限定名) select中id需与*Mapper.class中方法名一致 resultType=实体类路径

**注**  *Mapper.class 接口应尽量同 *Mapper.xml 分别在java和res的同路径下 当然也可不放同路径 具体取决于namespace和.yml中mybatis配置是否正确
