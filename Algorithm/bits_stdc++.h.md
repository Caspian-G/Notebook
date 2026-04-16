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
//<queue>
queue<T> que;
que.empty();  //判空
que.push();  //入队
que.front();  //返回队首元素
que.back();  //返回最后一个元素
que.pop();  //移除一个元素
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
//<cstring>
memset(void* p, int value, size_t num);  //直接逐字节写入内存 如memset(arr, 0, sizeof(arr));
注：除特定需求，不要将value设置为0或-1之外的数，因为逐字节填充而非按数据类型占内存大小
```

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
//<map>  底层原理红黑树
map<string, int> m1;  //构造一个key为string value为int的空容器
map<string, int> m2(m1);  //拷贝构造
map<string, int> m3(m2.begin(), m2.end());
m1.insert(pair<string, int>("abc", 123));  //pair可用于构造键值对 map对键值对进行查询
m1[...] = ...;  //直接插入或修改
m1.find(key);  //查到返回包含位置的迭代器 没查到返回m1.end()
m1.erase(it \or key);  //删除 迭代器 or key 对应的键和值
m1.erase(first, last);  //左闭右开迭代器
m1.size();
m1.clear();
m1.empty();  //判空
m1.count(key);  //map中键唯一 存在返回1不存在返回0
m1.lower_bound();  //返回迭代器 指向键>=key的第一个元素
m1.upper_bound();  //返回迭代器 指向键>key的第一个元素
//迭代器
m1.begin(); m1.end(); m1.rbegin(); m1.rend();  //使用迭代器需将其视作指针 但它同样可以被加减移位
```

```
//<utility>
//std::pair
pair<string, int> p1("abc", 123);
pair<string, int>("abc", 123);  //临时使用如插入map
p1 = {"aaa", 111};  //赋值 覆盖原值
p1 = make_pair("aaa", 111);  //等价于p1 = pair<string, int>("aaa", 111);
```

```
//<thread>  兼容Linux和win  自动多核并行
//构造函数
thread t1;  //无参构造
t1 = thread(func, 10);  //移动赋值函数 func是函数指针 10是func的参数
注：不允许拷贝构造
thread t1(func, 10);  //函数指针构造 thread使用了模板 你可以声明func的返回值和它的参数为任何类型 但建议声明返回值为void因为thread会直接忽略返回值
//成员函数
t1.joinable();  //返回true/false 判断线程是否可以执行.join() 在.joinable()返回true的情况下使用移动赋值(移动赋值的过程也是先销毁原绑定线程在赋值)或销毁t1绑定线程 程序都会调用std::terminate()终止整个进程
t1.join();  //执行这一段代码的线程将被t1阻塞等待t1执行完后再运行
t1.detach();  //将线程对象t1与其绑定的线程分离 分离后的线程在后台运行
t1.get_id();  //获取t1绑定线程的id
t1.swap(t2);  //交换
//this_thread类
std::this_thread::get_id();  //获取当前线程的id
std::this_thread::sleep_for(std::chrono::seconds(1));  //当前线程暂停一秒 milliseconds microseconds nanoseconds
std::this_thread::sleep_until();  //休眠至一个绝对时间
std::this_thread::yield();  //主动让出当前线程的时间片

thread t1(func, num);  //即便func函数声明时参数是引用方式 在线程执行完也不会影响到外部num 因为主线程的栈内num是以值拷贝的方式传入t1绑定的线程的栈内 如果非要影响主线程的num那么thread t1(func, std::ref(num));

t1.~thread();  //不要写 这样会手动调用析构函数 t1对象在生命周期结束的时候会自动析构 手动析构会导致二次析构最终程序崩溃 同时对joinable的线程对象进行手动析构会导致调用std::terminate()程序崩溃
```

![](D:\Notebook\Assets\Photoes\6e542be6fab440738d4088b8eaddd469.png)

![](D:\Notebook\Assets\Photoes\680413ee43884b2b8a4a96e9f4abe213.png)

**注** printf 和 cout 都带锁 且 cout << "123" << "456"; 123输出完后释放锁 抢到锁456输出完后释放锁 一个printf()里的内容具有原子性 两个<<之间的内容具有原子性

```
//<mutex>  互斥量
//互斥锁的必要性 已知各线程有自己独立的栈 但如全局变量等临界资源需保证线程安全

//mutex互斥锁
mutex mtx;  //声明互斥锁对象
void Func(int n)
{
	while (n--)
	{
		mtx.lock();  // 加锁
		g_val++;  //g_val是全局变量
		mtx.unlock();  // 解锁
	}
}
注：在while内进行加减锁为并行化 while外加减锁为串行化 一般并行化速度大于串行化 但如果对临界资源的操作简单频繁得加减锁反而会销耗过多时间
//recursive_mutex递归互斥锁  用在递归加锁的场景
mutex mtx;
void func(int n){
	if(n == 0) return 0;
	mtx.lock();
	n--; func(n);
	mtx.unlock();
}
int main(){
	int n = 1000;
	thread t1(func, n); thread t2(func, n);
	t1.join(); t2.join();
	return 0;
}  //这是一个普通mutex锁在递归场景下的情况 当第一次进入递归函数线程会拿到锁 之后第二次进入递归再一次申请锁 但此时锁并未释放所以会出现忙等 即死锁 此时使用recursive_mutex mtx;即可 它让线程在自己持有锁时不再申请锁
//timed_mutex  可定时解锁
//recursive_timed_mutex

//**RAII(资源获取即初始化)风格的锁** 
//lock_guard<mutex>
mutex mtx;
void func(int id){
	lock_guard<mutex> lock(mtx);
	cout << "Thread" << id << "is running." << endl;
	if(id == 1) throw runtime_error("Thread 1 encountered an error!");  //如果是正常锁的话 此处执行完会直接跳到main函数的catch而不会执行mtx.unlock(); 但std::lock_guard<mutex>会在作用域结束时自动解锁
}
int main(){
	try{
		thread t1(func, 1); thread t2(func, 2);
		t1.join(); t2.join();
	} catch(const exception& e){
		cerr << "Exception caught: " << e.what() << endl;
	}
	return 0;
}
//unique_lock<mutex>  有lock_guard<>的生命周期结束自动解锁 同时支持手动更灵活
mutex mtx;
void func(int n){
	unique_lock<mutex> lock(mtx);  //加锁
	...
	lock.unlock();  //手动解锁
	...
	lock.lock();  //手动加锁
}
```

```
//<condition_variable>  条件变量(相比于互斥锁 cv用于解决同步问题)
//mutex互斥锁可以阻塞等待 但有的时候我们可以允许不超过某数量的线程同时工作 那么就需要使用信号量
condition_variable = cv;
mutex mtx;
unique_lock<mutex> lock(mtx);
cv.wait(lock);  //使当前线程进入等待状态 直到被其他线程通过notify_one()或notify_all()函数唤醒 该方法调用时自动释放互斥锁并在被唤醒后重新获取互斥锁
cv.wait_for(lock, time);  //返回时情况1被唤醒未超时 情况2超时返回timeout后自动唤醒
cv.wait_until(lock,time);  //同上 但参数是绝对时间
notify_one();  //选择一个等待中的线程唤醒
notify_all();  //唤醒所有等待的线程 使它们从等待状态返回
```

