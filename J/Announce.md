# Announce

#### 修饰符

|           | 同一类 | 同一包 | 子类 | everywhere |
| :-------: | :----: | :----: | :--: | :--------: |
|  public   |   T    |   T    |  T   |     T      |
| protected |   T    |   T    |  T   |     F      |
|   none    |   T    |   T    |  F   |     F      |
|  private  |   T    |   F    |  F   |     F      |

#### super

``` 
public class DemoFather{
	void sayhello(){
		"hello1".sout;
	}
}
public class DemoSon extends DemoFather{
	@Override
	void sayHello(){
		super.sayHello();  //super相当于父类的对象
		"hello2".sout;
	}
}
```

#### 项目name修改

项目结构中项目名修改+本地磁盘下项目名修改

#### 经典https测试视频

```
"http://vjs.zencdn.net/v/oceans.mp4"
```

#### 使用正则表达式分割字符

```
String[] newPath = path.split("/");
```

#### 字符串常用方法

```
.endsWith()  //是否以某字符串结尾
.startsWith()
.contains()  //是否包含
.isEmpty()
.toLowerCase()/.toUpperCase()  //转小大写
.split()  //按某切开
.substring(int beginIndex, int endIndex)  //截取中间一段
.lastIndexOf(String s)  //查最后一个某字符位置

普通拼接(+)  //在 onCreate 里偶尔写一两次没关系。
StringBuilder  //如果在循环里拼接字符串 必须用
StringBuilder sb = new StringBuilder();
for (String s : list) {
    sb.append(s).append("/");
}
String result = sb.toString();
原因 Java的String 是“不可变”的 用+拼接1000次会创建1000个临时对象非常卡顿 StringBuilder就像一个可伸缩的容器 效率极高

==  //比较内存地址
.equals()  //比较值
```



