# #include <bits/stdc++.h>

**注** 提速代码 ios::sync_with_stdio(0);  //取消 C++ 标准流与 C 标准流的同步(此方法下cin会比scanf更快) 执行后不可在同一程序中混用cout(cin)和printf(scanf)

​     		cin.tie(0);  //解除cin和cout的绑定 默认情况cin和cout是绑定的 每当你执行cin读取数据前 系统都会自动执行一次flush刷新cout的缓冲区以确保你在输入前能看到之前的提示文字 批量输入输出时无需此绑定

**注** ios::sync_with_stdio(0); 会导致每次cout也不再刷新缓冲区 需在最后一次cout出endl强制刷新

```
unsigned a = 1;  //等价于unsigned int a = 1;且a不可用负数赋值
unsigned short a = 1;
unsigned long a = 1;
unsigned long long a = 1;
size_t a = 1;  //等价于size_t long a = 1;64位系统上
```

```
//<algorithm>
max()  max({})
abs()  //绝对值 只用于整数变量 <cmath>的fbs()可用于浮点
sort(a, a+5)  //默认从小到大排
find(a.begin(), a.end(), ...)  //查找 返回迭代器
swap(x, y)  //交换x和y
reverse(a, a+5)  //翻转数组a索引为0到4的值(a指向索引0)
fill(a, a+3, 1)  //在某区间内填充某值 1 1 1
count(a, a+3, 2)  //查找某数在区间出现次数
__gcd(x, y)  //求两数的最大公因数 最小公倍数=x*y/最大公因数
next_permutation(a, a+3)  //给定数组(必须数组)容器全排列(如果存在下一个"更大"的排列方式则返回true如果初始排列就是最大如{3,2,1} 则返回false)  注意排列组合的长度
    int a[3]={1,2,3};
    do{
        cout << a[0] <<a[1] << a[2] << endl;
    }while(next_permutation(a,a+3));
```

```
//<vector>
vector<ElemType> a(size, default);
a = {1,2,3, ...}可这样初始化覆盖默认0
a.at(pos);  //访问索引为pos的位置 但会检查越界 越界抛异常
a.front();  //访问第一个元素
a.back();  //访问最后一个元素
a.size();  //当前实际存储元素个数
a.capacity();  //不分配新内存情况下 最多容纳元素个数
vector 为动态数组 当push_back至超出capacity值时会重新找一片更大的(*1.5 or *2)新内存空间将原数据复制至新内存 然后释放旧内存空间
a.reserve();  //手动修改capacity的值 避免在使用push_back()时导致vector反复触发内存重分配
a.resize(n, 100);  //手动修改size值为n多余值填充100 如果无第二个参数填充时默认值为0
a.empty();  //判空
a.push_back();  //从现有数据尾部插入包括0
a.insert(a.begin(), 100);  //在索引为0的位置插入100
a.pop_back();  //删除最后一个元素
a.erase(a.begin());  //删除索引为0的元素
a.clear();
注 vector无<<方法 不可直接输出
//迭代器  增删时迭代器会失效 的迭代器方法返回值是一个对象而非值或指针 但该对象的类定义了类似指针和值的操作方法所以可按指针或值的方式操作它
a.begin();  //返回第一个元素位置
a.end();  //返回最后一个元素的后一个位置
a.rbegin();  //返回最后一个元素的位置
a.rend();  //返回第一个元素的前一个位置
```

```
//<string>  不同于c标准库的<string.h>(包含一个string类) 
//构造方法
string s1("abc");
string s2("abc", 1, 2);  //bc
string s3("abc", 2);  //ab
string s4(5, 'x');  //xxxxx
string s5(5, 42);  //*****(* == (ASCII)42)
string s6(s1.begin(), s1.begin()+2);  //ab 相当于选取部分赋值
//成员方法
s.size();  //当前实际存储元素个数
s.length();  //获取长度 不同于java的s.length
s.resize();  //重新分配空间 变小则截去 变大加入空字符
s.max_size();  //返回当前系统环境下保证不崩溃的理论字符串最长长度
s.capacity();  //不分配新内存情况下 最多容纳字符个数
s.reserve();  //手动修改capacity的值 避免在使用push_back()时导致反复触发内存重分配
s.empty();  //判空
s.operator[](n);  //返回索引为n的字符 类似对数组的索引操作 这样写等价于s[n];
s.c_str();  //获得C语言风格的const char*字符串
s.data(); //获取字符串内容
s.operator=("...");  //赋值 s = ...的底层函数
s.assign("...");  //相当于 s = ...
s.operator+("...");
s.operator+=("...");  //s += ...的底层实现函数
s.operator==();
s.operator!=();
s.operator<();
s.operator<<();
s.operator>>();
s1.swap(s2);  //交换两个字符串
s.insert(pos, "...");  //在pos位置插入字符串
s.append();  //加入字符串
s.push_back();  //加入字符串
s.pop_back();  //删除最后一个元素
s.erase(a.begin());  //删除索引为0的元素
s.clear();  //清空
s.replace(1, 2, "abc");  //从索引1开始替换2个字符为"abc"
s.replace(1, 2, 3, "a");  //从索引1开始替换2个字符为"aaa"
s.copy(buffer, 3, 0);  //从s索引0的位置复制三个字符到buffer(必须为char字符数组)数组同时返回复制的字符个数3 buffer缓冲区的大小需手动管理
s.find();  //字符串查找 返回第一个匹配的索引
s.rfind();  //反向查找
s.find_first_of("abc");  //a、b、c只要找到一个就算找到 返回第一个位置
s.find_first_not_of();  //查找不包含的第一个字符位置
s.find_last_of("abc");  //a、b、c只要找到一个就算找到 返回最后一个位置
s.find_last_not_of();  //查找不包含的最后一个字符位置
s.substr(x, y);  //从x分割到y   s.substr(x);  //从x分割到结束
s1.compare(s2);  //s1和s2值相等时返回0 比较的是字典序
//迭代器  增删时迭代器会失效 迭代器方法返回值是一个对象而非值或指针 但该对象的类定义了类似指针和值的操作方法所以可按指针或值的方式操作它
s.begin();  //返回第一个字符位置
s.end();  //返回最后一个字符的后一个位置
s.rbegin();  //返回最后一个字符的位置
s.rend();  //返回第一个字符的前一个位置
//方法
int i = stoi(s);  //字符串转整型
string s = to_string(i);  //整型转字符串
```
**注** c++ 中char时可以直接用==比较的 因为它存档是ASCII

```
//<iostream>
char buffer[100];
cin.getline(buffer, 100);  //从输入流读取一行到c语言风格的buffer字符串中 设置最多读取99个字符
cin.getline(buffer, 100, ',');  //读取直到遇到','
string line;
getline(cin, line);  //从标准输入读取一行(全局函数写法)
getline(cin, line, ',');
```

```
//<map>

```

