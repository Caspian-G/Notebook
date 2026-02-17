# IDEA模板快速创建SpringBoot项目  依赖  配置文件

图形化操作 start.spring.io

项目依赖 spring boot reference guide

#### 具体依赖

```
<parent> spring-boot-starter-parent //定义各技术版本信息
```

```
<dependency> spring-boot-starter-web
```

```
<dependency> spring-boot-starter-test
```

spring-boot-starter-parent的父父依赖的dependencymanagement会定义各技术版本

spring-boot-starter-web&test若不定义版本 会跟随父依赖中版本设置

#### 配置文件 三种写法

application.properties          server.port=8080         优先级最高

application.yml                      server: {port: 8080}

application.yaml                    server: {port: 8080}

对于springboot .yml和.yaml无异

##### yaml数据格式

对象(map):

```
person:
  name: abc
  age: 20
or
person: {name: abc, age: 20}
```

数组:

```
address:
  - beijing
  - shanghai
```

纯量:

```
msg1: 'hello \n world' # 单引不会识别转义字符
msg2: "hello \n world" # 双引会识别转义字符
```

参数引用(该方法无法用于花括号map):

```
name: example
person:
  name: ${name}  # 变成person: {name: example}
  age: 20
```

#### 读取配置文件内容

@Value 适用于单次读取

```
@Value("${server.port}")
private String port;
```

Environment  适用于多次

```
@Autowired
private Environment env;
"server.port".sout;
```

@ConfigurationProperties

```
Person.java:
@Component
@ConfigurationProperties(prefix = "map名")  //()内的内容可指定.yaml文件中的具体map
public class Person{} //需与代取内容同名的成员变量及其getter和setter
```

​	if出现友好提示缺少spring-boot-configuration-processor

```
<dependency> spring-boot-configuration-processor  //用于在编写.yml文件时提供提示词
```

#### Profiles切换不同环境下的配置文件

##### 配置

多文件配置( 严格命名 )

###### application-dev.properties  //开发

###### applciation-test.properties  //测试

###### application-pro.properties  //生产

.yml单文件配置

```
---
//配置信息
spring:
  profiles: dev
---
//配置信息
spring:
  profiles: test
---
//配置信息
spring:
  profiles: pro
---
```

##### 激活 ( 程序实参方式 > 虚拟机方式 > 配置文件方式 )

配置文件中激活

```
spring.profiles.active=dev  //.properties
```

```
spring:
  profiles:
    active: dev  //.yml
```

虚拟机方式

```
-Dspring.profiles.active=...  //Run - Edit Configuration中修改
```

程序实参方式

```
--spring.profiles.active=...  //Run - Edit Configuration中修改
```

**注** 实际生产环境不会有IDE参与 采用maven package 在target文件夹下cmd激活

```
cmd: java (-Dspring.profiles.active=...) -jar jar包name.jar (--spring.profiles.active=...)
```

#### 项目内部配置文件加载顺序

冲突时优先级：与.idea同级的config\application.properties > 与.idea同级的application.properties > src\main\res\config\application.properties > src\main\res\application.properties

**注** 与.idea同级的配置文件或config下的配置文件在模块被package后不会被打包 也就是说使用命令行形式运行.jar时配置信息取决于src子文件夹内的application.properties

#### 项目外部配置文件加载顺序 (冲突优先级：外部 > 内部)

冲突优先级：命令行参数(java -jar ....jar --server.port=...) > 特定配置文件位置(java -jar ....jar --spring.config.location=D://...) > target文件夹下与.jar同级的config\application.properties > target下与.jar同级的application.properties

