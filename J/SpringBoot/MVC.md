# MVC框架

#### Controller

```
@RestController
public class Controller{
	...
	@GetMapping("/hello")  //子网址
	public String controller(){
		"...".sout;  //console输出
		return "...";  //页面输出
	}
	@GetMapping("/hello1")  //子网址1
	public String controller1(){...}
}
```

#### Application

```
@SpringBootApplication
public class Application{
	SpringApplication.run(Application.class, args).psvm;
}
```

