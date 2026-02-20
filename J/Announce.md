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

