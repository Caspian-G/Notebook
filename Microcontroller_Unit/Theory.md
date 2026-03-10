#### 概念

单片机是嵌入式的一个子类

嵌入式开发和移动端开发基本使用ARM架构芯片

STM32F103C8T6  缺陷 flash闪存容量几十KB无法处理过多代码量的程序 CPU频率72MHz无法处理有极高计算需求的场景

STM32F103C8T6 F103指F1系列基础款 C指引脚数 8指闪存容量编号 T指封装编号 6指使用温度范围编号



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