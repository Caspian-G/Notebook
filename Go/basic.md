#### SDK

**注** 下载界面一般x86指32位 x86-64指64位  有的地方使用更精准的术语(386特指32位x86名字源于Intel80386处理器  amd64特指64位x86架构因为amd最先提出此标准)

```
//查看版本 与其它语言不同无--
go version
```

#### 配置环境

go1.16之后使用go module模式进行项目布局

```
go mod init <模块名>     //初始化模块/项目
go mod tidy             //下载依赖
```

GOPATH  存放全局命令行工具 和 作为依赖包的全局缓存

GOCACHE  编译时生成的中间文件缓存 可安全删除

GOENV  go内设环境路径

GOMODCACHE  所有项目共享的依赖包源码仓库

GOPROXY  设置下载源

```
//查看上述路径
go env
```

#### Run and Build

```
go run <文件名>     //运行文件 中间编译文件在%TEMP%
go build           //编译并打包
```

