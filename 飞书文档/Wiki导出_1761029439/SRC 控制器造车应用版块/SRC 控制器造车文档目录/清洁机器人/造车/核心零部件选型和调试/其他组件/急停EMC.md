# 急停EMC

## 急停EMC
## 更新记录：

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.29

## 更新发布

## 使用中

## 模型文件配置

进入EMC；
启用设备；
勾选急停触发及解除后的响应：恢复任务、取消任务；
选择急停后驱动器处理方式：协议急停、停机并下使能（失能）、poweroff等。
1711025687243-f0880215-cc97-4ec2-8507-1f16028d82e3.png

## 参数说明

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## recycleControl

## 模型文件- EMC-basic

## —

## false

## —

## —

## ~latest

是否回收控制权: 勾选该项时，当拍下急停按钮时，将从调度系统回收控制权；若不勾选该项，则不会实现该功能。（该功能需要配合我司开发的调度系统使用） 

### powerOffWhenCANError

## 模型文件- EMC-basic

## —

## false

## —

## —

## ~latest

是否通信断线时驱动器断电（继电器断开): 当驱动器 can 通信断开时，可用来断开驱动器供电（需要使用 SRC2000 控制器继电器）；若不勾选该项，则不会实现该功能。

## resumeTask

## 模型文件- EMC-basic

## —

## false

## —

## —

## ~latest

是否当急停拔起时，恢复当前任务 / 导航: 当急停拔起时会继续机器人当前的任务 / 导航；若不勾选该项，则急停时机器人会暂停当前任务 / 导航 

## cancelTask

## 模型文件- EMC-basic

## —

## false

## —

## —

## ~latest

是否当急停时取消当前任务 / 导航: 当急停时会取消机器人当前的任务 / 导航；若不勾选该项，则急停时机器人会暂停当前任务 / 导航 

## func

## 模型文件- EMC-func

## —

## —

## driver IO EMC
## stop and enable
## stop and power off
## driver protocol EMC
## onOpenTime

## ~latest

## driver IO EMC
## 释义: IO急停
### 说明: 通过驱动器的 IO 信号进行急停，电机处于失能状态
## stop and enable
## 释义: 停下且保持使能
### 说明: AGV 停止运动且电机保持使能状态
## stop and power off
## 释义: 停下并且驱动器掉电
### 说明: AGV 停止运动且采用将驱动器断电的方式进行急停
## driver protocol EMC
## 释义: 协议失能
说明: 通过协议进行失能（Trigger配置enableMotor或disableMotor时勿使用此功能） 
## onOpenTime
## 释义: 驱动器掉电
## 说明: 采用驱动器断电的方式进行急停

## 推荐配置
## 控制权设置
### 企业微信截图_17137498853272.png

该功能的逻辑关系为通过控制器中的某一个DI信号触发，使机器人做出相应的动作；
在【模型文件】中，点击 Trigger 功能；
并在 Trigger 中配置相应的需求；

## Key

## Description

## Value

## source

## 可通过什么方式进行触发

## EMC

## type

## 触发类型/条件

## risingEdge

## func

## 触发后做出相应的动作

## releaseControl
## 参数详解

## Key

## Value

## Notes

## source

## DI

## 通过控制器中的DI信号触发

## DO

## 通过控制器中的DO信号触发

## EMC

## 通过急停触发

## externalControl

## 通过外部控制触发

## manipolatorConnect

## 手持器连接触发

## motorInfo

## 电机信息触发

## errcode

## 通过控制器中的报错码触发

## type

## risingEdge

## 上升沿触发

## fallingEdge

## 下降沿触发

## func

## suspendTask

## 暂停任务

## resumeTask

## 继续任务

## cancelTask

## 取消任务

## releaseControl

## 释放控制权

## recycleControl

## 抢占控制权

## powerOffDriver

## 驱动器断电

## powerOnDriver

## 驱动器上电

## openDO

## 打开控制器中的DO信号

## closeDO

## 关闭控制器中的DI信号

## navTotarget

## 导航到某个站点

## runScript

## 执行脚本
