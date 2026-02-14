# IDEA模板快速创建SpringBoot项目

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
@ConfigurationProperties("map名")  //()内的内容可指定.yaml文件中的具体map
public class Person{} //需与代取内容同名的成员变量及其getter和setter
```

