# 电梯（lift)

## 电梯（lift)
## @杨达
## 流程图

1677676284874-7a27fa03-e100-4dae-bf1c-d1352c340ecd.png

## 配置项

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## door

## 场景-电梯-属性

## bool

## false

## -

## 电梯是否有门

## simulate

## 场景-电梯-属性

## uint16_t

## false

## -

## 是否是仿真电梯

## areaFloorTable

## 场景-电梯-属性

## json object

### {"1F":"1", "2F":"2"}

## -

## 电梯楼层表

## commandPeriod

## 场景-电梯-属性

## s

## 1.0

## 1.0

### 10.0

## 发指令的时间间隔

1677676301573-1078297d-7f5b-449b-8635-921320c80eae.png

## 电梯门

如果电梯配置中的 door 项为 false ，core不会控制电梯门
## 自动关门
用于解决保持电梯门常闭的需求。

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## KeepLiftDoorClosed

## 参数配置-导航

## -

## false

## -

## -

## ~latest

在没有机器人使用电梯时，是否发送关门指令。

需要保持常闭时，将KeepLiftDoorClosed改为true，core会在没有机器人用到电梯时，自动关门。
## 先关门再呼叫
机器人想要进电梯，但是电梯在其他楼层时，先关门再呼叫的需求。

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

### CloseLiftDoorOtherFloor

## 参数配置-导航

## -

## true

## -

## -

### 0.1.9

### 机器人在其他楼层且门打开了，是否先关门再呼叫到目标楼层

## 楼层号和区域名称的对应

## 示例：

### {"AREA1":"1","AREA2":"2"}

## 格式：
JSON对象，键为区域名称，值为楼层号，楼层号用字符串表示，即，1楼的楼层号为 "1"
实例中的配置，意为：调度地图中的区域 AREA1 对应电梯上的楼层 1 ， AREA2 对应电梯上的楼层 2 

## 注意：
### 每部电梯的楼层区域对应可以不同，需要分别配置

## 类型
## 目前支持下列几种电梯控制协议：
## 金博
## ModbusTCP
## 旺龙
## 新的ModbusTCP
## 倍加信
## 金博电梯配置

## 电梯通讯协议（新）_v2.pdf
type 字段选择 jinBo ，表示电梯使用金博梯控协议。模型文件中，对应的proxy类型为ModbusTCP。
## 配置项：

## 字段

## 类型

## 描述

## proxyName

## 字符串

## 代理名称

1677676320468-e3d990c8-b77b-4715-acdf-cf44144fd922.png

## 旺龙电梯配置

## 云电梯对外通讯协议接口V1.6.pdf
type 字段选择 WangLong ，表示电梯使用旺龙梯控协议。模型文件中，对应的proxy类型为ModbusTCP。
## 配置项：

## 字段

## 类型

## 描述

## proxyName

## 字符串

## 代理名称
1677828304616-d02d6a5a-35c9-4fc6-bc10-a6c551a9e10a.png

## ModbusTCP电梯配置
## <0.1.8.230216
type 字段选择 proxy ，表示电梯使用ModbusTCP梯控协议

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## liftCurrFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 读取电梯当前楼层的地址位

## liftDoorStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 读取电梯门状态的地址位

## liftStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 读取电梯运行状态的地址位

## liftTargetFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 写入电梯目标楼层的地址位

## liftUrgentStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 未启用参数

## proxyName

## 场景-设备-proxy

## string

## proxyOne

## -

## -

## 代理名称

## modbus配置项：
## JSON对象，有两个字段：

## 字段

## 类型

## 描述

## address

## uint16_t

## modbus地址位，无前缀

## addressType

## uint

### modbus函数号，表示读/写操作时的函数号
1677676336800-fda855db-b975-4d9a-b902-0987b864407a.png

## Modbus梯控配置的补充说明：
## 为了兼容老项目：
电梯的状态需要写在七个连续的寄存器中。Core读取时会使用 liftCurrFloor 中的地址为起始值，使用 liftCurrFloor 中的函数号，支持03和04。
将 liftCurrFloor 中的 address 值记为X，则电梯各状态应按下图写入PLC：
1677676359136-f9f87a4b-aabc-41c5-af4b-3c3eb0308c3b.png

例如， liftCurrFloor 配置了 {"address":2, "addressType":3} ，对应的读取方式如下图：
1677676373170-4bf9ba64-ba05-4885-9a2f-8ee845369aaa.png

如果电梯有3层，分别用1，2，3表示。那么，梯控应当将楼层号写入楼层低16位对应的寄存器，如果电梯当前在2楼，则将地址2对应的寄存器置为2
调度读取到电梯状态后，会按照配置的楼层表判断数字对应的楼层，因为兼容性问题，楼层表中的楼层需要用字符串配置。例如，楼层表配置了 {"Area1":"1", "Area2":"2", "Area3":"3"} ，表示，调度场景中的区域 Area1 对应楼层号 1 ，从梯控中读取到楼层为 1 时，调度判断电梯在 Area1
运行状态表示电梯是否到位， 0 表示电梯正在运行； 1 表示电梯已经到楼层，正在开门； 2 表示电梯已经到楼层并且门已经打开
门状态为 4 时表示门已经打开，其余状态，调度认为门已经关闭
## 梯控在电梯到了目标楼层后，需要自动开门
## 新版本的ModbusTCP电梯配置
## >0.1.8.230216
type 字段选择 proxyNew ，表示电梯使用新的ModbusTCP梯控协议

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## liftCurrFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## Core读取电梯当前楼层的地址位

## liftDoorStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## Core读取电梯门状态的地址位

## liftStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## Core读取电梯运行状态的地址位

## liftTargetFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## Core写入电梯目标楼层的地址位

## proxyName

## 场景-设备-proxy

## string

## proxyOne

## -

## -

## 代理名称

## modbus配置项：
## JSON对象，有三个字段：

## 字段

## 类型

## 描述

## address

## uint16_t

## modbus地址位，无前缀

## addressType

## uint

### modbus函数号，表示读/写操作时的函数号

## value

## uint16_t

## 期望的读取值
对于liftDoorStatus，value表示门开时的读取值。如，liftDoorStatus配置为 {"address":4, "addressType":4, "value":1} 时，core读取到地址4的值为1时，认为电梯门已开
对于liftStatus，value表示电梯上/下行完成时的读取值。如，liftStatus配置为 {"address":4, "addressType":4, "value":2"} 时，core读取到地址4的值为2时，认为电梯已经到了目标楼层
## ModbusTCP电梯3
type 字段选择 proxy3 ，表示电梯使用带关门指令的新版ModbusTCP梯控协议

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## liftCurrFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 读取电梯当前楼层的地址位

## liftDoorStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 读取电梯门状态的地址位

## liftStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 电梯运行状态

## liftTargetFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 写入电梯目标楼层的地址位

## proxyName

## 场景-设备-proxy

## string

## proxyOne

## -

## -

## 代理名称

## liftCloseDoor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 写入电梯关门指令的地址位

## modbus配置项：
## JSON对象，有三个字段：

## 字段

## 类型

## 描述

## address

## uint16_t

## modbus地址位，无前缀

## addressType

## uint

### modbus函数号，表示读/写操作时的函数号

## value

## uint16_t

## 期望的读取值
对于liftDoorStatus，value表示门开时的读取值。如，liftDoorStatus配置为 {"address":4, "addressType":4, "value":1} 时，core读取到地址4的值为1时，认为电梯门已开
对于liftStatus，value表示电梯上/下行完成时的读取值。如，liftStatus配置为 {"address":4, "addressType":4, "value":2"} 时，core读取到地址4的值为2时，认为电梯已经到了目标楼层
## proxy4
## 项目定制电梯
https://seer-group.coding.net/p/order_issue_pool/requirements/issues/5428/detail

### 电梯状态寄存器描述-20240131174432.docx
## 倍加信
## >0.1.9.231206

门禁梯控开发文档 网络协议 V2.0 2023.07.30.docx

## 梯控调试工具.zip
type 字段选择 UDP，表示电梯使用倍加信梯控协议。模型文件中，对应的proxy类型为UDP。
## 配置项：

## 字段

## 类型

## 描述

## proxyName

## 字符串

## 代理名称
1699323572677-38b0a273-cffe-4008-87bf-17e26bd63c33.png

## 楼层指令的配置
由于倍加信梯控模块中，读取当前楼层和发指令去目标楼层时，同一楼层对应的数字不同，导致需要单独配置楼层指令。例如，读取楼层时，读到2表示在 area-2，但是，如果想要呼叫电梯到area-2，需要发送的指令是3
配置项中的 areaCommandTable用来配置楼层指令，它是一个json数组，每个元素有两个成员：

## 字段名

## 类型

## 说明

## area

## string

## 区域名称

## cmd

## int

## 楼层指令
## [
    {"area":"area-1", "cmd":2},
    {"area":"area-2", "cmd":3},
## ]

上面的配置表示，去area-1时发送指令2，去area-2时发送指令3
## 上世物流电梯
## >0.1.9.240510
https://seer-group.coding.net/p/order_issue_pool/requirements/issues/5838/detail

MCTC-KZ-B0S通信协议-开放协议V1.7-20240324215658.pdf

type字段选择 proxyNewExtend，基于modbusTCP适配上世物流的场景，增加了新的字段agvStatus和agvControl

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## liftCurrFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 读取电梯当前楼层的地址位

## liftDoorStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 读取电梯门状态的地址位

## liftStatus

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 电梯运行状态

## liftTargetFloor

## 场景-电梯-proxy

## Modbus配置项

## -

## -

## -

## 写入电梯目标楼层的地址位

## agvStatus

## 场景-电梯-proxy

## Modbus配置项

## 读取agv运行状态

## agvControl

## 场景-电梯-proxy

## Modbus配置项

## 写入agv运行状态

## proxyName

## 场景-设备-proxy

## string

## proxyOne

## -

## -

## 代理名称

## modbus配置项：
## JSON对象，有三个字段：

## 字段

## 类型

## 描述

## address

## uint16_t

## modbus地址位，无前缀

## addressType

## uint

### modbus函数号，表示读/写操作时的函数号

## value

## uint16_t

## 期望的读取值
对于liftDoorStatus，value表示门开时的读取值。如，liftDoorStatus配置为 {"address":4, "addressType":4, "value":4} 时，core读取到地址4的值为4时，认为电梯门已开。
对于liftStatus，value表示电梯上/下行完成时的读取值。如，liftStatus配置为 {"address":4, "addressType":4, "value":0"} 时，core读取到地址4的值为0时，认为电梯已经到了目标楼层。
对于agvControl，value表示请求进入AGV状态，如agvControl配置为{"address":100,
"addressType":16,"value":1} 时，core读取到地址100的值为1时，认为申请进入AGV状态。
对于agvStatus，value表示当前AGV状态，0：正常（可以申请进入AGV状态），1：等待进入AGV状态，2：AGV运行状态。如agvStatus配置为{"address":101,"addressType":4,"value":0}时，core读取到地址101的值为0时，认为AGV正常可以申请进入AGV状态。
## 中集-通威项目定制电梯
和木牛流马的系统对接，类型选择 HTTP 接口说明和字段见文档：

仙工清洁机器人与木牛接口20230816v3-20230817135855.xls
## 电梯互斥组的设置
不同smap中，同一电梯对应的点，以及点前面的那条线，还有前置点，应在互斥组中，以保证不会出现争抢电梯的情况。如下图所示。
1677676389467-4246a66b-1081-4843-9571-f1c61546ae5e.png

机器人可以在还没到达电梯前，提前呼叫电梯。这样能提高效率。

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## OpenLiftDist

## 参数配置-导航

## m

## 0

## 0

### 100.0

机器人提前呼叫电梯的距离，此距离需要小于互斥组内距离SM点的最远距离。如果OpenLiftDist为0，则机器人到达前置点后，再呼叫电梯。

## 自动开门超时报错
## >0.1.9.20240513

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## LiftOpenDoorTimeout

## 参数配置-其他

## s

## 0

## 0

### 1000.0

电梯到目标楼层后，自动开门的超时时间。0表示不考虑超时，大于0时，如果电梯没有在设置的超时时间之内自动开门，会报错 50319
