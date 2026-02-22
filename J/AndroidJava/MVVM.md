# MVVM架构

#### view.MainActivity.java

```
public class MainActivity extends AppCompatActivity {
	@Override
	protected void onCreate(Bundle savedInstanceState){
		super.onCreate(savedInstanceState);  //调用父类onCreate方法
		EdgeToEdge.enable(this);  //实现全面屏
		setContentView(R.layout.activity_main);  //加载xml布局文件
		//全面屏适配
		ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main),(v,insets)->{
			Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
			v.setPadding(systemBars.left,systemBars.top,systemBars.right,systemBars.bottom);
			return insets;
		});
	}
}
```

#### model.entities.Entity.java

```
public class Entity{
	private Object ...;
	...getter&setter;
	...toString();
}
```

