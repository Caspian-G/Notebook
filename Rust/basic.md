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