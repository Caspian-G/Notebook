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

##### 未知读入数据长度时

```
while(cin >> a){if(a == EOF) break; ...}  //通常OJ会在数据输入完毕后输入EOF 需显示判断是否读到EOF并退出 否则死循环
while(scanf() != EOF){...}  //EOF为宏常量默认-1
```

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

##### #define  typedef;

```
#define ... ...  //宏定义 编译前文本替换 new前old后
typedef ... ...;  //给已有数据类型创建别名 old前new后
```

##### struct

```
//c语言
struct name{};  //标准结构体  创建变量struct name a;
struct name{}s;  //顺带创建变量
typedef struct name{}newname;  //创建别名  创建变量newname a;
typedef struct{}name;  //本无名直接创建别名
//c++ 无需typedef也可直接使用 结构体名 变量名 的形式创建单个变量

注：c++中struct和class唯一的区别在于struct的成员默认访问权限是public class是private
```

**注** 尽量让struct被声明到栈上而不是堆

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

##### start main

```
声明int返回值类型是因为需要给os返回是否成功执行 现代编译器的标准
```

##### switch

```
switch(x){  //x必须时int char bool或枚举
	case ...:  //case后面接的必须是常数
		...
		break;  //记得break;
	case ...:
		...
		break;
	default:
		...
		break;  //只要default在最后加不加break一样
}
```

##### const

```
const int a = 1;  //const修饰时就必须赋初值  不同于java的final可之后赋初值
```

##### 指针

野指针: 指向不确定或无效内存地址的指针

**注** 野指针或空指针无法使用p->next

指针初始化可指向非常多的位置  栈内存 堆内存 已初始化和未初始化的全局或静态变量区 代码或常量区 直接指向设备物理地址

```
* //1 声明指针 2 获取指针变量的内容
& //1 引用(cpp) 2 获取变量的地址
char* p = "abc";  //指针存放的永远是首地址
*p  //当指针指向的地址具有序列性时如string，*取值时只会取首值
const char* p;  //不能修改指针指向的内容
char* const p;  //不能修改指针变量的指向

//c++ 申请/释放内存空间
Stu* stuTemp = new Stu();
delete stuTemp;
delete[] list;  //释放整个链表空间
//指针不申请空间无法存连续值

const char* p; == char const* p;  //可修改p指向地址 但不可修改p指向内容
char* const p;  //可修改p指向内容 但不可修改p指向地址

//将指针作为参数传递时 依旧会是传递副本 除非加上&
int* p;
func(p);
void func(int* p){}  //该函数中可以对指针指向地址的内容进行修改 但修改形参p的指向时 实参是不会跟着修改的 除非引用即int* &p
```

**注** new Node(); Node是POD类型时初始化0，类类型时调用构造函数 ; new Node; Node是POD类型不初始化，类类型调用构造函数

##### 指针函数vs函数指针

```
//指针函数 返回值为指针
void* func(){return nullptr;}
//函数指针 指向函数在代码段中的地址
int (*p) (int, int);  //p可指向一个返回int 参数为两个int的函数
p = add;
cout << p(1, 2) << endl;  //3
```

##### VLA变长数组

```
int a[n];  //形如此 n为变量 尽量用vector创建VLA
```

##### 预处理

```
//常见
#include #define
#undef  //取消宏定义
#if  #ifdef  #ifndef  #else  #elif  #endif  //条件编译 根据条件选择保留或删除代码块
#error  #warning  //在预处理阶段残生错误或警告
#line  //修改编译器报错的行号
#pragma  //向编译器传递特定指令 如优化、警告等
#  //空指令 什么都不做 可用于排版
```

##### 类

```
struct声明类 默认访问权限为public
struct MyClass{
	void operator()(..., ...){}  //operator是运算符重载方法此处重载了()运算符
};
```

##### 重载vs重写

```
//重载是同名函数 参数列表不同
//重写是重新定义父类的方法 需保证函数名、参数、返回类型相同
```

##### main函数参数

```
//argc 命令行参数个数  argv[]数组每一个元素存一个字符串
int main(int argc, char* argv[]){  //声明为char*更具可移植性
	cout << "参数个数: " << argc << endl;
    for (int i = 0; i < argc; i++) {
        cout << "argv[" << i << "] = " << argv[i] << endl;  //此处argv[i]直接打印字符串而不是地址 是因为cout的<<运算符重载了很多参数类型 这里对于char*会调用直接输出字符串的运算符函数
    }
    return 0;
}
```

##### cout 控制输出位数

```
cout << [fixed << ] setprecision(n) << (double)num << endl;  //加fixed控制小数位数 不加控制有效位数
```

##### sqrt vs pow

```
sqrt()  //专门求算术平方根  默认返回double类型
pow()  //更灵活
```

##### INF

```
#define INF 0x3f3f3f3f  //int(or 1e9)
#define INF 4e18  //long long
#define INF 1e300  //double
```

##### GNU and GCC

GNU项目为Linux提供编译器、链接器、调试器、编辑器、Shell 以及各类命令行工具在内的全套软件

GCC是GNU中的编译器工具包  GCC本质上是通过调用cpp(预处理)、cc1(编译)、as(汇编)、ld(链接)这些子工具实现

##### Runtime

广义：程序在编译、链接后的运行时期  该时期的RuntimeError常因内存泄漏、除0异常等引起

狭义：运行所需库、运行所需环境(如jvm等)

##### stdio.h vs cstdio

stdio.h  全局命名空间

cstdio  std命名空间  c++推荐

##### \<string> vs \<cstring>

\<string>  c++字符串类

\<cstring>  c语言风格的<string.h>移植库唯一区别在于命名空间

##### int[]\(或long[]) 和 unordered_map的选择

条件：键为非负整数

```
if 已知数组可能的最大占用 选静态数组
else 选散列表
```

