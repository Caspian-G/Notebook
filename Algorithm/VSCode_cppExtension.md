# C++ Extension

#### 设置全局配置

```
>  //在vscode顶部搜索栏输入即可找到"C/C++:Edit Configurations(UI)" 修改后会生成对应的.vscode\c_cpp_properties.json文件
```

#### 设置局部配置

```
使用插件编译并运行.cpp文件 打开文件夹下会自动生成.vscode\tasks.json可进行局部配置
```

#### Complie

##### tasks.json方式  (Ctrl + F5)

```
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C++: Build with g++",
            "command": "D:\\Tools\\Compiler_for_C_or_C++\\mingw64\\bin\\g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "-fexec-charset=GBK" // 解决 MinGW 在 Windows 控制台中文乱码的小技巧
            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": ["$gcc"],
            "group": {
                "kind": "build",
                "isDefault": true
            }
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
            "name": "C++_Debug",
            "type": "cppdbg", //MinGW(gcc的win移植版开发环境)必须使用cppdbg
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "D:\\Tools\\Compiler_for_C_or_C++\\mingw64\\bin\\gdb.exe", //必须指向你MinGW里的 gdb
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C++: Build with g++" //这里必须对应tasks.json的label
        }
    ]
}
```

**注** VSCode里Debug的时候console里需盲打输入值 且输入两次回车确定输入 
