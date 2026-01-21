# 清洁模块驱动器配置SOP

## 清洁模块驱动器配置SOP

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.29

## 更新发布

## 使用中

## 工作原理图
## image.png

## 序号

## 描述&名称

## 备注

## 1

## SRC2000核心控制器

## NA

## 2

## 清洁模块驱动器

## NA

## 3

## 清洁机构

## NA

## 4

SRC核心控制器通过Can穿透的方式下发清洁指令到清洁模块驱动器

## 清洗机构统一由清洁驱动器控制

## 5

## 清洗机构执行清洁驱动器下发的指令

## NA

## 6

### 清洁机构各机械状态及运行状态反馈给清洁驱动器

## NA

## 7

SRC2000核心控制器通过Can报文方式读取并解析清洁机构的各机械状态及运行状态

## NA
## 配置前的工具准备

## 序号

## 准备事项

## 图片

## 1

## 机器人上位机软件
## 下载链接：

## RoboshopPro.zip

## image.png

## 2

## Windows 10/11操作系统

## NA

## 3

## 以太网双绞线

32d073f0-07b0-4945-9105-baeef37398bd.jpg

## 4

### PC端已经与SRC控制器连接，可通过电脑连接机器人

## NA

## 5

### 确保机器人的清水箱存在清水，且机器人未出现关于“水量”报错

## NA
## 控制清洁机构
## 两种控制模式

## 工作模式一：
## 清扫模式

## 开始洗地

## 结束洗地

a. 刷盘升降电机控制推杆将两个刷盘放下；

a. 关闭喷水电机；

b. 水扒升降电机控制将水扒放下；

b. 关闭喷水球阀；

c. 吸风电机开始工作；

c. 行走电机继续行走5~10m距离；

d. 喷水电磁阀开启；

d.刷盘电机停止工作、刷盘上升、水扒上升、吸风电机关闭；

e. 喷水电机开始工作，将水喷入刷盘；

f. 刷盘电机开始工作；

## NA

g. 行走电机开始工作，机器人开始洗地工作；

## NA

## 工作模式二：
## 推尘模式

## 开始推尘

## 结束推尘

a. 水扒升降电机控制水扒放下；

a. 行走电机停止工作；

b. 行走电机开始工作；

b. 水扒升降电机控制水扒上升；
分别对应于脚本参数operation中的四种模式，["WashStart","WashEnd","DustStart", "DustEnd"]，其中：

## WashStart

## 开始洗地

## WashEnd

## 结束洗地

## DustStart

## 开始推尘

## DustEnd

## 结束推尘
洗地模式可以自设置电机功率，推尘模式下电机不工作。
## 1、设备支持功率控制，如风机、刷盘电机、喷水电机；
### 2、默认是标准功率；
### 3、推尘模式，只控制水扒升降。
## 如何使用脚本操作
### 打开Roboshop Pro上位机软件，可看到如下界面：
## image.png

## 1.点击【机构脚本】栏；
### 2.选择需要使用的脚本，文件后缀为.py格式，表示python文件；
### 3.在上一步中点击需要使用的脚本后，在此界面会弹出该脚本的详细内容，可在线编辑；
### 4.点击【开始】，选择执行脚本定义好的operation动作，弹出如下栏框：
## image.png

### 5.brush_power:刷盘电机的功率；
### 6.jet_power:喷水电机的功率；
7.operation：WashStart（执行洗地模式）/WashEnd（结束洗地模式）/DustStart（开始推尘模式）/DustEnd（结束推尘模式）/AddWater（加水任务）/CloseBallValve（关闭排污球阀）/CheckInfo（检查清洁模块的状态信息）；
### 8.suck_power:吸风电机的功率；
执行operation中的任意操作，无需对刷盘电机、喷水电机、吸风电机的功率进行设置
## image.png

## 在上述步骤4中跳出上图所示的弹框：
## 1.勾选operation并在下拉框选择需要执行的操作；
### 2.确认需要执行的动作无误后，点击确定；
### 查看机器人是否有动作，若机器人无响应可能与下列因素有关：
① 清洁驱动器的CAN通讯损坏，可使用接收CAN报文的上位机软件查看报文；
② SRC控制器与清洁驱动器之间的CAN通讯线损坏，可使用接收CAN报文的上位机软件查看报文；
## 详细脚本输入参数
执行脚本程序时，用户需要输入的参数，其中 operation 参数必须输入，其参数值可选 "WashStart", "WashEnd", "DustStart", "DustEnd", "AddWater" 其中一项。
## {
## "operation":{
        "value": "",
        "default_value":["WashStart", "WashEnd", "DustStart", "DustEnd", "AddWater"],
        "tips": "操作选项",
## "type": "complex"
    },
## "brush_power":{
        "value": 67,
        "tips": "刷盘电机功率, 取值: 0-100, 0即关闭，100即满功率, 参数可缺省",
## "type": "int"
    },
## "suck_power":{
        "value": 50,
        "tips": "吸风电机功率, 取值: 0-100, 0即关闭, 100即满功率, 参数可缺省",
## "type": "int"
    },
## "jet_power":{
        "value": 20,
        "tips": "喷水电机功率, 取值: 0-100, 0即关闭, 100即满功率, 参数可缺省",
## "type": "int"
## }
## }
## 脚本参数配置文件
### 机构脚本文件： clean_robot_2000.py

## clean_robot_2000.py
脚本参数配置文件：clean_robot_2000.json

### clean_robot_2000.json

clean_robot_2000.json 是 clean_robot_2000.py 运行后自动生成的脚本参数配置文件，该文件主要是为用户提供参数修改便利，其中 default 是参数参考值，value 是参数实际生效值，内容如下：
## {
### "min_clean_water_level": {
        "value": 1.0,
        "default": 1.0,
### "comment": "清水液位最小值"
    },
### "max_clean_water_level": {
        "value": 99.0,
        "default": 99.0,
### "comment": "清水液位最大值"
    },
### "min_waste_water_level": {
        "value": 1.0,
        "default": 1.0,
### "comment": "污水液位最小值"
    },
### "max_waste_water_level": {
        "value": 90.0,
        "default": 90.0,
### "comment": "污水液位最大值"
    },
## "add_water_do": {
        "value": 4,
        "default": 4,
## "comment": "加水DO"
    },
## "brush_power": {
        "value": 67,
        "default": 67,
### "comment": "刷盘电机默认功率"
    },
## "suck_power": {
        "value": 50,
        "default": 50,
### "comment": "吸风电机默认功率"
    },
## "jet_power": {
        "value": 20,
        "default": 20,
### "comment": "喷水泵电机默认功率"
    },
### "auto_adjust_power": {
        "value": 1,
        "default": 1,
        "comment": "是否启动电机功率自动调节模式, 1: 启动， 0: 不启动"
    },
### "add_water_delay_time": {
        "value": 5.0,
        "default": 5.0,
### "comment": "加水延时关闭时间"
    },
### "high_mode_x_speed": {
        "value": 0.8,
        "default": 0.8,
        "comment": "x速度大于该值时, 清洁机构以高功率工作"
    },
### "std_mode_x_speed": {
        "value": 0.4,
        "default": 0.4,
        "comment": "x速度大于该值时, 清洁机构以标准功率工作"
    },
## "stop_x_speed": {
        "value": 0.05,
        "default": 0.05,
        "comment": "x速度大于该值时, 清洁机构以低功率工作, x速度小于该值时, 清洁机构停止工作"
## }
## }
## 脚本数据上报
清洁机器人脚本运行过程中，会实时上报运行数据，运行数据会上报到控制器数据中 move_status_info 字段中，可以通过 Robokit API 1100 接口查询获取，解析 move_status_info 字段即可。 运行数据还会打印在 Roboshop 界面上，可以通过 Roboshop 以下界面实时查看:
## image.png

## 上报数据字段解释：

## 字段

## 释义

## 备注

## args

## 执行脚本时输入的参数

## operation

## 当前正执行的任务操作

## 如 WashStart、WashEnd

## connected

## 与RBK的连接信号

## cleanRobot

## 清洁机器人的水位数据和清洁区域信息

## cleanWaterLevel

## 清水液位

## 0 - 100

## wasteWaterLevel

## 污水液位

## 0 - 100

## currentCleanArea

## 当前清洁区域编号

## brush_power

## 刷盘电机当前功率

## 0-100

## suck_power

## 吸风电机当前功率

## 0-100

## jet_power

## 喷水电机当前功率

## 0-100

## periodRunCounter

## periodRun 运行次数计数

当 RBK 程序启动后，periodRun 就会持续开始运行，但每次执行脚本任务，该计数会重置为0 

## query_all_info

## 一键查询的回复报文数据

## scriptStatus

## 脚本当前运行的实时状态

## 1：运行中
## 3: 已完成
## 4: 失败
## 5: 暂停
### 当运行脚本时被异常中断，该状态可能显示异常

## taskStatus

## 当前的导航任务状态

## 2：运行中
## 3：暂停
## 4： 完成
## 5：失败
## 6：取消

## time

## 当前系统时间

由于periodRun是开机后一直运行的，所以该时间也是持续计时的

## work_mode

## 当前清洁工作模式

## High：高功率运行
## STD：标准功率运行
## Low：低功率运行
## Stop：停止运行

## work_status

## 各个清洁机构的实时状态

## 1： 运行中
## 0： 已停止

## brush_lift_state

## 刷盘推杆工作状态

## 1： 运行中
## 0： 已停止

## brush_state

## 刷盘电机工作状态

## 1： 运行中
## 0： 已停止

## mop_lift_state

## 水扒推杆工作状态

## 1： 运行中
## 0： 已停止

## suck_state

## 吸风电机工作状态

## 1： 运行中
## 0： 已停止

## jet_pump_state

## 喷水电机工作状态

## 1： 运行中
## 0： 已停止

## clean_valve_state

## 清水阀开关状态

## 1： 开
## 0： 关

## waste_valve_state

## 污水阀开关状态

## 1： 开
## 0： 关

## agvSpeed

## 清洁机器人实时速度

## x

## X 实时速度

## m/s

## y

## Y 实时速度

## m/s

## rotate

## 转向实时速度

## rad/m
## 验证清洁模块
## 清扫模式
## {
  "operation": "Script",
## "script_args":{
    "operation": "WashStart",
  },
  "script_name": "clean_robot_2000.py"
## }

## image.png

在 operation 中勾选并下拉选择 WashStart ；
确认选择无误后点击确定；
WashStart表示清洁机器人的清扫模式，执行该模式后，机器人的刷盘升降电机、水扒升降电机、喷水电机、输盘旋转电机开始工作；可在下拉框选择 WashEnd结束清扫模式，清扫模式结束后，所有电机停止工作，所有清洗机构复位；
## 观察机器人是否与描述动作一致
## 清扫任务
## {
  "operation": "Script",
## "script_args":{
    "operation": "WashStart",
  },
  "script_name": "clean_robot_2000.py"
## }
## 结束清扫
## {
  "operation": "Script",
## "script_args":{
    "operation": "WashEnd",
  },
  "script_name": "clean_robot_2000.py"
## }
## 推尘模式
## 开始推尘
只降下水扒机构，其他机构不工作。
## {
  "operation": "Script",
## "script_args":{
    "operation": "DustStart",
  },
  "script_name": "clean_robot_2000.py"
## }
## image.png

在 operation 中勾选并下拉选择 DustStart ；
确认选择无误后点击确定；
DustStart表示清洁机器人的推尘模式，执行该模式后，机器人的水扒开始降下，可在下拉框选择 DustEnd结束推尘模式，推尘模式结束后，水扒升起；
## 结束推尘
## {
  "operation": "Script",
## "script_args":{
    "operation": "DustEnd",
  },
  "script_name": "clean_robot_2000.py"
## }
## 观察机器人是否与描述动作一致
## 加清水/排污水
### 进行此操作时需在机器人充电的情况下进行（CP点）
## {
  "operation": "Script",
## "script_args":{
### "operation": "AddWater"
  },
  "script_name": "clean_robot_2000.py"
## }

## image.png

在 operation 中勾选并下拉选择 AddWater ；
确认选择无误后点击确定；
AddWater 表示机器人执行加清水的动作任务，该模式需在充电站点进行，执行加水任务时，机器人会自动打开 DO1 ，此时工作站会自动往机器人的清水箱中加入清水，达到100%阈值时停止加水，在加水过程中同时检查污水箱中的污水阈值，符合排污需求后将污水箱中的污水排出车体外；
## 观察机器人是否与描述动作一致
上述operation中的动作（建议operation中的动作都执行一遍），若功能可正常执行，则代表清洁模块无任何问题。可正常执行清洁任务；
## 液位检测
### 当清污水液位达到所设定的阈值时，清洁机构将自动停止工作
在机构脚本的json文件中（json文件是执行脚本之后的自动生成的文件，在 params 文件夹下）
## image.png

清水液位的最小值，低于该值时，机器人触发告警并停止工作（可根据实际需求进行设置）；
污水液位的最大值，高于该值时，机器人触发告警并停止工作（可根据实际需求进行设置）；
