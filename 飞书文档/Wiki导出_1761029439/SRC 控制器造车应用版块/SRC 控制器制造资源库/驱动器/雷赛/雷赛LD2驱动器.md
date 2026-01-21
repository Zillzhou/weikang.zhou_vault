# 雷赛LD2驱动器

## 雷赛LD2驱动器

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.29

## 语雀迁移至飞书

## 使用中

## 说明

本文档针对机器人自动化改造过程进行规范，使公司产品符合行业标准，保证产品质量稳定，使技术人员在进行改造过程中有章可循。

机器人自动化改造涉及传感器众多，建议采用我司标准核心控制器线束TE23,TE35,本文档以核心控制器标准线束为蓝本进行作业指导。

注：本文档只适用于改造参考，不可作为技术协议及其他承担责任的内容。
## 一、适用范围

本技术规范适用于公司使用雷赛LD2系列驱动器canopen通讯驱动器进行自动化改造的研发、生产、调试的技术人员。
### 二、调试资源
### 📎LD2-CAN系列低压伺服系统使用手册 (2).pdf
### 📎MotionStudio_v1.4.5.zip
## 三 、改造和安装
### 3.1 进行改造（底盘驱动器部分）

### 3.1.1 行走电机驱动器安装方式

## 1.驱动器需要与车体进行可靠固定，检查驱动器与对应电机的三相线、编码器线路连接正确；

2.当机器人安装有多个驱动器(数量≥2)时，所有从站的CAN_L,CAN_H引脚直接相连即可，尽量采用串联方式接线，如图4.4.1所示；若驱动器仅提供一个通讯接口，无法完成驱动器的can 线串联时，则将所有驱动器can线引出后把所有can_H压入同一德驰插筒连接器，将所有can_L压入同一德驰插筒连接器，接入德驰DT06-2S公头，最后与TE35中的32、33号线（can1)相连.

由于部分驱动器没有提供级联接口，只能通过从总线上接引线的的方式来串联，这里引线的长度需小于10CM.

注：在改造过程中若由于驱动器连接所需要的线束数量不足，无法实现在驱动器端进行快速串联，可采用图 4.4.2 所示连接方式，但不推荐。

https://assets.seer-group.com/robokit/ethan/mt49.png

## 图4.4.1  图4.4.2

为保证can通讯质量，需要在距离核心控制器最远的驱动器上或总线末端安装终端电阻（阻值一般是120ohm).（注：客户可在购买驱动器时向驱动器厂商提出购买配套终端电阻）

### 3.CAN终端电阻是否正确打开的检测方法：

关机断电，断开驱动器和控制器的CAN连接线（如图4.4.1中Driver4和控制器之间的位置），使用万用表电阻档测量驱动器侧的CAN总线上CAN_L、CAN_H之间电阻，电阻值为120Ω则正确，如图4.4.3所示。电阻值明显小于120Ω（如60Ω),则说明至少有两个驱动器打开的终端电阻。

断开图4.4.1中Driver1和Driver2之间的连接线，使用万用表电阻档测量Driver1侧的CAN总线上CAN_L、CAN_H之间电阻，电阻值为120Ω则正确，如图4.4.3所示。如果电阻值明显大于120Ω（如几KΩ),则说明终端电阻打开的位置不在CAN总线末端，需要调整。

https://assets.seer-group.com/robokit/ethan/mt50.png

## 图4.4.3

### 4. 急停 ：高电平触发，DI3接GND，COM_IN 接 +24V，则急停输入有效

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-1.png
1680252082492-9d18fb80-7282-4a9e-b892-4ee55ecd656c.png

### 四、 驱动器参数配置 行走 （速度模式）

### 4.1 上位机配置

### 4.1.1 连接： 📎LD2-CAN系列调试软件.zip

### 通过USB_232连接，选择正确的通信端口

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-5.png

### 4.2 设备地址（canID）

### 以拨码开关为准，如果需要软件配置，拨码开关打到off档

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-6.png

### 4.3 看门狗配置

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-7.png

## 保存厂商参数

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-8.png

## 下载参数

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-9.png

### 4.4 关闭PDO，RPDO，TPDO都关掉

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-10.png
### 4.5 静止下蠕动问题
1689762631176-6b1ce2f8-c0aa-4a03-a2b3-ed143e7f9080.png

### 无如下参数，可以在功能那选下管理员，密码5521
1692063969321-da7bfc6f-eb63-4fc0-8dcf-14411f8f74a8.png

以上所有参数，重上电后生效，重上电后再次检查参数是否修改成功

### 五、 驱动器参数配置 舵轮（位置模式）

更新绝对位置模式固件 📎LeadshineFirmwareLoader_v3.2.0_LD2_CAN_20230407.zip

## 打开上位机软件
1681293789445-dacb7a5b-1079-48a4-825f-35c0e0bd47da.png 

；观察上面显示接口是否为正确的接口，烧录文件会自行选择目录；的然后电机烧录

1681293857297-6ef5d561-7a9e-461b-856f-1ec9a53740cd.png

### 等待烧录成功，断电重启驱动器 （固件烧录过程中不要断电）

1681293909061-1633c9c5-2ab0-410f-97fe-47c56bd478e7.png

### 5.1 上位机配置

### 5.1.1 连接： 📎LD2-CAN系列调试软件.zip

### 通过USB_232连接，选择正确的通信端口

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-5.png

### 5.2 设备地址（canID）

### 以拨码开关为准，如果需要软件配置，拨码开关打到off档

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-6.png

### 5.3 看门狗配置

## 心跳时间设置为300ms

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-7.png

### 5.4 回零模式配置

### 设置回零模式为17负限位回零（6098）
### 设置回零速度为20000（6099 01）

1680252232418-1f058970-2bdf-433f-90a0-9761d54bb9bd.png

## 设置位置模式加速度与速度

1681210181664-f7cb7b19-3936-4a1f-bca2-65b2f3f90996.png

### 5.5 限位与急停开关
第一步判断限位开关的线序，一般传感器的线序为：棕正，蓝负，黑信号。 此处线序需要向厂家确认。
第二步判断舵机的限位开关是 NPN 还是 PNP。下面提到不同类型传感器的两种接法：
若是 NPN 型（低电平触发） ：将所有驱动器的 COMI 并联后接入 DCDC 24V+ 输出；将所有驱动器 IN3并联后连接 SRC2000 急停输出 1+(TE35 4 号线），将另一根急停输出 1-（TE35 5 号线）连接 DCDC 24V- 输出；
若是 PNP 型（高电平触发） ：将所有驱动器的 COMI 并联后接入DCDC 24V- 输出；将所有驱动器 IN3 并联后连接 SRC2000 急停输出 1+(TE35 4 号线），将另一根急停输出 1-（TE35 5 号线）连接 DCDC 24V+ 输出。
限位开关 ，使用负限位标零限，接入 IN6 ， 并按照图中配置IN6为负限位，IN3为急停功能
1681361412931-f91e7b7a-b0a2-45f3-af4e-86f78137fc0e.png

## 保存厂商参数

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-8.png

## 下载参数

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-9.png

### 5.6 关闭PDO，RPDO，TPDO都关掉

https://assets.seer-group.com/robokit/yanshuo/configuration-of-tne-LeiSai_LD2-device-10.png

以上所有参数，重上电后生效，重上电后再次检查参数是否修改成功

### 七、附录

### 7.1通讯线序：

## 1. 232通讯线序：

1680252613370-afeffe44-3507-4ddd-9e1e-9633f489b259.png

### 2. can通讯线序

1680252636558-bbcc2520-851a-4706-96d1-c06a01414aa2.png

### 7.2 驱动器常见错误码

### 根据roboshop上对应的报错码，在表格中找到对应报错

1680252781262-cf49b9c9-f685-491e-b286-9c9e93ab1e0c.png

1680252798181-1184e994-e028-4c8f-a888-1ede5f8263ee.png

1680252812690-f3105a58-8706-4c01-a1ca-76dfe2f79e41.png

1680252828932-4991f682-2a5f-4074-880a-d5c6b572eeb4.png

1680252846463-3a37c894-0fdf-4c1d-96eb-6462516c61b1.png

1680252859043-e146cf66-08c1-4c0b-8dc4-523145de2878.png

1680252874263-1078430e-24ad-479c-ae1b-7d7865e465a0.png

1680252884724-4bc84829-912f-4d2c-b2c6-f781d7841b12.png
