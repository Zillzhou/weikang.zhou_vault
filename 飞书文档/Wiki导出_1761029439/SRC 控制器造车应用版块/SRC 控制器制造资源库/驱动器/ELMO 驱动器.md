# ELMO 驱动器

## ELMO 驱动器

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.29

## 语雀迁移飞书

## 使用中

## 一、适用范围

本文档针对机器人改造过程进行规范，使公司产品符合行业标准，保证产品质量稳定，使技术人员在进行改造过程中有章可循。

机器人自动化改造涉及传感器众多，建议采用我司标准核心控制器线束 TE23、TE35，本文档以核心控制器标准线束为蓝本进行作业指导。
### 二、调试资源

## USB-485/232 转换器

1677573096889-8ac3ee87-bb2e-46e1-839f-4654eafbafe8.png

## 驱动器相关手册：

### Elmo 伺服CAN通信-DS402控制参考.pdf

Gold CAN DS-301 Implementation Guide.pdf

Gold CAN DS‑402 Implementation Guide.pdf

## G-GOLDTWI.pdf

Elmo Application Studio II 调试步骤.pdf

## 驱动器配置软件：
https://www.elmomc.com/products/application-studio/download-resource-center/

### 三、改造和安装

### 3. 进行改造（底盘驱动器部分）

### 3.1.1 行走电机驱动器安装方式

## 1.驱动器需要与车体进行可靠固定，检查驱动器与对应电机的三相线（ 黄U 绿V 蓝W ）、编码器线路连接正确；
2.当机器人安装有多个驱动器(数量 ≥2)时，所有从站的 CAN_L,CAN_H 引脚直接相连即可，尽量采用串联方式接线，如图 3.1.1 所示；若驱动器仅提供一个通讯接口，无法完成驱动器的 can 线串联时，则将所有驱动器 can 线引出后把所有 can_H 压入同一德驰插筒连接器，将所有 can_L 压入同一德驰插筒连接器，接入德驰 DT06-2S 公头，最后与 TE28 中的 22、23 号线（SRC800V3 can2)相连,（ SRC800版本不同，can接口不同，需根据SRC800版本来进行接线 ）（ 由于本驱动器自带STO急停功能，但是此STO不满足急停去使能功能，所以不使用STO，需要将STO的两根线直接接到24V上，见图3.1.3 ）

由于部分驱动器没有提供级联接口，只能通过从总线上接引线的的方式来串联，这里引线的长度需小于 10CM.

注：在改造过程中若由于驱动器连接所需要的线束数量不足，无法实现在驱动器端进行快速串联，可采用图 3.1.2所示连接方式，但不推荐。

1677573114959-20e29b25-a207-423a-b90c-ef47270ba214.png

## 图 3.1.1  图3.1.2

1677573131500-0066f154-f076-4753-8a17-c82fd05e8bd2.png

## 图 3.1.3

为保证 can 通讯质量，需要在距离核心控制器最远的驱动器上或总线末端安装终端电阻（阻值一般是 120ohm).（注：客户可在购买驱动器时向驱动器厂商提出购买配套终端电阻）

### 3.CAN 终端电阻是否正确打开的检测方法：

关机断电，断开驱动器和控制器的 CAN 连接线（如图 4.4.1 中 Driver4 和控制器之间的位置），使用万用表电阻档测量驱动器侧的 CAN 总线上 CAN_L、CAN_H 之间电阻，电阻值为 120Ω 则正确，如图 4.4.3 所示。电阻值明显小于120Ω（如 60Ω),则说明至少有两个驱动器打开的终端电阻。

断开图 4.4.1 中 Driver1 和 Driver2 之间的连接线，使用万用表电阻档测量 Driver1 侧的 CAN 总线上 CAN_L、CAN_H 之间电阻，电阻值为 120Ω 则正确，如图 4.4.3 所示。如果电阻值明显大于 120Ω（如几 KΩ),则说明终端电阻打开的位置不在 CAN 总线末端，需要调整。

1677573159600-41da7e75-b4f8-4990-b64e-ee6985460453.png

## 图3.4.3
### 4.急停
将所有驱动器的 DI3(由于此驱动器急停状态是通过读取DI3状态，所以必须接DI3) 分别接入驱动 24V ；将所有驱动器 DIRENT 并联后连 SRC2000急停输1+ （TE354 号线），将另一根 急停输出1- （TE355 号线）连接 0V

1677573176194-864ac2ca-c66b-4b1f-a919-352492d8f617.png

### 四、驱动器参数配置

## 工具：

## USB-485/232 转换器

1677573192822-9dde1ab3-19f2-42b0-a594-e0cb65044405.png

## 1.使用 USB-485/232 转换器连接电脑和驱动器( 注意232TX与RX需要反接 ）

### 2. 连接调试软件
点击 System Configuration——Workspace——右键 Add Gold Dive
选择已添加的 Drive01——Connection Type——Direct Access RS232，选择已连接的 COM 口，
波特率 115200，Parity-None，点击左上角 Connect，系统读条后会显示已连接。

1679458376185-fdd14d90-d31b-429e-bf84-b398ba825e99.png

1677573228499-9487a793-9ed5-4ad5-9d91-79c872d8f6ad.png

### 3.参数配置
点击 Drive Setup and Motion —— Expert Tuning ，根据实际情况，在列表里逐一设置；
### ① Axis Configurations ：
Axis and Control Configurations——Single Axis 单轴驱动
Electro Mechanical Configuration——Rotary Motor Rotary Load+Gear 旋转电机旋转负载+减速 箱
### Total Gear Ratio：减速箱减速比设置 1
Feedback Configuration：Single Feedback 选择单反馈
Loop Feedback Configuration ： Rotary Feedback on Motor 反馈在电机侧
Mode of Operation：Velocity 速度模式
写入后点击右下角 Apply，设置到驱动器里，同时左上角的*会消失
1677573279757-bddd4980-7f76-4384-b7b8-7cc77d2d21e1.png

### 4. 根据电机手册填写
### Motor Settings 电机参数设置
Motor Type：Rotary Brushless（3 Phase） 三项无刷电机
### Peak Current：电机峰值电流（有效值）：21
Continuous Stall Current： 连续电流（有效值） ：5.7
Maximal Motor Speed： 电机最大转速 ：6000
Pole Pairs per Revolution：电机极对数（电机级数/2）：4
### R-resistance：电机项电阻 ：0.9946
### L-inductance：电机项电感 ：0.4973
Ke-back emf constant：电机反电动势 ：8.4
写入后点击右下角 Apply，设置到驱动器里，同时左上角的*会消失
1677573298299-10869aa8-5f72-4a0b-8c76-9523c9e1765e.png

### 5. Feedback Settings 反馈参数设置
## 在下拉菜单里选择电机的反馈类型
例如，增量 2500 线带霍尔编码器，接在 PortA 设置如下：
Sensor Name ： Encoder Quad,PortA
### Use Digital Halls:Yes
### Lines/revolution:2500
1677573321701-417d36db-5fc8-4431-9810-33afbf21745a.png

### 6.User Units 用 户单位，可以先不设置，用默认的 No Conversion
1677573341766-bdfda2a5-3764-4885-810a-1dede0a879ae.png

### 7.Limits and Protections
## Current Limits ：
## Drive PL[1]：28
## Drive CL[1]：7.5
Peak Current Duration PL[2] ：峰值电流的持续时间，单位秒，默认是 3
## 其余两个参数保持默认即可
1677573358285-5bc63a00-12d5-4d94-b87f-0874401e9112.png

### 8.Motion Limits and Modulo
### Stop Deceleration SD：499.998
### Max Velocity Command VH：6000
1677573378693-e7e44aa7-57c6-43f3-80f4-b5c6783b9715.png

### 9.Protections
Position Error WindowER[3]：1E+9
Position Error WindowER[2]：6E+5
### Current Limit CL[2]：100
### Velocity Limit CL[3]：6000
### Time Duration CL[4]：5000
1677573406930-0831946f-6456-4208-b21a-2e2b5b628776.png

### 10.Application Setting
## Setting Window
Target Position Winddow TR[1]：100
Target Velocity Winddow TR[3]：0.6
Target Position Winddow Time TR[2]：20
Target Velocity Winddow Time TR[4]：20
1677573423789-71131fa8-4f51-4aec-9026-db23a620132b.png

## Inputs and Outputs
### 将Input 3配置位Inhbit（急停去使能）
1677573444023-bb22d4e4-79eb-4b80-9ec6-1bd799cee7ea.png

### 11. Current 电流环整定 （ 此为电机自动整定过程，电机会有震动 ）
### Identification ：电机整定
Current Level ：整定电流，这里默认是 60% 额定电流，点击 identity ，开始整定。
### 整定完成后会绘制整定曲线，并测量出电机的电阻和电感
### 3 根曲线应基本重合，若区别较大，请检查电机接线及电磁干扰。
1677573459829-64289354-0c99-4cdd-927d-6a8199957820.png

1677573476201-3bc7753c-2746-4072-bba5-25942b9a0c92.png

## Design 设计电流环增益
### 点击 Design,可以用识别到的参数设计一套电流环参数
1677573493653-adf59150-00aa-4f9e-8307-59a0f7c2c87a.png

## VERIFICATION 校验
## 以下步骤务必在确认安全的情况下操作
验证电流环参数，模式选自动，点击 Verity ，在弹出的对话框点 yes
电机会震动，会有电流采用图出现，观察电流响应曲线，调整 KPKI 参数
1677573511804-24fe55c7-cbdb-41bb-b75d-96bc53d7a11f.png

如下图，KP 参数偏小，一点点加大 KP，一般可以按每次 10%-20%增加
曲线应尽量贴合（由于演示的电机电感小，这个曲线效果一般，解决方案：电机串电感）
1677573530576-26573d98-e62a-4193-8b21-f15efa26fb42.png

## Commutation 换向
## Auto-Phasing Method
1677573646000-dfa31a7b-b7cf-4187-ba67-69b8a0d9b019.png

1677573662988-c33b4085-9df6-4255-89b5-4893184819d8.png 

1677573683562-0993ef2c-bc67-426a-bb78-68f4af1ff950.png

Current Level 换向电流 建议值 60%-110%
Displacement 换向距离建议拉到最大，这样可以让电机运行尽量多的距离，确保每个角度都 没问题
点击 RUN Commutation，开始换向，电机会旋转，确认安全前提下点击 OK
1677573723724-e02643a1-fd44-46ed-8763-a9c144102bb5.png

换向运行中，可以看到右侧的电机位置在均匀变化，同时观察电机运行方向，选择正方向
OR 负方向，选择后电机会向相反方向再运行一次，已校验方向。
成功后会提示换向成功，电机 OK 。
## 若换向不成功：
## 1. 检测编码器反馈
### 2. 增大换向电流
1677573741784-4d2b627b-b299-4cff-83dd-fd398b1ff817.png

1677573757246-327c1522-be9d-44e7-b057-9735db6675d4.png

Velocity and Position 速度环与位置环设置
## Identification 识别电机
模式选择 Nomal ，电流可以选择默认的 100 ，点击识别。
对电机扫频会产生震动，务必要固定电机，确认安全。
弹出对话框点 OK ，电机会快速扫频，从低频到高频，测试电机的动态性能。电机会啸叫一
## 声，并绘制出电机的波特图
1677573773257-e2538c55-9bab-4af9-8d53-83f70f8e3144.png

1677573788319-c895063f-a19b-4036-ad21-e63878e358ea.png

## Design 设计 PKPI 参数
Objective 选择 Advanced Controller
### Scheduling 选择 Best Setting
### 建议添加低通滤波器，频率 800hz，Damp 0.6
1677573808817-1461b1ea-2a3e-43fb-b6b0-20ab4c8ae184.png

### 点击 Design，会自动绘制相域分析图
黄色线内为不稳定状态，蓝色线要在黄色线外。
点击 Margins and Performance，可以看到速度环带宽和位置带宽
1677573826037-18074b08-ce2b-496c-afe8-048015314884.png

## Resolution 采样频率
## Record 纪录时间
## Trigger 触发条件
### 可以修改合适的采样频率和纪录时间，以纪录运行曲线
1677573844127-f3c779ac-0b76-477c-aabb-0929addefe4f.png

### 12. 备份
点击 SAVE，生成一个离线文件，保存到电脑里，下次可以直接下载，无需调试
1677573977393-310cc9e7-fa54-4d09-89ff-159d4f157598.png

## 保存：
Drive Expert Tuner —— Drive Save ，保存到驱动器
注意这个保存和备份的区别；
这个保存是保存到驱动器，掉电不丢失，如果不做这个操作，断电所有参数恢复到上次保存的状态
1677573994003-59bcc550-cf54-405c-9159-344227db655a.png

### 五、机器人模型配置说明

根据电机及减速实际情况配置行走电机参数，品牌选择ELMO-CANOpen

注：这些参数需根据驱动器及选用电机及减速机的实际进行填写；

1677574016990-445f8658-9a1a-4f6d-a60f-ebffc76e5b81.png

1677574035093-c727043b-3266-4241-b967-4f39db6f8919.png

注：减速比、编码器线数、电机最大转速、驱动器品牌需要根据选用的实际填写。

### 六、驱动器的功能检测

## 1.在整车组装完成未安装外壳前，请再检查一遍接线确保接线正确。

2.将车体加高，使轮子离地。开启机器人，使用网线连接机器人。使用 Roboshop 软件操作机器人让轮子转起来。 使用 CanScope 夹在 CAN 总线上检测 CAN 报文至少 1 小时，CAN 报文无错误。

### 3.让车体着地，使用 Roboshop 软件操作机器人做运动动作:向前，向后，向左，向右运动。

4.未拍急停按钮前，推动机器人，无法推动（电机使能），检查 Roboshop 中机器人状态处于“未急停”“驱动器未急停”，如图 6.1 所示；所示拍下急停按钮后，再次推动机器人，可以推动（电机使能释放），检查 Roboshop 中机器人状态处于“已急停”，“驱动器已急停”，如图 6.2 所示。

5.测试驱动器与控制器掉通讯，使用Roboshop软件操作机器人向前运动，在运动过程中拔掉驱动器与控制器连接的canH canL，此时机器人停下，机器人无法推动（电机使能）。

1677574072297-a4ea0b26-e3fd-4ab7-9784-a0e274cabe67.png

## 图 6.1

1677574248663-3c205779-deec-4a93-b8d8-b8025bbe603a.png

## 图 6.2

### 5.任务链运动老化测试 24H，查看 Robokit Log 无错误报警。

### 七、附录

### 7.1 USB CAN 卡使用方法

## 1.软件安装—安装软件 USB_CAN Tool（软件及使用手册请联系 CAN卡 厂商售后）.

2.硬件连接—准备 USB CAN 卡和连接线，将连接线 CAN_H 接到 SRC2000 外接线束 TE35 33 号线上，将连接线 CAN_L 接到 SRC2000 外接线束 TE35 32 号线上。如图 7.1.1 所示：

1677574270836-cbb75a18-c927-4edc-a794-9f2aadd8e7c4.png

## 图 7.1.1

3.打开 USB CAN tool ,选择【设备操作（O）】【启动设备（S）】,确认 CAN 参数，【波特率】为 250Kbps,选择【CAN通道号】为通道1,点击【确认】。如图 7.1.2 所示：

1677574286853-99a8221e-dca8-4191-aa75-ce3412f1762f.png

## 图 7.1.2

### 4.选择【显示（V）】，取消选择【合并相同 ID 数据(M)】，CAN 报文如图 7.1.3 所示。

1677574309180-92e8d023-530b-484d-97ff-f746884afbcc.png 

## 图 7.1.3

### 7.2 udpconsole 使用方法

udpConsole 是我司工程师用于调试 bug 开发的小工具，可以检查到固件上报的错误信息。

## 1.打开 udpconsole 工具前需用网线确保电脑与机器人的物理连接。

### 2.打开 udpconsole 进行驱动器功能测试，时刻检查 udpconsole 显示内容。

### 驱动器通讯过程中出现错误帧如图 7.2.1 所示：

1677574327745-d46c43d9-9820-4321-a779-0e4adbfe42c9.png

## 图 7.2.1

### 八、驱动器常见错误码

绿色 LED 为电源指示灯，当驱动器接通电源时，该 LED 常亮；当驱动器切断电源时，该 LED 熄灭。红色 LED 为故障指示灯，当驱动器出现故障时，驱动器将停机，并提示相应故障代码。用户需软件做报警清除，故障才可为清除。
1677574345776-5c0ceecd-c65b-420e-9597-9e18464ad4c5.png

1677574359940-8fed3a5b-1b98-4936-98b0-f03a45ae114e.png

1677574384359-385d9c9a-0448-4fce-be63-b4f91fe183df.png

1677574403132-2fa1a6a4-201e-4a7f-9d81-d4883567f679.png

1677574419388-89e54f4f-d17e-4ce4-893e-d87fbb6ba094.png

1677574434401-2fb10ae2-7ef0-490a-8670-ba42228e9447.png

1677574449460-b03a5bf1-c5d0-41ce-ae17-65d516832cde.png

## 举例：此电机按位读取报错，例如
## 1.0x81报错，根据表格信息参考过压的错误去分析
### 2.0x8，为过载
### 3.8080，参考电压出错

### 九、 附录

### 9.1 驱动器接线图
1677574464567-6c56b035-163f-4c8d-9eb6-4d0e54efeba0.png
