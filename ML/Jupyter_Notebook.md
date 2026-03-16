# JN

使用conda或mamba创建虚拟环境后

activate环境+conda(mamba) install jupyter

#### 修改根目录

初始化生成config文件: ```jupyter notebook --generate-config```   and   ```jupyter-lab --generate-config```

修改config文件中**c.ServerApp.root_dir**路径

#### 快速启动

使用批处理命令

```@echo off```

```call mamba activate JN```

```jupyter notebook```

