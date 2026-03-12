#### 概念

单片机是嵌入式的一个子类

嵌入式开发和移动端开发基本使用ARM架构芯片

STM32F103C8T6  缺陷 flash闪存容量几十KB无法处理过多代码量的程序 CPU频率72MHz无法处理有极高计算需求的场景

STM32F103C8T6 F103指F1系列基础款 C指引脚数 8指闪存容量编号 T指封装编号 6指使用温度范围编号

STM32F103C8T6 供电在2.0-3.6V 超出需加稳压器

#### 片上资源/外设

| 英文缩写 | 名称               | 英文缩写 | 名称               |
| -------- | ------------------ | -------- | ------------------ |
| NVIC     | 嵌套向量中断控制器 | CAN      | CAN通信            |
| SysTick  | 系统滴答定时器     | USB      | USB通信            |
| RCC      | 复位和时钟控制     | RTC      | 实时时钟           |
| GPIO     | 通用I/O口          | CRC      | CRC校验            |
| AFIO     | 复用I/O口          | PWR      | 电源控制           |
| EXT1     | 外部中断           | BKP      | 备份寄存器         |
| TIM      | 定时器             | IWDG     | 独立看门狗         |
| ADC      | 模数转换器         | WWDG     | 窗口看门狗         |
| DMA      | 直接内存访问       | DAC      | 数模转换器         |
| USART    | 同步/异步串口通信  | SDIO     | SD卡接口           |
| I2C      | I2C通信            | FSMC     | 可变静态存储控制器 |
| SPI      | SPI通信            | USB OTG  | USB主机接口        |

**注** 具体型号的具体外设看官方手册

#### STM32分类表

| 大类        | 系列 | 内核       | 特性描述                             |
| ----------- | ---- | ---------- | ------------------------------------ |
| MCU         | G0   | Cortex M0+ | 全新入门级MCU                        |
| MCU         | G4   | Cortex M4  | 模数混合型MCU                        |
| MCU         | F0   | Cortex M0  | 入门级MCU                            |
| MCU         | F1   | Cortex M3  | 基础型MCU                            |
| MCU         | F3   | Cortex M4  | 混合信号MCU                          |
| MCU         | F2   | Cortex M4  | 高性能MCU                            |
| MCU         | F4   | Cortex M4  | 高性能MCU                            |
| MCU         | F7   | Cortex M7  | 高性能MCU                            |
| MCU         | H7   | Cortex M7  | 超高性能MCU，部分型号有双核（M7+M4） |
| 超低功耗MCU | L0   | Cortex M0+ | 超低功耗MCU                          |
| 超低功耗MCU | L1   | Cortex M3  | 超低功耗MCU                          |
| 超低功耗MCU | L4   | Cortex M4  | 超低功耗MCU                          |
| 超低功耗MCU | L4+  | Cortex M4  | 超低功耗高性能MCU                    |
| 超低功耗MCU | L5   | Cortex M33 | 超低功耗高性能安全MCU                |
| 超低功耗MCU | U5   | Cortex M33 | 超低功耗高性能大容量安全MCU          |
| 无线MCU     | WB   | Cortex M4  | 双核无线MCU                          |
| 无线MCU     | WL   | Cortex M4  | 远程无线MCU                          |
| MPU         | MP1  | Cortex A7  | 双核超高性能MPU  (可运行Linux)       |

#### STM32F1 系列产品线

| 产品线        | 类型       | CPU 频率 | 主要特性                           |
| ------------- | ---------- | -------- | ---------------------------------- |
| STM32F100     | 超值型     | 24 MHz   | 电机控制、CEC功能                  |
| STM32F101     | 基本型     | 36 MHz   | 高达 1MB Flash                     |
| STM32F102     | USB 基本型 | 48 MHz   | USB FS（全速）                     |
| STM32F103     | 增强型     | 72 MHz   | 高达 1MB Flash、电机控制、USB、CAN |
| STM32F105/107 | 互联型     | 72 MHz   | 以太网 MAC、CAN、USB 2.0 OTG       |

#### 系统结构

![系统结构](https://i-blog.csdnimg.cn/blog_migrate/80fc62ae0f7f93356c8127a0bf27d08b.png)

#### STM32F103C8T6引脚

![](https://i-blog.csdnimg.cn/blog_migrate/02a69795d06199a80236b81727a9382e.png)

#### STM32F103C8T6引脚定义表

![](https://i-blog.csdnimg.cn/blog_migrate/39507943a24842bd41a3e8c47b1d50d7.png)

#### STM32F103C8T6最小系统板

![](https://i-blog.csdnimg.cn/blog_migrate/98fef2033fcd0e4db345af4cce588bd5.png)