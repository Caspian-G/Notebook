# application配置文件具体设置

application.properties    application.yml    application.yml

```
server.port=8080(default)  //被访问端口号
server.servlet.context-path=/...  //被访问源路径

spring.data.redis.host=localhost(default)
spring.data.redis.port=6379(default)
```

#### 实现网络路径与本地文件映射

##### 法一 使用配置文件 该方法相当于修改了static文件夹的路径

```
spring:
  web:
    resources:
      # 配置静态资源映射，'file:' 前缀表示本地磁盘路径
      static-locations: file:D:/videos/
  mvc:
    # 配置访问路径前缀（可选），这样访问时需加上 /videos/**
    static-path-pattern: /videos/**
```

#### 法二 WebConfig配置类

```
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        // 将 /videos/** 的请求映射到本地的 D:/videos/ 文件夹
        // 注意 'file:' 前缀不能少
        registry.addResourceHandler("/videos/**")  //**必需
                .addResourceLocations("file:D:/videos/");
        // 如果你想添加多个路径，继续调用即可
        // registry.addResourceHandler("/images/**").addResourceLocations("file:E:/images/");
    }
}
```



