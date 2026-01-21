# 库位监控（bin）

## 库位监控（bin）
## @杨达
## 解决的问题
## 实时更新库位状态
## 解决方式

1656041623380-d0ce2928-86bf-4a2e-a8f1-2c77764ac71e.png

### 通过光电传感器或者RoboView获取库位状态
## RDSCore通过轮询更新库位状态
### 上位系统通过HTTP API查询库位状态
## 使用PLC对接光电传感器的库位监控配置

## 字段

## 参数位置

## 类型

## 最小值

## 最大值

## binJson

## 库位监控-属性

## json array

## -

## 库位地址配置

## funCode

## 库位监控-属性

## uint16_t

## 1

## 16

## 读取库位状态的Modbus函数号

## proxyName

## 库位监控-属性

## string

## -

## 代理名称

## 配置示例：
1653227377652-fe6416c4-23a4-4767-9415-c2285854afd4.png

使用modbus slave测试时，funCode和地址类型的对照表：
1674894633881-339c2363-c0ea-4a06-b18e-90c26819e6d8.png

## funCode

## Function

## 1,3

## 01 Coil Status

## 2,4

## 02 Input Status
## binJson
## JSON数组，其中每个元素有下列字段：

## 字段

## 参数位置

## 类型

## 最小值

## 最大值

## binareaname

## 库位监控-属性-binJson

## string

## -

## 库区名称

## binaddress

## 库位监控-属性-binJson

## uint16_t

## 0

## 65535

## 读取库位状态的modbus地址位

## binholderaddress

## 库位监控-属性-binJson

## uint16_t

## 0

## 65535

## 读取库位所有者的modbus地址位

## binname

## 库位监控-属性-binJson

## string

## -

## 库位名称

## 单个库位配置示例：
## {
  "binareaname": "bb", // 库区名为bb
  "binholderaddress": 3, // 从地址位3中读取库位占用者信息
  "binaddress": 4, // 从地址位4中读取库位状态
  "binname" : "bin-02" // 库位名称位bin-02
## }
### binJson中有多个库位的配置，示例：
## [
## {
        "binareaname": "bb",
        "binaddress": 3,
        "binholderaddress": 4,
## "binname": "bin-02"
    },
## {
        "binareaname": "aa",
        "binaddress": 5,
        "binholderaddress": 6,
## "binname": "bin-01"
## }
## ]
## binHolder
用于多个系统同时操作的库位，如，人和机器人都会卸货的库位，或者多个系统控制的机器人都会卸货的库位。标识core能否操作当前库位。
如果只有core操作库位，将binHolderAddress配置为不用的地址即可，推荐配置为0。
## RDS端对应的配置
## 库位监测同步配置教程
## 使用RoboView的库位监控配置
## 调用协议

## 字段

## 参数位置

## 类型

## 默认值

## 最小值

## 最大值

### DisableRoboViewBinMonitor

## 参数配置-其他

## bool

## true

## -

## 禁用RoboView库位监测

## 使用RDS的库位监控配置
## > 0.1.8.221207

## 字段

## 参数位置

## 类型

## 默认值

## 最小值

## 最大值

### DisableRDSBinMonitor

## 参数配置-其他

## bool

## true

## -

## 禁用RDS库位监测

## RDS URL

## 参数配置-其他

## string

### http://localhost:8080

## -

## RDS地址
