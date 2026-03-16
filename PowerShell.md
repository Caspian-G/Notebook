# PowerShell

增强版cmd  且不像cmd被高度集成在os中  ps基于.NET它的管道传递.NET对象

ps可更新

```powershell
winget upgrade Microsoft.PowerShell --source winget
```

#### cmd命令

```
dir  //扫文件夹
```



#### .bat

```
@echo off  //关闭回显
[call] ...  //同一窗口中同步执行
[start]  //启动  在新窗口中异步执行
[pause]
[exit]  //运行完退出父命令行(在cmd中运行.bat或提前退出.bat时生效 如果直接双击.bat运行完都会退出)
```

**注** 如果使用.bat启动Ubuntu(wsl) call 后写 `wsl -d Ubuntu-22.04 --cd ~`

