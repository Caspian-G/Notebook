##### string类成员函数 "".format()

```
x = "{}{{}}{}".format("a", "b")  //x = "a{}b"
x = "{1}{0}{1}".format("a", "b")  //x = "bab"
x = "{name}{age}".format(name = "abc", age = "20")  //x = "abc20"
x = "{0[0]}{0[1]}".format(list)  //x = "ab"  (list = ["a", "b"])
x = "{newList[0]}{newList[1]}".format(newList = list)
x = "{0}{1}".format(*list)
x = "{0[name]}{0[age]}".format(dict)  //x = "abc20" dict = {name: "abc", age = "20"}
x = "{newDict[name]}{newDict[age]}".format(newDict = dict)
x = "{name}{age}".format(**dict)
x = "{Person.name}{Person.age}".format(Person = person)  //x = "abc20" class Person: name = abc, age = 20
//使用__format__()  可自定义对象在格式化时输出
class Test():
	def __format__(self, test):
		print(f"{test}")
		return "1"  //__format__()需要有输出
test = Test()
format(test, "abc")
//填充
print("Hello{0:{1}>{2}}".format("abc","*",15))  //Hello************abc (1填充物 >声明右对齐 2总字符数)
//注
.format()的参数不一定必须是string类型
当{}无法找到匹配的数据时报错
```

##### 表达式插入

```
x = 111  //x可以是任何表达式
print(f"{x}")
x = 1.111111111
print(f"{x:.2f}")  //仅插入保留两位小数点的形式
```

##### random库  提供各种伪随机数

```
import random
seed(n)  //可固定种子数为n
random.randint(a, b)  //[a, b] 随机整数
random.randrange(a)  //[0, a) 随机整数
random.randrange(a, b, step)  //[a, b) 以step为步长的随机整数
random.random()  //[0.0, 1.0) 随机浮点数
random.uniform(a, b)  //[a, b] 随机浮点数
random.choice(seq)  //从序列中随机返回一个元素 seq可以是字符串、列表等等
random.choices(["win", "lose", "draw"], [4, 4, 2], k=10)  //在序列中重复采样k次 可设置各元素权重如[4, 4, 2]
random.shuffle(seq)  //将序列中元素顺序随机打乱再排列
random.sample(seq, k=3)  //从序列中随机选取k个元素以列表方式返回
```

##### lambda 匿名函数 

```
相当于将整个函数赋值给一个变量
a = lambda x, y: x + y  //语法 lambda 参数: 返回值
b = a(x, y)  //函数生效
```

##### 快速生成推导式

```
{键: 值 for 变量 in 可迭代对象 if 条件}  //字典
[变量/表达式 for 变量 in 可迭代对象 if 条件]  // 列表
tuple(变量 for 变量 in 可迭代对象 if 条件)  //元组  (...) 被分配给了生成器
{变量 for 变量 in 可迭代对象 if 条件}  //集合
```

##### zip()

```
把多个可迭代对象(列表、元组等)像拉链一样"咬合"在一起 配对成一个个元组
{n: v for n,v in zip(datas.target_names, np.bincount(datas.target))}
```

