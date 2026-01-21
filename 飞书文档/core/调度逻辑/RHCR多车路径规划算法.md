# RHCR多车路径规划算法

## RHCR多车路径规划算法
## @杨达
除G-MAPF外，Core提供另一种多车路径规划算法：RHCR
## 使用方法
### 需要使用版本号前缀为0.2.0的Core！
在参数配置中，先将G-MAPF改为False；此处需要注意，机器人执行任务时，G-MAPF无法关闭，需要等到任务执行完毕，G-MAPF才会“释放”机器人，所有机器人都释放后，报警码54006消失，G-MAPF才算成功关闭
在参数配置中， 将 RHCR 改为True  240902后默认开启
## 参数相关问题，需要咨询调度团队
## 参数列表
### 下列参数中，标记为开发中的，不推荐在现场开启

## 参数名称

## 类型

## 描述

## 状态

## RHCR

## bool

## RHCR总开关

## 已完成

## RHCRAlgorithm

## int

## 算法编号，目前仅支持1

## 已完成

## RHCRInterval

## int

## 两次规划之间的时间间隔，单位秒

## 已完成

## RHCRHWindow

## int

## H窗口

## 已完成

## RHCRGWindow

## int

## G窗口

## 已完成

## RHCRMaxNodeCount

## int

## 最大节点数量限制

## 已完成

## RHCRVehicleSpeed

## double

## 估计时使用的车辆速度

## 已完成

## RHCRSendDist

## double

## 的发送距离，单位米

## 已完成

## RHCRInitialWaitTime

## int

## 的初始等待时间，单位毫秒

## 已完成

## RHCRUseADG

## bool

## 使用ADG后处理

## 开发中

## RHCRHuTongMode

## int

## 胡同模式

## 已完成

### RHCRUseUnreachableSite

## bool

## 是否使用不可达点

## 已完成

## RHCREnableDetour

## bool

## 绕路开关

## 已完成

## RHCRAsync

## bool

## 异步规划

## 开发中

## RHCRKeepAwayDist

## double

## 识别任务时，两车的最小距离

## 已完成
## ！！！2024-08-10之前的版本
## 使用RHCR时，必须确保：
每个机器人组的所有机器人，底盘尺寸一致，参考【roboshop】将机器人模型文件同步到场景
### 参数中，将DisableGoodsShape改为True
### 参数中，将RobotRuntimeShape改为False
