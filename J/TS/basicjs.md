#### 引用

```
<script>document.write("hw");</script>
//or
<script src="main.js"></script>
```

####  输出

```
//页面输出
document.write("...");
//控制台输出
console.log("...");
console.info("...");
console.warn("...");
console.error("...");
//弹出窗口输出
alert("...");
```

#### 注释

```
//单行注释
/** 
 *多行注释 
 */
```

#### 变量

```
//区别见basicts.md
var a = 1;
let a = 1;
//常量 const b = 1;
```

#### 数据类型

String
Number  表示整型或浮点(范围正负最大值1.7976931348623157e+308 最小值5e-324) NaN非法数字
Boolean
Undefined  是值也是类型(作为未初始化变量的类型、无返回值的函数类型等) 仅有undefined值
Null  是值也是类型 仅有null值
Object  除上述都为object型

```
console.log(typeof 1);  //number
a = a.toString();  //转换undefined和null类型会报错
a = String(a);  //转换undefined返回"undefined" null转换"null"
a = Number(a);  //null -> 0 undefined -> NaN
a = Boolean(a);  //null undefined 0 NaN "" -> false
```

####  比较运算符

== 相等运算 类型不同时会先将类型统一再比较

!= 不相等运算 类型不同时会先将类型统一再比较 

=== 全等运算 类型不同直接false

!== 不全等运算 类型不同直接true

#### 条件运算符

类似c++三元运算符

####  逗号运算符

```
var num1 = 1, num2 = 2, num3 = 3;
```

#### 代码块

{}  let const 块级作用域

​     var  函数作用域

#### 判断

if...else if...else

switch...case (类c++)

#### 循环

for  while  do...while

引入 跳转控制  用于跳出多层循环

```
outer: for (var i = 0; i < 10; i++) {
    for (var j = 0; j < 10; j++) {
        if (j == 5) {
            break outer;
        }
        console.log(j);
    }
}
```

#### 对象

