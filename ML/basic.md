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

