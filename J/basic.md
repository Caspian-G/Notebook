#### JDKJREJVM

![](D:\Notebook\Assets\Photoes\3e3560c0ce275eebe70a454c05a2755f.png)

```
javac ....java  //编译  使用官方库无需额外输入参数
javac -cp ".;lib/*" ....java  //使用官方库时 第三方jar放在lib文件夹下
java -cp ".;lib/*" ...  //运行
```

**注** jit指即时编译器 在javac将.java编译成.class字节码后 jvm解释字节码时会监控被执行得比较频繁的代码 某段代码调用次数超阈值时jit介入将这段代码编译为本地机器码

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

#### lambda

java所有的lambda表达式背后都需要一个函数式接口(一个接口有且只有一个抽象方法)

```
//.java
@FunctionalInterface  //函数式接口注解 保证只有一个抽象方法
interface NoParameterNoReturn { 
	void test();
	default void test2() {
		System.out.println("JDK1.8新特性，default默认方法可以有具体的实现");
	}
}
//.java
NoParameterNoReturn noParameterNoReturn = () -> {
    System.out.println("无参数无返回值");  //此处可直接捕获并使用外部变量 但需保证外部变量是final或从未修改过
};
noParameterNoReturn.test();  //执行函数
```

#### 运行原理

javac: .java -> .class    JVM对.class进行解释型执行  (java更像一个混合语言同时存在编译型特征和解释性特征)

#### switch

```
//java11及以前
int day = 1;
String dayName;
switch (day) {  //day可以是多种类型(java17+)但必须有参数
    case 1:
        dayName = "Monday";
        break;
    case 2:
        dayName = "Tuesday";
        break;
    default:
        dayName = "Unknown";
}
//注 除最后一个小段其余必须加上break;
//注 不能直接返回值
```

```
//java12/17+
int day = 1;
String dayName = switch (day) {
    case 1, 7 -> "Weekend"; // 逗号合并
    case 2, 3, 4, 5, 6 -> "Weekday";
    default -> "Invalid day"; // 作为表达式时，default 通常是强制的
};
//注 使用->无需break
//注 switch可直接返回值
//注 case后可接多参数 但依旧只能是常量(java17+可支持模式匹配case后可写类型但使用依然受限)
String result = switch (score) {
    case 100 -> {
        System.out.println("满分！");
        yield "Perfect"; //使用该switch形式在case内有多行逻辑时不能使用return 而必须用yield返回值
    }
    default -> "Good";
};
```

#### 判断对象类型

```
对象 instanceof 类名  //判断一个对象是不是属于某个特定的类或其子类 返回true或false
dog instanceof Dog  //true
dog instanceof Animal  //true
null instanceof 任何类  //false
list instanceof Serializable  //也可检查对象是否实现了某接口
```

#### I/O

```
Scannner scanner = new Scanner(System.in);
String name = scanner.nextLine();  //.nextInt() .nextDouble()

//以下调用命令行需在try{}下
String command = "java --version";
Process process = Runtime.getRuntime().exec(command);  //创建进程
BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(process.getInputStream()));
String reader;
while((reader = bufferedReader.readLine()) != null){  //读取每行
    System.out.println(reader);
}
bufferedReader.close();
process.waitFor();  //等待命令行执行完毕
//或使用ProcessBuilder创建进程
ProcessBuilder processBuilder = new ProcessBuilder("java", "--version");
Process process = processBuilder.start();
```

#### JSON

json本质上是特定格式的字符串
