#### 程序烧录 调试

STM32的GND SWCLK SWDIO 3.3 通过杜邦线对应连接到ST Link

前后两头usb接口应只插一个

使用ST-LINK V2进行供电、烧录和调试前 先独立插入ST-LINK下载器并查看设备管理器中有无该设备同时有无驱动程序

Keil 中 Options for target -> Debug -> (注意选择ST-Link Debugger)Setting 可查看STLINK是否正常连接

#### 程序擦除

Options for target -> Utilities -> Erase Full Chip 

退回主菜单 Flash -> Erase

