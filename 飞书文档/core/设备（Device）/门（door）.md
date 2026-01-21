# 门（door）

## 门（door）
## @杨达
## 控制时序
1656040744849-86209cc4-04e4-4f0d-9aba-9ba3adb4736d.png

## 配置

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## triggerType

## 自动门-属性

## 下拉框

## level

## edge

## ~latest

## 开关门指令发送方式

## type

## 自动门-属性

## 下拉框

## HTTPProxy

## ModbusTCPProxy

## ~latest

## 通信方式

## commandPeriod

## 自动门-属性

## s

## 1

## 0

## 100

## ~latest

## 发送指令间隔

当机器人进入门时，给门的控制指令周期为 1s。
## 开关门指令发送方式
## 电平触发，level
1653229035981-1edbe52f-be7e-45dd-aca5-c855af12427f.png

发送指令时，会将对应的地址写为设定值。例如， open 配置为 {"addressType":6,"address":100,"value":1} 时，发送开门指令时，会将地址100写为1，不会恢复。
## 边沿触发，edge
1653229071489-077231a0-b5e2-4f4e-829e-085be69eca4e.png

keep 参数表示保持高电平多久恢复，单位为毫秒。发送指令时，会将对应地址写为设定值，保持一段时间后，恢复。例如， open 配置为 {"addressType":6,"address":100,"value":1} ， keep 配置为 100 时，发送开门指令时，会将地址100写为1，保持100毫秒，然后将地址100写为0。

## ModbusTCP门控配置

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## close

### 自动门-属性-ModbusTCPProxy

## json string

{"address":16,"addressType":5,"value":0}

## ~latest

## 关门指令地址、值

## open

### 自动门-属性-ModbusTCPProxy

## json string

{"address":16,"addressType":5,"value":1}

## ~latest

## 开门指令地址、值

## status

### 自动门-属性-ModbusTCPProxy

## json string

{"address":0,"addressType":1,"value":1}

## 开门状态地址、值

## proxyName

### 自动门-属性-ModbusTCPProxy

## string

## proxyone

## 代理名称

## 示例：
1653229459241-aedad58c-c3c2-463c-8ec8-06368d077f2c.png

上图中的门，采用边沿触发方式发指令，保持100毫秒；关门时，向地址10写入1；开门时，向地址10写入0；读取到地址14的值为0时，表示门已开到位；否则表示门已关；使用名称为T的代理进行通信
## >0.1.9.240511
### 支持合并ModbusTCPProxy类型门控的状态读取

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## MergeReadDoor

## 参数配置-其他

## -

## false

## -

## ~latest

合并门控状态读取，为false时，不合并，为true时，尽量合并

### 开关门时需要读写多个地址的ModbusTCP配置
## >0.1.8.221103

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## closeList

### 自动门-属性-ModbusTCPProxy2

## json string

[{"address":16,"addressType":5,"value":0}]

## ~latest

## 关门指令地址、值列表

## openList

### 自动门-属性-ModbusTCPProxy2

## json string

{"address":16,"addressType":5,"value":1}

## ~latest

## 开门指令地址、值列表

## status

### 自动门-属性-ModbusTCPProxy2

## json string

{"address":0,"addressType":1,"value":1}

## 开门状态地址、值

## proxyName

### 自动门-属性-ModbusTCPProxy2

## string

## proxyone

## 代理名称

1668661366031-fb84f3eb-8026-4c3d-bbf7-d4441b60e947.png

## HTTP 门控配置

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## proxyName

## 自动门-属性-HTTPProxy

## string

## http-proxy-1

## -

## ~latest

## 代理名称

## 示例：
1653229630332-13e5b737-e6a0-4250-9a69-0eedb15776c5.png

### 注意：HTTP门控中，触发方式配置不生效
## HTTP 门控协议
## 控制门开关
## 请求：
## POST /controlDoor
## {
### "doorName":"DOOR_001", //门禁ID
### "state":"1" //控制指令 0关门  1开门
## }
## 响应：
## {
"result":0,    //  -1操作失败  -2不存在当前设备
## "message":"操作成功"
## }
## 查询门状态
## 请求：
## POST /doorStateList
## {
  "doorNameList":["DOOR_001,DOOR_002"] //门禁ID，支持英文,分割
## }
## Core每次只会查询一个门的状态
## 响应：
## {
  "result":0,
  "message":"操作成功",
## "data":[
## {
      "doorName":"设备id",
      "state":"门磁状态", // 此字段类型为整数，0门磁吸合，指门已经完全关闭；1 门磁未吸合，关闭中；
### 2 门完全打开，3门磁未吸合，打开中， -1门禁不在线,-2 不存在当前设备，-3 门故障
      "msg":"状态异常时的提示",
### "timestamp":0 //更新时间戳（s）
## }
## ]
## }

## 提前开门
机器人在进入门前需要让门的打开。优先场景需要门提前打开，这样机器人能够不减速的进过们。参数配置中的 OpenDoorDist 会实现这个功能。

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## OpenDoorDist

## 参数配置-导航

## m

### 0.0

### 0.0

### 100.0

## ~latest

机器人提前开门的距离，如果为0，则机器人在门的前置点会先停下来，打开门后再进入。

## 自动关门
用于解决人机共用自动门时，需要保持自动门常闭的需求。

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## KeepDoorClosed

## 参数配置-导航

## -

## true

## -

## -

## ~latest

在没有机器人使用门时，是否发送关门指令。

需要保持常闭时，将KeepDoorClosed改为true，core会在没有机器人用到门时，自动关门。
## 测试相关
使用modbus slave或类似工具测试时， addressType 和地址类型的关系：
1674893094916-c00da1bc-776d-49b7-b386-5ab3eb798ff7.png

## addressType值

### modbus slave中对应的地址类型

## 1,5,15

## 01 Coil Status

## 2

## 02 Input Status

## 3,6,16

## 03 Holding Register

## 4

## 04 Input Registers
