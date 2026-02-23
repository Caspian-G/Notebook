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

#### viewmodel.ViewModel.java  编写逻辑业务

```
//这里extends是为了在Activity中对ViewModel实例使用new ViewModelProvider(this).get(...ViewModel.class);
public class ...ViewModel extends ViewModel{
	
}
```

**注** Activity 中 ViewModel viewModel = new ViewModelProvider(this).get(ViewModel.class);  //this指Activity实例 这样写是为了保证在手机从横屏转竖屏Activity销毁时ViewModel会被继续保留而不是重新创建 这样可防止内存泄漏且优化体验

#### model.repository.Repository.java  编写数据逻辑

```
public class ...Repository{
	
}
```

#### res资源文件夹

**注** R: res文件夹

res\drawable 图片等

res\raw           视频等

res\layout       xml布局文件

#### LiveData

设计思想：观察者模式

添加依赖

```
dependencies {
    implementation("androidx.lifecycle:lifecycle-viewmodel:2.6.1")
    implementation("androidx.lifecycle:lifecycle-livedata:2.6.1")
}
```

使用

```
类1 以ViewModel为例
private final MutableLiveData<WlPlayer> wlPlayerLiveData = new MutableLiveData<>();  //创建一个容器
//暴露一个不可变的LiveData给Activity看（为了安全，不让 Activity 随便改里面的值）
public LiveData<WlPlayer> getWlPlayerLiveData() {
	return wlPlayerLiveData;
}

类2 以Activity为例
vlViewModel.getWlPlayerLiveData().observe(this, new Observer<WlPlayer>() {
    @Override
    public void onChanged(WlPlayer player) {
        if (player != null) {
            wlSurfaceView.setWlPlayer(player);
        }
    }
});
//当你执行 viewModel.getWlPlayerLiveData().observe(...) 时，Android 系统内部做了一件事：它把你的 Activity 引用和一个 onChanged 函数存进了一个 List（列表） 里，平时： 代码处于静默状态，完全不消耗 CPU 循环，不会像 while(true) 那样空转，触发时： 只要你在 ViewModel 中执行了 wlPlayerLiveData.setValue(player)，LiveData 内部就会遍历那个 List，挨个调用里面存着的 onChanged 方法
```

#### Room本地数据库

设计思想：单例模式
