# Add Junit

needed

```
<denpendency> spring-boot-starter-test
```

测试类  ...Test.java:

```
@ExtendWith(SpringExtension.class)  //Junit5: @ExtendWith(); Junit4: @RunWith()
@SpringBootTest(classes = ....java)  //如果测试类和被测试类分别在main和test文件夹下路径相同 则可省略括号
public class ...Test{
	@Autowired
	private ... ...;
	@Test
	public void testPrint(){
		....print();
	}
}
```

