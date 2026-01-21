# 根据任务实例Id查询块运行列表以及任务状态

### 根据任务实例Id查询块运行列表以及任务状态
## 接口介绍
### 此 API 可以获取指定 任务实例 状态以及 块 运行列表
## 请求
功能：获取指定任务实例状态以及块运行列表。
## 方法：POST
接口说明： / api/queryBlocksByTaskId
POST "http://host:8080/api/queryBlocksByTaskId"
## 请求头

## Name

## Type

## Description

## Required

## language

## String

表示语言，不传则默认使用中文。（zh：中文，en：英文，ja-jp：日文）

## 否
## 请求参数

## Name

## Type

## Description

## Required

## taskRecordId

## String

## 表示任务实例Id

## 是
## 请求示例
查询英文环境下，任务实例id为c797a1d3-2aef-4bf6-8874-cf5d1c5602f7的块运行列表以及任务状态
## 请求头

## key

## value

## language

## en
## Body参数
## {
    "taskRecordId":"c797a1d3-2aef-4bf6-8874-cf5d1c5602f7"
## }
## 响应

## Name

## Type

## Description

## code

## int

## API 错误码，详情见 API 错误码

## msg

## String

## API 错误码信息

## data

## Object

## 返回的数据对象

## blockList

## List

## 表示当前任务实例的块运行情况列表

## blockName

## int

## 表示块的名称

## status

## String

表示块的运行状态（1000：块运行中；1001：块终止；1002：块暂停；1003：块完成；1004：块异常停止；1005：块未运行；1006：并行块结束；1007：块等待）

## startedOn

## Date

## 表示块的开始时间

## endedOn

## Date

## 表示块的结束时间

## endedReason

## String

## 表示块的结束原因

## blockLabel

## String

## 表示块的标签

## taskStatus

## String

表示任务运行状态（ 1000:运行中；1001:终止；1002：暂停；1003：结束；1004：异常结束；1005：重启异常；1006：异常中断；1007：手动结束 ）
## 响应数据示例
## Responses Code 200
## {
    "code": 200,
    "msg": "Success",
## "data": {
## "blockList": [
## {
                "blockName": "RootBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:56:57.000+00:00",
                "endedOn": "2022-05-23T04:58:00.000+00:00",
                "endedReason": "[RootBp]End Of Task",
### "blockLabel": "start block"
            },
## {
                "blockName": "QueryIdleSiteBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:56:57.000+00:00",
                "endedOn": "2022-05-23T04:56:58.000+00:00",
                "endedReason": "[QueryIdleSiteBp]Block Finish",
### "blockLabel": "Worksite"
            },
## {
                "blockName": "QueryIdleSiteBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:56:58.000+00:00",
                "endedOn": "2022-05-23T04:56:58.000+00:00",
                "endedReason": "[QueryIdleSiteBp]Block Finish",
### "blockLabel": "Worksite"
            },
## {
                "blockName": "IfBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:56:58.000+00:00",
                "endedOn": "2022-05-23T04:57:59.000+00:00",
                "endedReason": "[IfBp]Block Finish",
### "blockLabel": "Program Flow"
            },
## {
                "blockName": "CSelectAgvBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:56:58.000+00:00",
                "endedOn": "2022-05-23T04:57:58.000+00:00",
                "endedReason": "[CSelectAgvBp]Block Finish，agvId=sim_02",
### "blockLabel": "Core"
            },
## {
                "blockName": "CAgvOperationBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:57:18.000+00:00",
                "endedOn": "2022-05-23T04:57:43.000+00:00",
                "endedReason": "[CAgvOperationBp]Block Finish",
### "blockLabel": "Core"
            },
## {
                "blockName": "CAgvOperationBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:57:43.000+00:00",
                "endedOn": "2022-05-23T04:57:58.000+00:00",
                "endedReason": "[CAgvOperationBp]Block Finish",
### "blockLabel": "Core"
            },
## {
                "blockName": "IfBp",
                "status": 1003,
                "startedOn": "2022-05-23T04:57:59.000+00:00",
                "endedOn": "2022-05-23T04:58:00.000+00:00",
                "endedReason": "[IfBp]Block Finish",
### "blockLabel": "Program Flow"
## }
        ],
## "taskStatus": 1003
## }
## }
