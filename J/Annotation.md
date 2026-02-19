# Annotation

#### 系统提供

```
@Deprecated  //弃用
@SuppressWarning  //压制警告
```

#### 自定义

```
@Target(ElementType.TYPE)  //放在类上
//ElementType.FIELD 放成员变量上 ElementType.METHOD 放方法上 ElementType.CONSTRUCTOR放构造方法上
@Retention(RetentionPolicy.RUNTIME)  //运行时生效
//RetentionPolicy.CLASS 类(字节码)时生效 RetentionPolicy.Source 源码时(.java)生效
@interface MyAnnotation{
	String test() default "test";  //可理解成变量 设置默认值"test"
}
```

#### 使用

```
@MyAnnotation(value = "abc")  //value为默认变量 如果只写值只能赋值给value 需注解接口声明value
@MyAnnotation  //直接使用默认值
```

#### 通过反射获取Annotation

思路：1 获取元数据 2 获取元数据中注解

```
Class c = person.getClass();
c.isAnnotationPresent(MyAnnotation.class);  //判断是否有某注解 括号内指定注解接口名
MyAnnotation m = (MyAnnotation)c.getAnnotation(MyAnnotation.class);  //必须强制进行类型转换，因为c.getAnnotation()返回Annotation类型，Annotation类没有获取注解内输入的值的方法
m.value();  //获取注解内输入数据
```

