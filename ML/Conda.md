# Conda

使用miniforge 开源免费 且空间占用小

Introduce anaconda miniconda forge miniforge mamba:

https://www.bilibili.com/video/BV1Fm4ZzDEeY/?spm_id_from=333.337.search-card.all.click&vd_source=061767036ee163dce2ad1405b47d2711

#### 安装miniforge

Github下载.exe+安装+设置环境变量+cmd:conda init

#### 创建新环境  (以下所有命令中conda替换mamba可加速下载速度)

```conda create -n 环境名 python=预装的py版本号```

#### 激活环境

```conda activate 环境名```

#### 退出环境

```conda deactivate```

#### 安装软件包

```conda install 软件包名(如:numpy pandas)```

#### 列出所有环境

```conda env list```

#### 删除环境

```conda env remove -n 环境名```

#### 更新软件包

```conda update --all```

#### 使用conda-forge软件源(少用)

搜索软件包：`conda search -c conda-forge package_name`

安装Conda-Forge软件包：`conda install -c conda-forge package_name`

**注** 上述init过程conda可能会自动生成profile.ps1文件 这个配置文件可以让powershell每次启动自动加载conda环境但我们必须给文件操作powershell的权限 `Set-ExecutionPolicy RemoteSigned`

**注** 当conda上没有要的包用pip
