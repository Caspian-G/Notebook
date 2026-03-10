##### 运算符  (针对基本类型变量)

| **运算符** | **含义** |
| ---------- | -------- |
| **&**      | 按位与   |
| **\|**     | 按位或   |
| **^**      | 按位异或 |
| **~**      | 按位取反 |
| **<<**     | 左移     |
| **>>**     | 右移     |

##### 在不改变其它位的情况下对某几个位进行设值

```
//比如改变GPIOA状态
GPIOA->CRL &= 0XFFFFFF0F;    //将第4~7位清0
//然后再与需要设置的值进行|或运算:
GPIOA->CRL |= 0X00000040;    //设置相应位的值(4) 不改变其他位的值
```

##### 移位操作提高可读性

```
SysTick->CTRL |= 1 << 1;  //将CTRL寄存器的第一位设置为1 (1 << 1: 0000 0010)
SysTick->CTRL |= 0X0002;  //也可以实现 但可读性稍差
```

##### 清除某位

```
SysTick->CTRL &= ~(1 << 0) ;    //关闭 SYSTICK
```

##### 按位异或操作

```
GPIOB->ODR ^= 1 << 5;  //相当于使GPIOB->ODR第5位取反
```

##### 条件编译

```\
//#ifdef当标识符被定义过时执行程序段1
#ifdef 标识符
  程序段 1
#else
  程序段 2
#endif

//#ifndef当标识符没有被定义过时执行程序段1
#ifndef 标识符
  程序段 1
#endif

//#if !define(...)  !define()是一个条件表达式返回值为1或0
#if !defined(SOME_MACRO)  
  #define HSE_VALUE 24000000U  
#endif
```

**注** U无符号整型 UL无符号长整型 F浮点型 (不区分大小写)

##### extern变量声明

```
extern uint16_t speed_x;  //extern可置于变量或函数前 表示变量或函数被定义在别的模块中 注意在别的模块只能定义一次
```

##### 引用结构体成员变量

```
// 访问结构体变量的成员
person1.age = 25;
// 如果有一个指向结构体的指针
struct Person *ptr = &person2;
ptr->age = 30;
```

##### 结构体成员的内存分配与对齐

理论上 结构体所占用的存储空间是各个成员变量所占的存储空间之和 但是为了提高CPU的访问效率 采用了内存对齐方式:

(1)结构体的每一个成员起始地址必须是自身类型大小的整数倍

(2)结构体大小必须是结构体占用最大字节数成员的整数倍

```
struct test 
{ 
    char a;
    int b;
    float c;
    double d;
}mytest;
//假设1B为一个地址 char占1B int占4B(起始地址需要为4所以char被扩展到4B) float4B double8B 总24B是最大8B的整数倍
```

##### volatile

强制编译器减少优化 告诉编译器必须每次去内存中取变量值而不是去保存了数据的寄存器取

```
#define __I volatile
#define __O volatile
#define __IO volatile
#define __IM volatile const  //const是软件层面只读无法修改 但硬件层面上可以修改
#define __OM volatile
#define __IOM volatile
```

##### static

