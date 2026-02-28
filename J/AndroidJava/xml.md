#### AndroidManifest.xml

//app户口本

```
<?xml version="1.0" encoding="utf-8"?>
//声明 命名空间 此处标有android:或tools:开头的属性都要遵循Android官方规范
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    //请求使用权限
    <uses-permission android:name="android.permission.INTERNET" />  //网络权限
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>  //读取本地存储权限
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>  //写入本地存储权限
    <application
        //备份
        android:allowBackup="true"  //允许系统备份应用数据
        android:dataExtractionRules="@xml/data_extraction_rules"  //引用一个 XML 文件，详细规定哪些数据可以被提取备份
        android:fullBackupContent="@xml/backup_rules"  //详细规定哪些数据不可以被备份
        //应用外观与辅助
        android:label="@string/app_name"  //定义应用显示名称
        android:supportsRtl="true"  //允许“从右到左”的布局
        android:theme="@style/Theme.VL">  //整个应用的皮肤
        android:icon="@drawable/ic_launcher"  //设置软件外图标
        //定义用户能看到的界面
        <activity
            android:name=".MainActivity"
            android:screenOrientation="portrait"  //固定为竖屏 landscape-横屏
            android:configChanges="orientation|screenSize|keyboardHidden"  //不重启但允许旋转
            android:exported="true">  //声明系统有权启动这个界面(如果需将该页作为启动页需true)
            //启动意图
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />  //标记应用主入口

                <category android:name="android.intent.category.LAUNCHER" />  //将此应用的图标放在系统桌面上
            </intent-filter>
        </activity>
    </application>

</manifest>
```

###### 屏幕方向固定 法二 java代码

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    // 强制设置为竖屏/横屏
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
    // 恢复为根据传感器自动旋转（前提是用户的系统开启了“自动旋转”）
	setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_SENSOR);
}
```

#### activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
//constraintLayout是一个大幕布
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".view.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"  //文本
        //上下左右都是"parent"对齐所以会居中
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

	//这里是一个wlmedia播放器的案例 使用Surface机制
    <com.ywl5320.wlmedia.widget.WlSurfaceView
        android:id="@+id/wlsurfaceview"  //写上id给javaorkotlin识别
        android:layout_width="match_parent"  //铺满整个width
        android:layout_height="match_parent" />  //铺满整个height
</androidx.constraintlayout.widget.ConstraintLayout>
```

**注** 后面的代码会覆盖前面的 如果后面的代码声明铺满整个constraintlayout则会覆盖前面的text文本

**注** app:layout_constraint..._to...Of只能在父为constraintlayout的情况下才能生效 如果父非constraintlayout那么可以使用<View></View>(android:layout_weight="1" //降低权重)将其它控件挤到其它位置以实现居右或居下的效果

wrap_content 包裹内容

match_parent 占满父母

##### Button

```
<Button
    android:layout_width="match_parent"
    android:layout_height="35dp"
    android:text="test"
    app:cornerRadius="0dp"  //去除角弧度
    app:backgroundTint="@color/grey">
</Button>
```

##### 顶部标题栏+次顶部下拉框+下方滚动窗口 模式      含activity中对于控件的逻辑代码

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".view.MainActivity">
    //顶部标题栏
    <LinearLayout
        android:layout_width="match_parent"
 	    android:layout_height="match_parent"
        android:orientation="vertical">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="30dp"
            android:padding="6dp">
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:textColor="@color/grey"
                android:text="VL">
            </TextView>
            <View  //用于填充中间空间
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_weight="1">
            </View>
            <ImageButton
                android:id="@+id/setting"
                android:src="@drawable/ic_setting"
                android:background="@color/white"
                android:layout_width="20dp"
                android:layout_height="match_parent">
            </ImageButton>
        </LinearLayout>
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="30dp"
            android:background="@color/grey">
            <ImageButton
                android:id="@+id/search"
                android:layout_width="27dp"
                android:layout_height="match_parent"
                android:background="@color/grey"
                android:src="@drawable/ic_search"
                android:paddingLeft="3dp">
            </ImageButton>
            <EditText
                android:id="@+id/search_input"
                android:layout_width="match_parent"
                android:textSize="14sp"
                android:imeOptions="actionSearch"  //将回车定义为“搜索”
                android:layout_height="match_parent"
                android:layout_weight="1"
                android:includeFontPadding="false"
                android:background="@null"  //去除背景避免文本被遮挡(父的height设置有些小) 不想去掉下划线强行修改padding也可以
                android:gravity="center_vertical"  //垂直居中 改center后值还可水平居中
                android:visibility="gone"
                android:inputType="text">
            </EditText>
            <View
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_weight="2">
            </View>
            <ImageButton
                android:id="@+id/select_list"
                android:layout_width="27dp"
                android:layout_height="match_parent"
                android:src="@drawable/ic_select_list"
                android:background="@color/grey"
                android:paddingRight="3dp">
            </ImageButton>
        </LinearLayout>
        <ScrollView
            android:layout_width="match_parent"
            android:layout_height="match_parent">
            ...
        </ScrollView>
    </LinearLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="20dp"
        app:layout_constraintBottom_toBottomOf="parent">
        <TextView  //底部用于显示路径
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:text="path"
            android:paddingLeft="4dp">
        </TextView>
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

**注** IDEA可生成背景透明的向量资源 在res/drawable下新建Vector Asset即可

**注** 使用本地图片android:src="@drawable/..." 使用自带资源库android:src="@android:drawable/..."

```
import androidx.appcompat.widget.PopupMenu;

@Override
protected void onCreate(Bundle savedInstanceState){
	...
	//以setting为例
	final ImageButton selectList = findViewById(R.id.select_list);
	selectList.setOnClickListener(new View.OnClickListener(){  //设置点击监听器
		@Override
		public void onClick(View v){  //View是button、ImageButton、TextView等类的基类
			//下方如果直接使用this而不是MainActivity.this编译器会以为this是指OnClickListener对象
			PopupMenu popup = new PopupMenu(MainActivity.this, v);
			popup.getMenu().add("获取视频资源目录");
			popup.getMenu().add("是否允许应用界面翻转 Now:");
            popup.setOnMenuItemClickListener(item -> {  //设置点击每个选项的功能
                String title = item.getTitle().toString();
                if (title.equals("...")) {
                    ...
                }
                return true;
            });
            popup.show();  //显示在屏幕上
		}
	});
}
```

##### 使用EditText隐藏文本框时可采用全局点击逻辑

```
//判断点击坐标是否在event范围内
private boolean isViewInEvent(View v, MotionEvent event) {
    if( v!=null && v.getClass() == EditText.class ){
        int[] loc = new int[2];
        v.getLocationOnScreen(loc);
        float x = event.getRawX();
        float y = event.getRawY();
        if( x > loc[0] && x < loc[0] + v.getWidth() && y > loc[1] && y < loc[1] + v.getHeight() ){
            return true;
        }
    }
    return false;
}
//全局点击逻辑
@Override
public boolean dispatchTouchEvent(MotionEvent event) {
    if (event.getAction() == MotionEvent.ACTION_DOWN) {
        View v = getCurrentFocus();
        if( !isViewInEvent(v, event) ){
            searchInput.setVisibility(View.GONE);
            InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
            if (imm != null) {
                imm.hideSoftInputFromWindow(searchInput.getWindowToken(), 0);  //键盘退出
            }
            searchInput.clearFocus();  //清除聚焦
        }
    }
    return super.dispatchTouchEvent(event);
}
```

##### 使用EditText对象的requestFocus()方法时 无法弹出键盘 可采用延迟聚焦请求的方法

```
private void settingClicker(View v) {
    // 1. 先让输入框可见
    searchInput.setVisibility(View.VISIBLE);
    // 2. 加入延迟处理
    searchInput.postDelayed(new Runnable() {
        @Override
        public void run() {
            // 确保 View 已经拿到了焦点
            searchInput.requestFocus();
            // 获取系统键盘管理器
            InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
            if (imm != null) {
                // 此时 View 已经布局完成，键盘可以顺利弹出
                imm.showSoftInput(searchInput, InputMethodManager.SHOW_IMPLICIT);
            }
        }
    }, 100); // 100 毫秒是体感延迟和成功率的平衡点
}
```

##### 实现ImageButton点击时简单旋转动画逻辑 setting为例

```
ObjectAnimator animator = ObjectAnimator.ofFloat(v, "rotation", 0f, 180f);
animator.setDuration(400);
animator.start();
```

##### 点击setting弹出左侧抽屉栏效果

将DrawerLayout包裹希望被覆盖的View

**注** DrawerLayout中同一侧只能存在一个View

```
<androidx.drawerlayout.widget.DrawerLayout 
    xmlns:android=...
    android:id="@+id/..."
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout  //弹出时被覆盖部分
    	...>
    	...
    </LinearLayout>

    <LinearLayout
        android:id="@+id/..."
        android:layout_width="100dp" 
        android:layout_height="match_parent"
        android:layout_gravity="start"  //从左侧弹出 这里不能不写layout
        android:background="#FFFFFF"
        android:orientation="vertical"
        android:clickable="true">
        
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="设置选项"
            android:textSize="20sp"
            android:padding="20dp">
        </TextView>
    </LinearLayout>

</androidx.drawerlayout.widget.DrawerLayout>
```

编写点击逻辑

```
private DrawerLayout drawerLayout = findViewById(R.id....);
drawerLayout.setDrawerLockMode(DrawerLayout.LOCK_MODE_LOCKED_CLOSED);  //禁用drawerlayout的手势滑动 避免覆盖两侧按钮点击事件
drawerLayout.openDrawer(GravityCompat.START);
```

##### 使用Intent实现页面跳转

同时创建新页面的xml和class

**注** 新页面是可以使用onCreate和findViewById方法的

**注** 在新页面的xml中的最外层View中设置id为...以对应.java中的全面屏适配代码中的findViewById(R.id....)

```
setContentView(R.layout.activity_settings)  //在class里写上这段以连接xml和class
```

点击逻辑内

```
settingBtn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        // 1. 创建 Intent：参数分别为 (当前页面上下文, 目标页面类)
        Intent intent = new Intent(MainActivity.this, SettingsActivity.class);
        // 2. 执行跳转
        startActivity(intent);
        // 如果你希望跳转后不能通过返回键回到这一页，可以调用 finish();
    }
});
```

**注** 解释一下为什么使用finish()可阻止返回 跳转过程中相当于入栈 finish()相遇出栈

在AndroidManifest.xml中注册跳转后页面

```
<activity 
    android:name=".view.SettingsActivity"
    android:exported="false">
</activity>
```

##### 获取EditText文本框中输入的内容

```
String text = ....getText().toString();
```

##### 使用Toast发出简单warning

```
Toast.makeText(this, "...", Toast.LENGTH_SHORT).show();
//使用Toast的Tips
//老式匿名内部类写法 lambda写法无此问题
vlViewModel.getErrorLiveData().observe(this, new Observer<String>() {
    @Override
    public void onChanged(String error) {
        //报错 这里的this指向的是这个匿名的Observer对象
        Toast.makeText(this, error, Toast.LENGTH_SHORT).show(); 
        //必须这样写
        Toast.makeText(MainActivity.this, error, Toast.LENGTH_SHORT).show();
    }
});
```

##### 从其它页面获取数据至main

static方式 (无法保持数据持久化)

```
//MainActivity
public static Text currentText;
//OtherActivity
MainActivity.currentText = text;
```

#### 采用RecyclerView实现动态且滚动窗口

添加依赖

```
implementation("androidx.recyclerview:recyclerview:1.2.1")
```

编写局部窗口结构定义文件

```
//list_file.xml  单独的xml文件用于定义每一行长什么样
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout android:layout_width="match_parent"
    android:layout_height="40dp"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <ImageView
        android:id="@+id/list_file_icon"
        android:layout_width="40dp"
        android:layout_height="match_parent"
        android:padding="10dp">
    </ImageView>
    <TextView
        android:id="@+id/list_file_title"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text=""
        android:textSize="15sp"
        android:gravity="center_vertical">
    </TextView>
</LinearLayout>

//activity_main.xml
<androidx.recyclerview.widget.RecyclerView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/list_file"
    android:visibility="gone">
</androidx.recyclerview.widget.RecyclerView>
```

编写适配器

```
public class MainAdapter extends RecyclerView.Adapter<MainAdapter.MainViewHolder> {
    private List<String> fileList;
    private OnItemClickListener listener;
    // 定义点击回调接口
    public interface OnItemClickListener {
        void onItemClick(String fileName);
    }
	//构造函数
    public MainAdapter(List<String> fileList, OnItemClickListener listener) {
        this.fileList = fileList;
        this.listener = listener;
    }
    @NonNull
    @Override
    public MainViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        // 加载布局并创建 ViewHolder
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.list_file, parent, false);
        return new MainViewHolder(view);
    }
    @Override
    public void onBindViewHolder(@NonNull MainViewHolder holder, int position) {  //主要在此添加样式
        // 绑定数据
        String fileName = fileList.get(position);
        holder.tvFileTitle.setText(fileName);

        // 设置点击事件
        holder.itemView.setOnClickListener(v -> {
            if (listener != null) listener.onItemClick(fileName);
        });
    }
	//获取列表长度
    @Override
    public int getItemCount() {
        return fileList.size();
    }
    // 更新数据的方法(可选)
    public void updateData(List<String> newList) {
        this.fileList = newList;
        notifyDataSetChanged();
    }
    // ViewHolder：内部类，用来缓存控件
    static class MainViewHolder extends RecyclerView.ViewHolder {
        TextView tvFileTitle;
        ImageView ivIcon;
        public MainViewHolder(@NonNull View itemView) {
            super(itemView);
            tvFileTitle = itemView.findViewById(R.id.list_file_title);
            ivIcon = itemView.findViewById(R.id.list_file_icon);
        }
    }
}
```

activity中使用

```
private RecyclerView recyclerView;
private MainAdapter mainAdapter;
private List<String> pathList = new ArrayList<>();
@Override
protected void onCreate(Bundle savedInstanceState) {
    // 找到布局里的 RecyclerView
    recyclerView = findViewById(R.id.list_file); 
    // 重要 设置布局管理器，不然列表刷不出来
    recyclerView.setLayoutManager(new LinearLayoutManager(this));
    // 初始化适配器（传入空列表和点击回调）
    List<String> dataList = new ArrayList<>();  //此处的dataList仅是提供一个空值防止传null后续会被newList替代
    adapter = new FileAdapter(dataList, fileName -> {
        // 这里是点击每一行后的逻辑
        Toast.makeText(MainActivity.this, "你点击了：" + fileName, Toast.LENGTH_SHORT).show();
    });
    // 绑定适配器
    recyclerView.setAdapter(adapter);
    //观察并更新数据
    vlViewModel.getPathMenuData().observe(this, newList->{
        if(newList != null) mainAdapter.updateData(newList);
    });
}

```

#### FrameLayout(帧布局)实现层级效果(一层一层向上盖)

```
<FrameLayout>
```

#### 利用LayoutParams动态修改View的宽高

```
//onCreate()
View myView = findViewById(R.id.video_framework);
ViewGroup.LayoutParams params = myView.getLayoutParams();
params.height = 1000;  //设置具体的像素值 单位px
params.height = ViewGroup.LayoutParams.MATCH_PARENT;  //设置系统值
myView.setLayoutParams(params);

public int dpToPx(int dp) {  //dp转px
    float density = getResources().getDisplayMetrics().density;
    return Math.round((float) dp * density);
}
```

