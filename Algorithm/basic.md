##### 整型

```
short  //2B
int  //4B
long  //4B or 8B
long long  //8B
```

##### 换行符

"\n" 插入换行符 性能更高

endl 插入换行符+强制刷新输出流缓冲区 性能较差 **使用场景**(调试程序)当你需要通过cout定位崩溃位置时用endl能确保程序崩溃前的信息已经显示在屏幕上而不是死在缓冲区里

刷新缓冲区: 为了提高效率C++ 的cout并不总是立刻把数据送到屏幕上而是先存放在内存的一块**缓冲区（Buffer）**里等缓冲区满了或者程序结束了再统一发送

##### <> ""

```
主要区别在于头文件包含时路径搜索顺序不同
<>  优先编译器设置的标准库路径  注 <bits/stdc++.h>是GCC编译器扩展 非c++标准库
""  优先源文件所在目录
```

##### 标准库 头文件 命名空间 std

```
标准库 = 头文件(.h 声明 类似接口) + 库文件(.lib/.a/.so 实现)
命名空间 的产生是为避免引入各种库导致的各种方法名和变量名等的冲突
std::cin >> a;  //std是指标准库的命名空间
using namespace std;  //全声明命名空间
using std::cin;  //部分声明
```

##### 循环语法糖

```c++
for(auto x : vec){  //c++11  默认为拷贝模式 引用模式需指定(auto &x : vec) 
	...;  //编译时会展开为使用迭代器的普通循环 因此不能向容器调价或删减元素如vec.push_back();
}
```

```java
for(var x : list){  //java5  var-java10 默认为引用模式
    ...;  //同样不能增删元素
}
```

##### auto

此类型声明可通过等号右侧的数据类型推导出左侧类型

```
auto a = "abc";  //a的类型确定后不可再修改  推导能力强可用于返回值、Lambda、模板等
```

java类似var

```
var a = "abc";  //同样一次确定类型后续无法更改  推导能力较弱仅限于局部变量
```

python

```
a = "abc";  //可随意更改数据类型
```

##### 泛型

```
//函数泛型
template <typename T, typename ...>
T func(T a, T b){}  //相较于直接使用auto func(auto a, auto b){}(auto函数返回类型推导c++14, auto函数参数类型推导c++20) 使用泛型可统一参数和函数类型
//类泛型 定义一个通用的栈
template <typename T>
class Stack {
private:
    vector<T> data;
public:
    void push(T value) { data.push_back(value); }
    T pop() {
        T val = data.back();
        data.pop_back();
        return val;
    }
};
```

##### ASCII

```
char a = '0';  //ASCII 48
char a = 'A';  //ASCII 65
char a = 'a';  //ASCII 97
char a = '\0';  //ASCII 0
char a = '\n';  //ASCII 10
char a = '\r';  //回车(光标移到本行行首) ASCII 13
char a = ' ';  //ASCII 32
```

##### \<typeinfo\>

```
typeid(a).name()  //可输出a的数据类型(较长) 用处不大
```

