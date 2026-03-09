#### File

```
//开始 默认路径cas@HugeBox:~$
~是指/home/cas  //在当前默认目录下有完整读写权限无需sudo 存放代码、配置文件、个人数据
//根路径
/  			   //这是整个文件系统的起点 存放系统程序(/bin)、配置文件(/etc)、安装的软件(/usr)等
```

#### Command

```
pwd  //回显具体路径
mkdir -p  //创建文件夹 -p允许递归创建多层
cd  //跳转到路径
dir  //遍历文件夹
rm  //删除文件
./...  //运行当前文件夹下的...

sudo apt update  //更新软件包列表

sudo apt install build-essential -y  //apt是Ubuntu软件管家 build-essential是gcc开发工具包 -y指同意
注 apt会把软件装到系统级的目录(/usr/bin或/usr/lib)而不是你当前文件夹下
gcc --version
```

#### Vim

```
vim ~/.bashrc  //打开环境变量配置文件
//按o在当前行下方新开一行并进入插入模式
//保存并退出 Esc + :wq
source ~/.bashrc  //立即生效
```

