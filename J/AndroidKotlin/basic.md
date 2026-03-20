##### 运行原理

kotlin 同c++一样是一门静态类型语言 即编译前所有变量的类型就已经确定(与python不同)

由于JVM只认.class不在乎.class怎么来的 所以kotlin使用自己的编译器生成.class即可运行

kotlin更像java的补集而非超集

kotlin的函数是一等公民

**注** 一等公民要求 (1)能存进变量 (2)能当成参数传走 (3)能当成返回值 (4)能动态创建

##### Compile and Run

kotlinc(也是由java写的) 最低要求本地有JDK17

环境变量设置后查看版本

```
kotlinc.bat -version
```

编译 法一

```
kotlinc demo.kt -include-runtime -d demo.jar  //-include-runtime指将kotlin运行环境(包含kotlin标准库)打包到jar包 -d指目的地
```

运行 法一

```
java -jar demo.jar  //java命令会启动os的一个进程即JVM
```

编译 法二

```
kotlinc demo.kt  //编译生成不包含kotlin标准库的.class文件  额外生成的META-INF是告诉JVM如何处理代码的说明书
```

运行 法二

```
kotlin DemoKt.class  //这里依赖kotlin环境 如果直接使用java运行JVM会报错(找不到kotlin标准库)
```

#### 语法

##### 变量

```
//kotlin存在类型推导机制
var a = 1  //var声明可变变量  无需结尾加";"
val a = 1  //val声明不可变变量(final)
//也可显式声明类型
var a: Int = 1  //kotlin不再使用基本类型 全部使用对象类型需全部大写开头
```

##### 函数

```
//无参无返回值
fun demo(){}
//有参有返回值
fun demo(a: Int, b: Int): Int{
	return a + b
}
//当函数只有一行代码时
fun demo(a: Int, b: Int): Int = a + b
//kotlin有类型推导机制 返回类型可省略
fun demo(a: Int, b: Int) = a + b
```

##### if

kotlin中if是有返回值的

无三元运算符

```
//形式与java无异
fun max(a: Int, b: Int): Int {
    if (a > b) return a
    else return b
}
//if也可以被作为返回值
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```

**注** 当把if赋值给一个变量时 if 必须跟 else 否则报错

**注** kotlin中==等价于java中.equals()     ===等价于java中==

##### when  类似switch

kotlin中when有返回值

```
val x = 1
when (x) {
    1 -> println("x == 1")  //不需要写break kotlin会自动处理
    2 -> println("x == 2")
    else -> println("x is neither 1 nor 2")
}
```

**注** 当把when赋值给一个变量时 when 最后必须跟 else 否则报错

**注** "case"后面不一定要接常数 还可以是各种表达式

```
//when还可进行模式匹配 java17+也支持
fun checkNumber(num: Number) {
    when (num) {
        is Int -> println("Int")
        is Double -> println("Double")
        else -> println("others")
    }
}
//when还可不要参数 直接使用外部变量 不同于java
fun getScore(name: String) = when {
    name == "Tom" -> "不及格"
    name == "Jim" -> "及格"
    name == "Pony" -> "良好"
    name == "Tony" -> "优秀"
    else -> "名字非法"
}
```

##### 循环

```
//while
while(){}  //与java类似
do{}while()  //与java类似 但java中{}内定义的变量在()里是访问不到的 kotlin中{}内定义的变量在()可以访问
//for-in
fun main() {
	val range = 0 until 10 step 1  //[0, 10)且可设置步长
    val range = 0..10  //将区间[0, 10]赋值给range
    val range 10 downTo 1  //也可降序10到1
    for (i in range) { //也可直接for (i in 0..10)
        println(i)  //输出结果为 从0打印到10
    }
}
```

##### 类和对象

```
```

