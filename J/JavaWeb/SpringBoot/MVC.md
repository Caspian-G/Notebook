# MVC框架

#### Application  (Gate)

```
@SpringBootApplication
public class Application{
	SpringApplication.run(Application.class, args).psvm;
}
```

**注** 统一加入注解方式 例@Mapper @MapperScan("com.example.demo.mapper")  // 扫描整个包

#### Controller

```
@RestController
public class Controller{
	...
	@GetMapping("/hello")  //子网址  这里的GetMapping其实是AllowGetMapping
	public String controller(){
		"...".sout;  //console输出
		return "...";  //JSON数据输出
	}
	@GetMapping("/hello1")  //子网址1
	public String controller1(){...}
}
```

@GetMapping("/")是@RequestMapping(value="/", method=RequestMethod.GET)的精简写法  @RequestMapping()中method可以是GET POST DELETE等

@RestController(返回JSON数据)=@Controller(返回页面)+@ResponseBody(告诉spring不要找视图，直接把返回数据写进http响应体)

##### @Controller使用

静态：res\static\hello.html

return "hello.html"  //return只在static文件夹下查找资源

动态：res\templates\hello.html

```
<denpendency> spring-boot-starter-thymeleaf  //加入该依赖spring就只在templates下查找资源 static下仅存放静态图片等资源
```

....html:  `<h1 th: text="${...}">` 

DemoController.java: `public String ...(Model model){ model.addAttribute("...", ...); return "....html"; }`

##### 参数获取方式

###### 法一 @RequestParam

```
@GetMapping("/api/user")
public String getUser(@RequestParam(value = "id", defaultValue = "1") String id,  //这里的value是key defaultValue是key的默认赋值也就是默认key的默认值
                      @RequestParam(value = "name", required = false) String name)  //required指定是否必须存在
{
    // 访问：/api/user?id=123&name=张三
    return "用户ID：" + id + "，姓名：" + name;
}
```

###### 法二  @PathVariable

```
@GetMapping("/api/user/{id}")
public String getUserById(@PathVariable String id) {
    // 访问：/api/user/123
    return "用户ID：" + id;
}
// 多个路径变量
@GetMapping("/api/user/{userId}/order/{orderId}")
public String getUserOrder(@PathVariable String userId, 
                          @PathVariable String orderId) {
    return "用户：" + userId + "，订单：" + orderId;
}
```

#### Service

```
@Service
public class UserService{
	...;
}
```

**注** 注解与类名不可相同 包引用时会冲突



#### 各数据生命周期

| 作用域          | 生命周期                         | 例子                 | 代码示例               |
| :-------------- | :------------------------------- | :------------------- | ---------------------- |
| **Model**       | 一次请求                         | 从 A 页面传到 B 页面 | `model.addAttribute()` |
| **Request**     | 一次请求                         | 请求参数             | `@RequestParam`        |
| **Session**     | 一次会话（用户访问到关闭浏览器） | 登录状态             | `HttpSession`          |
| **Application** | 应用运行期间（服务器启动到关闭） | 全局配置             | `ServletContext`       |
| **全局变量**    | 应用运行期间                     | static 变量          | `private static List`  |

