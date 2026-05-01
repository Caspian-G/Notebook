#### 变量与可变性

rust变量默认不可变  可不显式类型

```
let a = 5;
let mut b = 5;  //显式声明变量可变
```

#### 常量

声明时需显式类型

类似c++的constexpr(编译期常量)  但相比于c++ rust不会给常量分配内存空间编译器会将其值直接内联至使用处

```
const a: i32 = 5;
```

#### 数据类型

```
let a: i32 = -5;  //有符号32位整数
let b: u64 = 100;  //无符号64位整数
let c = 0xff;  //十六进制
let d = 0o77;  //八进制
let e: f32 = 3.14;  //单精度
let f = 2.71828  //默认f64
let flag = true;  //布尔 占1字节
let g = 'A';  //字符型(Unicode编码 4字节 支持emoji)
```

**注** c++中char占1字节

```
let person: (&str, i32, bool) = ("张三", 25, true);  //元组 固定长度 可包含不同类型的值
println!("元组：姓名={}, 年龄={}, 是否学生={}", person.0, person.1, person.2);
let (name, age, is_student) = person;  //元组结构 用于提取元素
println!("解构后：{}，{}岁，学生：{}", name, age, is_student);

let scores: [i32; 5] = [90, 85, 95, 88, 92];  //数组[类型, 长度] 固定长度 所有元素类型相同(栈上分配)
println!("数组长度：{}", scores.len());
println!("第一个元素：{}", scores[0]);

let mut fruits = vec!["苹果", "香蕉", "橙子"];  //动态数组Vec 堆上分配 长度可变 标准库提供需用vec!宏创建
fruits.push("葡萄");   //添加元素
fruits.remove(1);     //删除索引1的元素
println!("动态数组：{:?}", fruits);  //{:?}用于打印复合类型
```

**注** vec! 是一个宏 而非普通函数

**注** 因为使用Unicode变长编码 所以无法通过索引访问str或String类型 而Vec支持使用索引访问

#### 判断 循环

```
if a = 0 {
	...
} else if a > 0 {
	...
} else {
	...
}
//类三元运算符
let flag = if a >= 0 { true } else { false };

//loop循环需要break退出 否则无限循环
let res = loop{
	if a > 0 {
		break a;
	}
};

//while循环
while a > 0 {
	...
}

//for
for it in list {
	...
}
//1..5表示1~4 1..=5表示1~5
for it in 1..5 {
	...
}

//match模式匹配  要求必须覆盖所有可能情况(编译时检查)
match a {
	1 => println!("1");
	2 => println!("2");
	3 => println!("3");
	_ => println!("其它所有数");  //_必有
}
match b {
	1..=10 => ...
	11..=20 => ...
	21..=30 => ...
	_ => ...
}
```

#### 特性 所有权与借用 (内存安全保障)

编译时检查

1 每个值都有一个所有者

2 同一时间 一个值只能有一个所有者

3 当所有者离开作用域(如函数结束或代码块结束等) 值会被自动销毁(内存释放)

##### 转移所有权

```
let s1 = String::from("abc");
let s2 = s1;  //s1失去所有权 无法使用 如果声明的是let mut s1 = ... 则可再次初始化s1
```

###### 栈上存储  实现Copy可按位复制 

结构体 元组 数组 枚举 ...

###### 堆上存储  移动所有权

String Vec\<T> Box\<T> Rc\<T> Arc\<T> ...

##### 借用

临时使用某值而不转移所有权  本质上类似c++的&引用

```
//不可变借用 同一时间可有多个不可变借用
let s = String::from("abc");
let r1 = &s;
let r2 = &s;
println!("{} {}", r1, r2);  //打印abc abc 而不是两串地址是因为println!宏会自动解引用

//可变借用 同一时间只能有一个可变借用 且不可有不可变借用
let mut s_mut = String::from("abc");
let r3 = &mut s_mut;  //r3是一个指针
r3.push_str("def");
let _ = r3;  //手动释放 可选 作用域结束或再次使用s_mut时会自动释放
```

**注** _ 为无名占用符 一个值被绑定到_后被立即丢弃

##### 切片

一种特殊的借用 用于访问序列的一部分(如字符串、数组)的一部分 无所有权

```
let s = String::from("hello world");
let slice1 = &s[0..5];  //从索引0到4
let slice2 = &s[6..];  //从索引6到结尾
```

**注** 切片借用不可变

```
let mut s = String::from("111222");
let slice1 = &mut s[0..5];  //即便使用&mut 返回的是&mut str而不是&mut String 所以依旧是不可变 无法使用push_str进行操作
slice1.push_str("333");  //这里会报错
```

**注** &mut str类型只能原地修改值 不支持增加或删除字符

#### 自定义类型

##### 结构体

