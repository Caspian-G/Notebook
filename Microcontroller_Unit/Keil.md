#### Keil_IDE and Pack_Installer

D:\Keil_uVision5(MDK-ARM_v5.43)\Keil_v5\UV4

**注** Pack_Installer可手动导入File -> Import [pack下载网址](https://www.keil.arm.com/packs/?q=STM32F1xx_DFP&pack-search=)

#### Packs_Location

D:\Keil_uVision5(MDK-ARM_v5.43)\Arm\Packs

#### SignUp Community

管理员身份运行  File -> License Management  复制CID

需要keygen注册机 获取LIC 在License Management里add即可

#### Create Project

Project -> new uVision Project

**注** 需选择导入库 STM...(对应型号) 必选(RTE)CMSIS -> CORE 、 Device -> Startup

**注** 可选 StdPeriph Drivers -> GPIO(输入输出端口控制高低电平输入输出) RCC(时钟控制) 

##### Create File

File -> new  写入代码  Save As 设置路径和名称(必须.c)建议保存至和.uvprojx同级  最后需右键项目里的Source Group 1 选择Add Existing Files

