# JavaWeb交互

**接口 (ApiService)**：菜单 定义了你能点什么菜（比如 `getPathMenu`）

**Retrofit**：外卖员 负责把你的请求发出去 并把结果带回来

**ViewModel & LiveData**：餐桌 数据拿回来后放在这里 Activity 只要看着桌子 有菜了（数据变了）就开吃（刷新 UI）

**Retrofit**  包含  Gson: 负责处理JSON数据  OkHttp: 一个http客户端

needed

```
dependencies{
	implementation("com.squareup.retrofit2:retrofit:2.9.0")
	implementation("com.squareup.retrofit2:converter-gson:2.9.0")
}
```

给予http网络权限(Android默认不支持http 只支持https)

```
<uses-permission android:name="android.permission.INTERNET" />
<application> android:usesCleartextTraffic="true"
```

#### 编写接口

```
public interface ApiService {
    @GET("VL/Func/PathMenu")
    Call<List<String>> getPathMenu(@Query("root")String rootValue);  //"root"对应Key名
}
```

#### ViewModel中编写LiveData容器 同时调用接口

```
public class VlViewModel extends ViewModel {
    private final MutableLiveData<List<String>> pathMenu = new MutableLiveData<>();
    public LiveData<List<String>> getPathMenuData() {
        return pathMenu;
    }
    public void fetchPathMenu(String baseUrl, String apiValue) {  //此处因为接口需传值所以定义了apiValue
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(baseUrl)
                .addConverterFactory(GsonConverterFactory.create())
                .build();  //收集之前填好的所有配置 把它们塞进一个复杂的Retrofit实例里并初始化好底层的网络引擎OkHttp
        ApiService api = retrofit.create(ApiService.class);
        //enqueue异步运行
        api.getPathMenu(apiValue).enqueue(new Callback<List<String>>() {  //此处如果apiValue为null则不会传参 必须写""才能保证传参但在服务器端时使用默认值
            @Override
            public void onResponse(Call<List<String>> call, Response<List<String>> response) {
                if (response.isSuccessful() && response.body() != null) {
                    pathMenu.postValue(response.body());
                }
            }
            @Override
            public void onFailure(Call<List<String>> call, Throwable t) {
                Log.e("NetworkError", t.getMessage());
            }
        });
    }
}
```

#### Activity类

```
vlViewModel.getPathMenuData().observe(this, newList->{
    if(newList != null) mainAdapter.updateData(newList);
});
```

