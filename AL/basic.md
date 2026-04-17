# Assemble && Link

运行环境 

汇编器nasm(Intel语法) 链接器MinGW中gcc(实际工作的是ld)

 指令集架构x86-64 os平台win64 

```
nasm -f win64 Demo.asm [-o Demo.obj]  //此处-o是指定输出文件名
gcc Demo.obj -o Demo  //真正链接
```

指令集架构x86-32 os平台win32

```
nasm -f win32 Demo.asm [-o Demo.obj]
gcc -m32 Demo.obj -o Demo  //win下ld需要显式动态链接库路径很麻烦 推荐使用gcc
```

**注** 使用gcc而不是g++

指令集架构x86-64 os平台Linux64

```
nasm -f elf64 Demo [-o Demo]
ld Demo.o -o demo  //Linux下可直接ld
```

指令集架构x86-32 os平台Linux32

```
nasm -f elf32 Demo [-o Demo]
ld -m elf_i386 Demo.o -o demo
```

# Grammar

**注** 使用ld显式链接器默认_start为入口  使用gcc链接时默认main为入口  修改时替换两处代码即可

**注** win下使用ld链接 _start和main皆可

#### 内容

##### 初始化数据段

```
section .data
	array db 10, 20, 30, 40, 50  ;db声明定义字节数据
```
db  字节 8b
dw  字 16b
dd  双字 32b
dq  三字 64b

**注** 《计组》中一字是32/64位 汇编中一字16位为历史遗留问题

```
//注
汇编不区分大小写
汇编一行只能有一条语句
段的名字用字母或下划线开头 需要做到含义清晰且不能与保留字重名
汇编语言中用英文分号后的内容表示程序注释
不能在一个段的内部定义另一个段
```

##### 未初始化数据段

```
section .bss
	buffer resb 64  //预留64B
```

resb  预留字节

resw  预留字

resd  预留双字

resq  预留三字

##### 代码段

```
section .text
	global _start
	
_start:
	mov ...
```

mov  a, b	; b赋值给a

movzx    a, b  ; 带零扩展的传送 a位数大于b时 b赋值a的同时多出的高位会赋值为0

add   a, b	; 得到b+a的结果并赋值给a (a和b的位数需相等)

xor    a, a	; 异或指令 结果赋值a 此代码将a清0

**注** 异或两个性质 1)翻转位 某位与1异或即可1变0 0变1 2)可逆性 A异或B=C C异或B=A(常用于简单加密)

inc     a	    ; 即a += 1

loop  标签名   ; 默认搭配ecx使用 执行时会隐式执行两件事 1)ecx-=1 2)判断if ecx减一后不等于0则跳转至标签处 else 停止跳转执行下一条指令

int     0x80      ; (Linux32) int(interrupt) 向cpu发起0x80号软中断  此时eax告知os内核系统调用的功能号 eax存储的功能号不同相应得此时的ebx,ecx,edx存储信息的含义也不同

| **eax (功能号)** | **对应功能 (Linux x86)** | **ebx 的身份**          | **ecx 的身份**      | **edx 的身份** |
| ---------------- | ------------------------ | ----------------------- | ------------------- | -------------- |
| **1**            | **sys_exit** (退出)      | **退出状态码**          | (不使用)            | (不使用)       |
| **3**            | **sys_read** (读取)      | 文件描述符 (自哪读)     | 缓冲区地址 (存到哪) | 读取字节数     |
| **4**            | **sys_write** (写入)     | **文件描述符** (往哪写) | 字符串地址          | 字符串长度     |
| **5**            | **sys_open** (打开)      | 文件路径字符串地址      | 打开模式            | 权限设置       |

syscall	    ; (Linux64) syscall系统调用时rax内容为功能号

**注** eax时rax的低32位

**注** win的系统调用指令更为复杂

jmp    标签名  ; 无条件跳转指令

cmp   a, b       ;  减法比较 不改变流程不保存结果 只影响状态寄存器EFLAGS的标志位 a==b(ZF=1, CF=0, OF=0) a>b(ZF=0, CF=0) a<b(ZF=0, CF=1)

**注** CF=1在加法时表示有进位 在减法时表示有借位

test    a, b       ; 按位与 不改变流程不保存结果 只影响状态寄存器标志位ZF 当a&b结果为0时ZF为1
