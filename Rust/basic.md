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
rustup --version  //rust更新器 rustup update
rustc --version  //rust编译器
cargo --version  //rust的编译管理器、包管理器和通用工具 使用cargo会调用rustc
rustdoc  --version  //rust的文档工具
rustup toolchain list  //查看链接器
```

