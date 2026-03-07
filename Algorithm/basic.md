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

##### 保留小数策略

```
printf("%.2f", 3.1467);  //3.15 格式化输出四舍五入
int a = (int)3.14;  //强制类型转换 强行截断
```

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

##### lambda  (c++11及以上)

```
//语法
[capture](parameters) mutable -> return_type {  加上mutable后捕获的变量的拷贝值就可修改 但依旧不会修改原变量值
    // 函数体 
}

//捕获方式
捕获方式		语法		生命周期		修改权限		示例
值捕获			 [x]	  创建时拷贝		需要mutable	int x=5; [x]{...};
引用捕获	    [&x]	 依赖原变量		直接修改原值	 [&x]{x=10;};
隐式值捕获		[=]		创建时全拷贝	  需要mutable	  [=]{return a+b;};
隐式引用捕获	   [&]		依赖原变量	  直接修改原值	[&]{modify(a);};
捕获当前类的this指针[this]	依赖外部对象	不需要mutable	[this]{return m_var;}
混合捕获		[=, &x]	   组合使用		 按各自规则	   [=,&err]{...};
auto a = [&b] {  //无参引用捕获事例 相当于将函数赋值给了a 此处若无法使用auto则当函数不return时使用std::function<void()> []内可以写捕获表达式(c++14)
    b += 5;
}; a();  //此处调用函数体b值+5
//参数传递方式
auto a = [](int x, int y){return x + y;};  //显式参数类型
auto a = [](auto x)->float{if(x == 0)return 0;else return static_cast<float>(x);};  //当return多类型时需显式定义返回类型

//高级特性
//立即执行
const auto result = [](int base) {
    int sum = 0;
    for(int i = 1; i <= base; ++i) 
    {
        sum += i;
    }
    return sum;
}(100);  //此处加上(100)后会立即执行函数并返回结果 而不是把函数本身赋值给result变量
std::cout << result;  //输出5050
//递归lambda
auto factorial = [](auto self, int n) -> int {
    return n <= 1 ? 1 : n * self(self, n-1);
};
std::cout << factorial(factorial, 5);  //输出120
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

##### 强制类型转换(显式)

```
double x = (double)y;  //c风格
double x = static_cast<double>(y);  //编译时转换 适用非多态转换
double x = dynamic_cast<double>(y);  //运行时转换 多态类之间的安全转型
double x = const_cast<double>(y);  //编译时转换 唯一可对const变量操作 但会去除const 有后续只读变量被修改的风险
double x = reinterpret_cast<double>(y);  //编译时转换 硬件地址操作、位模式重新定义
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

