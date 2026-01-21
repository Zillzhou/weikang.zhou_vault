# 终端（terminal)

## 终端（terminal)
## @杨达
## 解决的问题
执行任务时，机器人需要和产线设备对接。产线设备种类多，对接流程多变，需要有灵活的机制来支持。
## 解决方式
1656042466899-7bc78016-9239-40d5-8645-6a83b58d381b.png

## 使用两个“寄存器”实现对接：
TerminalStatus 表示设备输出，类似设备给出的DO，RDSCore读取
RobotStatus 表示RDSCore的输出，类似RDSCore给出的DO，设备读取
### 对接流程，以RDSCore向辊筒发送开始转动命令为例：
### RDSCore将转动指令写入 RobotStatus
辊筒设备读取 RobotStatus ，获取到开始转动的命令，执行指令，同时将回执消息写入 TerminalStatus
RDSCore读取 TerminalStatus ，确认指令发送成功
## 配置方式
### 目前支持使用ModbusTCP通信的两种方式：

## 种类名称

## 通信方式

## 备注

## proxy

## TCP

ModbusTCP通信协议，core作为master(客户端)

## slave

## TCP

ModbusTCP通信协议，core作为slave(服务器) 待支持

## simulate

## -

## 仿真
## proxy类型的配置

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## registerOne

## 场景-设备-proxy

## Modbus配置项

## -

## -

## -

## Core寄存器，Core读写的寄存器

## registerTwo

## 场景-设备-proxy

## Modbus配置项

## -

## -

## -

## 设备寄存器，设备读写的寄存器

## port

## 场景-设备-proxy

## uint16_t

## 503

## 503

## 65535

## 端口

## proxyName

## 场景-设备-proxy

## string

## proxy

## -

## -

## 代理名称

### Modbus配置项为 JSON对象，有两个字段：

## 字段

## 类型

## 描述

## address

## uint16_t

## modbus地址位，无前缀

## addressType

## uint

### modbus函数号，表示读/写操作时的函数号
## proxy类型的配置示例：
1653225034713-85b2e0b4-2cf6-460e-a486-35b883994a18.png

isConfigParameter 参数是为了和老版本兼容，不再使用。
## slave类型的配置

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## registerAddressMax

## 场景-设备-slave

## uint16_t

## 1000

## 0

## 65535

## 最多有多少个寄存器

## registerOne

## 场景-设备-slave

## Modbus配置项

## -

## -

## -

## Core寄存器

## registerTwo

## 场景-设备-slave

## Modbus配置项

## -

## -

## -

## 设备寄存器

## port

## 场景-设备-slave

## uint16_t

## 503

## 503

## 65535

## 端口

## slave类型的配置示例
1653225923597-06d9040d-b2c7-4bdc-ae13-f28954b31e72.png

## extendedProxy类型的终端
相比 proxy 类型的终端，增加了读写多个寄存器的功能，配置中增加了 number 选项，表示读写的寄存器数量。
