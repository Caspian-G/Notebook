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
wsl --install  //默认安装Ubuntu
//VSCode安装WSL插件 
```



