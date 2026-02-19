# Add Redis

needed

```
<denpendency> spring-boot-starter-data-redis
```

#### redis安装方法

Github: [Releases · redis-windows/redis-windows](https://github.com/redis-windows/redis-windows/releases) (开发者利用 Cygwin 等工具，将 Redis 源码编译成可在 Windows 运行的程序)

#### 其它方法

##### win下使用docker在linux环境的容器下运行最新版本redis(>=7.4版本可使用map内字段级过期的新特性)

Docker简述: 将项目和其相关依赖打包成docker镜像 docker镜像可在不同环境容器下运行 dockerfile可用于生成自定义镜像(此处redis可直接拉取)

**注** docker安装后无法使用软链接方式降低c盘存储空间消耗(系统服务无权限访问D盘) 应在install时使用命令行方式定义安装路径`start /w "" "Docker Desktop Installer.exe" install --installation-dir="D:\Tools\Docker" --wsl-default-data-root="D:\Tools\Docker\wsl\data"` --wsl-default-data-root定义镜像容器等文件存放路径且需要**管理员模式启动快捷方式**

**注** 运行`docker run -d --name MyRedis -p 6379:6379 redis` 命令时可能会因为网络原因导致docker pull失败

#### Redis 启动

cmd: redis-server.exe redis.conf  //服务器端 不加.conf即为默认配置(=双击.exe)

cmd: redis-cli.exe -h 127.0.0.1 -p 6379  //客户端 -h指定主机地址 -p指定端口号

#### Redis 缓存使用

**注** Redis一般被@Service调用

```
@Autowired
private RedisTemplate redisTemplate
@...
public void ...(){
	redisTemplate.boundValueOps("Key").set("Value");
	//redisTemplate.boundValueOps("Key").set("Value", 10, TimeUnit.MINUTES);  10分钟后销毁
	redisTemplate.boundValueOps("Key").get();
	redisTemplate.delete("Key");
}
```

**注** 在测试类中使用redis需@SpringBootTest(classes = {....class, DataRedisAutoConfiguration.class})

#### Redis 配置设置

application.yml:

```
spring:
  data:
    redis:
      host: localhost
      port: 6379(default)
      # database: 0  # 数据库索引（默认为0）
      # timeout: 1800000  # 连接超时时间（毫秒）
      # lettuce:
        # pool:
          # max-active: 20  # 连接池最大连接数（使用负值表示没有限制）
          # max-wait: -1  # 最大阻塞等待时间（负数表示没有限制）
          # max-idle: 5  # 连接池中最大空闲连接
          # min-idle: 0  # 连接池中最小空闲连接
```

