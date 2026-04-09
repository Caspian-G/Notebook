#### rustup-init.exe

```
//Rustup metadata and toolchains will be installed into the Rustup home directory, located at:
C:\Users\13917\.rustup  //This can be modified with the RUSTUP_HOME environment variable.

//The Cargo home directory is located at:
C:\Users\13917\.cargo  //This can be modified with the CARGO_HOME environment variable.

//The cargo, rustc, rustup and other commands will be added to Cargo's bin directory, located at:
C:\Users\13917\.cargo\bin  //This path will then be added to your PATH environment variable by modifying the PATH registry key at HKEY_CURRENT_USER\Environment.

//You can uninstall at any time with rustup self uninstall and these changes will be reverted.

Current installation options:  //我使用的配置
   default host triple: x86_64-pc-windows-gnu  //使用gcc链接而非msvc
     default toolchain: stable
               profile: default
  modify PATH variable: yes
```

#### 查看版本

```
rustup --version  //rust更新器 rustup update 更新命令 rustup self uninstall 卸载命令
rustc --version  //rust编译器
cargo --version  //rust的编译管理器、包管理器和通用工具 使用cargo会调用rustc
rustdoc  --version  //rust的文档工具
rustup toolchain list  //查看链接器
```

##### 配置Cargo 使用GNU工具链

C:\Users\\<你的用户名>\\.cargo\config.toml

```
[target.x86_64-pc-windows-gnu]
linker = "...\\mingw64\\bin\\gcc.exe"  //路径必须使用"\\"
//归档器 用于创建静态库
ar = "...\\mingw64\\bin\\ar.exe"
```

#### 创建并运行HelloWorld

```
cargo new 项目名
cd .\项目名
cargo run
cargo run --release  //重度优化和去除无用代码并运行
```

#### 添加依赖

项目文件夹下Cargo.toml

```
[dependencies]
ferris-says = "0.3.1"  //示例
```

**注** Cargo.lock记载本地项目以及依赖的版本号

```
cargo build  //安装依赖 Dev使用
cargo build --release  //重度优化和去除无用代码 常在release分发时使用
```

**注** cargo run 和 cargo build 都会调用 rustc    cargo run 包含 cargo build

#### 文档

[Rust相关文档](https://www.rust-lang.org/zh-CN/learn)

