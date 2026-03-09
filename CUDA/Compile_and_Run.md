#### Compile

##### 命令行方式

```
nvcc Demo.cu -o demo
```

**注** nvcc依赖c++编译器且在win环境下必须使用msvc编译器(x64) linux下必须使用gcc

```
//编译时可能报错
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v13.1\include\cuda_runtime_api.h(14276): warning C4819: 该文件包含不能在当前代码页(936)中表示的字符。请将该文件保存为 Unicode 格式以防止数据丢失
```

解决方案:

```
nvcc -g -G Demo.cu -o demo -Xcompiler "/utf-8"  //让nvcc调用cl.exe时带上utf8参数 别使用默认GBK  -g -G是为后续调试用
```

##### tasks.json方式  (Ctrl + F5)

```
{
    "version": "2.0.0",  //vscode任务系统的架构版本
    "tasks": [
        {
            "label": "build-cuda", 
            "type": "shell",
            "command": "nvcc",  //此处也可指定全路径
            "args": [
                "-g", "-G",  //调试用
                "${file}",  //自动获取当前打开的文件名
                "-o", "${fileDirname}\\${fileBasenameNoExtension}.exe",  //自动生成同名.exe
                "-Xcompiler", "/utf-8"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "problemMatcher": ["$gcc"]
        }
    ]
}
```

#### Debug  (F5)

```
//launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "GPU_Debug",
            "type": "cuda-gdb",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
            "preLaunchTask": "build-cuda",  //关联tasks.json任务名
            "cwd": "${fileDirname}"  //指定工作目录，确保程序能找到它需要的资源文件
        },
        {
            "name": "CPU_Debug",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
            "preLaunchTask": "build-cuda"
        }
    ]
}
```

**注** 使用Nsight Extension去实现.cu代码高亮等功能

##### WSL2

cuda-gdb必须在linux环境下运行 所以我们需要一个Linux系统 WSL2提供了一种轻量化虚拟化技术

```
//双击本地下载好的wsl.2.6.3.0.x64.msi安装wsl内核
wsl --install -d Ubuntu  //下载太卡可直接在MicrosoftStore里下载
//ubuntu在C:\Users\你的用户名\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu...\LocalState\ext4.vhdx实际上我们直接操作资源器里的虚拟磁盘文件就行
//VSCode安装WSL插件
//在WSL终端输入nvidia-smi如果能看到显卡信息说明硬件层已打通
//在WSL的Ubuntu中安装CUDA Toolkit(默认预装gcc)  严禁在WSL里装驱动
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb//下载NVIDIA的密钥环 针对 WSL2-Ubuntu 22.04
sudo dpkg -i cuda-keyring_1.1-1_all.deb  //安装密钥环
sudo apt-get update  //更新软件源
sudo apt-get -y install cuda-toolkit-12-6  //正式安装CUDA工具包
//安装完后添加CUDA编译器的环境变量
vim ~/.bashrc
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
source ~/.bashrc  //让环境变量立即生效而不用重新打开终端
nvcc --version
//连接vscode
mkdir -p ~/Projects/C_C++_Projects/CUDA
打开vscode插件并打开Ubuntu22.04即可自动连接
```

###### vscode(WSL)

使用rm命令进行删除操作
修改tasks.json和launch.json  不能有.exe 无需刻意指定编码"-Xcompiler", "/utf-8"



