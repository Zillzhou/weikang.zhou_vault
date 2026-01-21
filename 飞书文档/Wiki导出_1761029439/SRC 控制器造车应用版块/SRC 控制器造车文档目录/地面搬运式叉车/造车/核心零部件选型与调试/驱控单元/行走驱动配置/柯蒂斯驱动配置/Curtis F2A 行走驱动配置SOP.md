# Curtis F2A 行走驱动配置SOP

### Curtis F2A 行走驱动配置SOP

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.29

## 更新发布

## 使用中

柯蒂斯软件由柯蒂斯给叉车厂家定制，因此如需单独购买驱动器电机，对应的柯蒂斯程序需要找对应的销售联系，刷好后发出。下面介绍F2A相关的参数配置。驱动器配置可参考Curtis驱动器F2A参数调整指导文档
## 调试资源
## 调试软件：
1662041960426-0d0b4799-1a5b-4f9d-b480-9246efd24124.png

1662041984110-b2d1ba65-a157-484f-b0b3-bc6393935b3d.png

【此软件授权码只能在一台电脑上安装使用，如果在别的电脑上面使用的话，需要重新授权，且授权激活码是收费的】
## PEAK 的软件（此软件不需要授权）
## 软件名称： PcanView.exe
PEAK驱动和软件的用法，可以直接去PEAK官网上面下载或者使用下面的压缩包中包含的驱动和软件。

## PEAK软件.rar
## 调试工具：
## PEAK 数据线接
1662042279908-cf868aea-a5ab-4e6a-b7af-43828b460bbc.png

## 接线与修改参数步骤
### 2.1. 电气接线
按照实际的项目的电机原理图进行接线。
### 2.2. 修改参数步骤
## 连接PEAK
将PEAK连接到F2A驱动器上面，注意区分 CANH 、 CANL 的定义。
## 打开 PcanViewe.exe 软件
配置波特率为 250K ；
1662089953184-f9b32318-f6d4-4877-a544-85ab49bf26b7.png

点击 OK ，就可以在 PCAN-view 上面读到数据；
### 打开 Curtis Integrated ToolKit
需要先加载 CurtisF2A 对应的固件程序，才能完整打开 F2A 内部的参数；
1662090134607-d510615b-1bfa-4716-99bc-99309cf66d1f.png

选择 Communication ，点击 Connect ；
选择 Node 0x26 等待设备加载完成；
点击 Programmer ；
1662090294437-cf85aae9-b89b-474c-aae0-76bb515ae7b5.png

### 2.3. 注意事项
## 注意：
在连接PEAK的时候，一定要检查 CANH 、 CANL 和 F2A 的 CANH 、 CANL 连接是否正确。
对于科蒂斯的软件 Curtis Integrated ToolKit 公司只有一台电脑上面有授权，其他工程师使用其他电脑调试需要购买软件授权。
## 驱动器配置
### 3.1.  F2A驱动器工作模式
我们使用 Curtis-F2A 系列的驱动器，要使用 F2A 的 速度模式 。
## 注意：
F2A 的位置模式有一个问题，在停止的时候会出现行走的保持电流，一方面会造成行走数据反馈异常，另一方面会造成电机老化过快，出现电机异常。
## 如果使用伺服模式可能会遇到的一些问题：
负载起升货叉的时候（车辆行走静止状态），行走电机上面有异常速度反馈，造成报警 " out of path "；严重的会造成丢定位的现象，在放货过程中出现危险；
行走电机有保持电流存在，如果多次出现刚启动的时候被阻挡或者急停，会造成F2A电流无法衰减，出现F2A驱动器报错。
### 3.2.  需要修改确认的F2A内部参数
## Speed Mode Express
KP 设置为 30% ，如果行走的轮子不出现抖动的话，可以适当调整到 40%；
KI 设置为 40%；
Brake Rate 设置为 0.1sec；
1662043888218-51b6f1b0-aeac-4da9-b9cd-8c73405d8723.png

Speed Mode -- Response -- fine turning
Max speed Accel 设置为 0.1 sec；
Max Speed Decel 设置为 0.1 sec；
1662044464037-00443f28-38d2-411a-bf7f-6ae244e02cab.png

### Restraint -- Position Hold
Soft Stop speed 设置为 0 rpm；
Position Hold Enable 置成 ON ；
KP 设置为 50% ；
Kd 设置为15% ；
1662044668126-6d9db444-dcb2-488c-99d1-6aa6d3726c5b.png

Application Setup -- EM Brake Control
Zero Speed Threshold 设置为 70rpm ；
Brake set Time 设置为 304ms ；
Torque Release Time 设置为 200ms -- 超调效果差的时候，可以设置为 100ms，甚至 50ms ；
Brake Release Time 设置为 48ms ；
1662044818259-4cc8e66d-6b0e-4d4c-9a35-c5feb6b9ba1b.png

## 下降比例阀相关的参数
Driver_1_Ki 设置为 2.5% ；
Driver_1_Kp 设置为 25% ；
Driver_1_Max_Current 设置为 0.8 ；
Driver_1_Min_Current 设置为 0 ；
1662194297419-2a5fcca5-c03c-42b2-98b1-23c3341eed61.png

### 3.3. Roboshop中运动学参数修改说明

原地旋转超调问题 -- 参数修改确认。
### 在 Roboshop 参数配置中检查下面对应的参数：

## 参数名称

## 建议值

## 默认参数

## 备注

## jerkRot

## 4

## 10

### Load_MaxManualRotAcc

## 15

## 30

## 保持默认值

### Load_MaxManualRotDec

## 15

## 60

## 保持默认值

## Load_maxRot

## 60

## 60

## 保持默认值

## Load_maxRotAcc

## 15

## 15

## 保持默认值

## Load_maxRotDec

## 15

## 15

## 保持默认值

## MaxRot

## 60

## 60

## 保持默认值

## MaxRotAcc

## 30

## 30

## 保持默认值

## MaxRot Dec

## 30

## 30

## 保持默认值
## 行走驱控模型文件配置
点击①弹出，右侧弹出电机模型配置界面，第一步先配行走驱动相关模型，确认func为walk。
1709804063534-2cc8ffb7-c8a3-4f59-a909-b83fc5f8a5b4.png

### 4.1. CAN通讯设置
### 在 canPort 启用控制器上的两路can
1709806037496-ec05a055-5ba9-4c47-b4c2-ac387588e33b.png

在行走电机 motor 模型文件上根据电气接线配置对应的canport，canID，cbd15在第一路can，且Curtis设置的canID为1。
1709806624919-b485f48b-d2a9-4546-8574-701b632f263c.png

## 驱动器品牌根据实际情况勾选，如下图
1709806766285-0ada2077-5a60-4213-8fb6-4c7c29dedc7b.png

### 4.2. 电机位置与机械相关
### x: 右手坐标系下的 x 位置，如小车模型中坐标
### y: 右手坐标系下的 y 位置，如小车模型中坐标
### z: 右手坐标系下的 z 位置，如小车模型中坐标
### yaw: 右手坐标系下朝向角，右手坐标系下的 x 位置
### maxRPM: 是指电机的最大转速，请根据实际情况进行填写
inverse: 驱动器是否反向，勾选则为反向，不勾选则为正向。请根据实际情况进行勾选
### wheelRadius: 请根据实·际情况进行填写
### reductionRatio: 请根据实际情况进行填写
### minimumspeed: 请根据实际情况进行填写
### encoderLine： 请根据行走电机实际情况进行填写
1709805537937-b912e20f-e5d5-4dc9-ac46-881b1c753218.png 

1709805566014-6943d3e1-b49a-42f2-a4ec-a5a2259fd91a.png 

1709805576833-7b33f32d-bed1-489a-91d7-a7211f54662e.png

行走电机没有零位上下限位、外置编码器等参数，填"-1"不启用
## 验证
### 5.1. 推送保持模型文件，没有motor超时错误
### 5.2. 控制车体前进、后退、左转、右转，车体按预期执行
### 5.3. 下发里程距离平动3m，误差在3cm左右，没有明显偏差
