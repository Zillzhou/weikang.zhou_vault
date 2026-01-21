# M4 调度 API 文档

## M4 调度 API 文档
## HTTP 请求通用逻辑
M4 接口基础规范请参加：M4 HTTP 和 WebSocket 接口文档

## 特别注意：
如果输入参数有误，报 400。具体格式见上文连接。

## 编写原则
两级标题，第一级是分组，第二级是具体接口。不要出现更多级别。
每个接口：用法解释、URL、请求报文列表、响应报文列表、请求示例、响应示例。
URL 以 HTTP 动词开头，路径以根路径开头（含 api），如 `GET /api/ping` 。
WebSocket 接口给出 action 和请求响应报文格式（实体名 + 字段）。
要写出请求报文和响应报文在后端的实体名。
请求响应字段：名称（代码名称）、含义（中文标签）、类型、必填（可为空）、说明。
类型用 Java 类型写：String、Boolean、Int、Long、Double、Date；数组（列表）统一用 List，如 List<String>；枚举类型就写枚举类型的名字，在说明里列出所有枚举值。
对于有嵌套实体的，每个实体一个表格。
请求和响应实体被重用的，只列一次；其他地方引用之前的。
不解释接口和字段涉及的基本概念，如什么是点位、库位，什么是可打断；防止本文档过长；基础概念见产品文档。
URL、请求响应示例放在代码块里。

## WebSocket 报文格式
## TODO

## 运单
## 创建运单
给调度的指定场景新建一条调度运单。

## URL
### POST /api/fleet/orders/create

## 请求报文
## CreateOrderReq

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

运单所属场景的 ID。

## externalId

## 外部单号

## String

默认值为 null。
用户可传入自定义的文本内容，但是不能超过 50 个字符，例如 “上游业务单据的 ID”。

## priority

## 优先级

## Int

默认值为 0。数值越大，优先级越高。

## expectedRobotNames

## 指定机器人

## List<String>

可以执行此运单的机器人的名称。可以指定多个。

## expectedRobotGroups

## 指定机器人组

## List<String>

可以执行此运单的机器人组的名称。可以指定多个。

## containerId

## 容器编号

## String

机器人将要操作的容器的编号。不需要的时候可以不传。

## containerTypeName

## 容器类型名

## String

容器类型名称，必须是场景中存在的容器类型的名称。不需要的时候可以不传。

## containerDir

## 终点放置容器的方向

## Double

指定在终点放下货物时的货物朝向（世界坐标系）。单位是弧度（rad）。不需要的时候可以不传。

## keyLocations

## 关键位置

## List<String>

调度系统会基于关键位置派单。如果有运单步骤，可以不传。否则必须传一个地图中的点位。

## stepFixed

## 封口状态

## Boolean

默认值为 false。
false 表示运单未封口，可以继续添加运单步骤。
true 表示运单已封口，无法再添加运单步骤。

## steps

## 运单步骤列表

## List<CreateStepReq>

一个或多个运单步骤的集合。CreateStepReq 结构见下文。可以不传或为空，注意此时要传 keyLocations

## 响应报文

## 名称

## 含义

## 类型

## 说明

## orderId

## 运单 ID

## String

新创建的运单的 ID，由调度系统自动生成。

## externalId

## 外部单号

## String

内容为请求报文中指定的 externalId 的内容。

## 请求示例

## {
    "sceneId": "6870D00B569153590E058D3F",
    "priority": 0,
    "expectedRobotNames": [],
    "expectedRobotGroups": [],
    "keyLocations": [],
    "stepFixed": true,
## "steps": [
## {
            "location": "BHQ1_0105_04",
## "rbkArgs": {
## "binTask": "Load"
            },
            "forLoad": true,
            "forUnload": false,
            "withdrawOrderAllowed": true,
### "nextStepSameOrder": false
        },
## {
            "location": "BHQ1_0101_03",
## "rbkArgs": {
## "binTask": "Unload"
            },
            "forLoad": false,
            "forUnload": true,
            "withdrawOrderAllowed": false,
### "nextStepSameOrder": false
## }
## ]
## }

## 响应示例

## {
  "orderId": "TO20250806-000025",
## "externalId": null
## }

## 添加步骤
给未封口的非终态运单添加运单步骤。
调用此接口后，系统会将请求中的运单步骤，依次添加到目标运单的运单步骤之后。

## URL
POST /api/fleet/orders/add-steps

## 请求报文
## AddStepsReq

## 名称

## 含义

## 类型

## 必填

## 说明

## orderId

## 运单 ID

## String

## 是

需要添加步骤的运单的 ID。

## steps

## 运单步骤列表

## List<CreateStepReq>

## 是

一个或多个运单步骤组成的列表。

## CreateStepReq

## 名称

## 含义

## 类型

## 必填

## 说明

## location

## 作业位置

## String

## 是

作业位置可以是地图中的点位名称或库位名。
如果要求机器人去作业位置执行 binTask，则作业位置必须是库位名称。

## rbkArgs

## 动作参数

## Map<String, Any>

默认值为 null 。
如果机器人前往 “作业位置” 不需要执行动作，则动作参数的值为 null 或者 {}。
如果机器人要前往 “作业位置” 执行 binTask，则动作参数的格式为 {"binTask": "目标库位上存在的 binTask 的名称"}，例如 {"binTask": "Load"} 。 
如果机器人要前往 “作业位置” 执行 operation，非专业人士请忽略这种情况。

## forLoad

## 在此步取货

## Boolean

默认值为 false。
同一个步骤中，forLoad 和 forUnload 不能都是 true 。
false 表示机器人完成当前步骤后，调度系统不会将机器人标记为已取货状态。
true 表示机器人完成当前步骤后，调度系统会将此机器人标记为载货状态。

## forUnload

## 在此步放货

## Boolean

默认值为 false。
同一个步骤中，forLoad 和 forUnload 不能都是 true 。

### withdrawOrderAllowed

## 允许重分派

## Boolean

默认值为 false。

## nextStepSameOrder

## 强制要求下一步必须相同运单

## Boolean

默认值为 false。

## 响应报文
无。

## 请求示例

## {
  "orderId": "TO20250806-000031",
## "steps": [
## {
      "location": "AP2483",
## "rbkArgs": {
## "operation": "Load"
      },
      "forLoad": true,
## "forUnload": false
## }
## ]
## }

## 修改步骤
修改非终态的运单的运单步骤详情，并且只能修改可执行（Executable）状态的运单步骤。一次可以修改一个或多个，步骤以 stepIndex 为标识。

## URL
POST /api/fleet/orders/update-steps

## 请求报文
## UpdateStepsReq

## 名称

## 含义

## 类型

## 必填

## 说明

## orderId

## 运单 ID

## String

## 是

步骤所属的运单的 ID。

## steps

## 步骤 ID 的列表

## List<UpdateStepReq>

## 是

将被修改的步骤的集合。

## UpdateStepReq

## 名称

## 含义

## 类型

## 必填

## 说明

## stepIndex

## 步骤索引值

## Int

## 是

当前步骤在其所属运单中处于第几个步骤，从 0 开始计数。

## location

## 作业位置

## String

同 CreateStepReq 的 location 。

## rbkArgs

## 动作参数

## Map<String, Any>

同 CreateStepReq 的 rbkArgs 。

## forLoad

## 在此步取货

## Boolean

同 CreateStepReq 的 forLoad 。

## forUnload

## 在此步放货

## Boolean

同 CreateStepReq 的 forUnload 。

### withdrawOrderAllowed

## 允许充分派

## Boolean

同 CreateStepReq 的 withdrawOrderAllowed 。

## nextStepSameOrder

## 强制要求下一步必须相同运单

## Boolean

同 CreateStepReq 的 nextStepSameOrder 。

## 响应报文
无。

## 请求示例

// 将运单 TO20250806-000030 的 stepIndex 是 5 的步骤的 “作业位置”
## {
  "orderId": "TO20250806-000030",
## "steps": [
## {
      "stepIndex": 5,
### "location": "AP2483"
## }
## ]
## }

## 删除步骤
删除非终态的运单的若干个步骤，且只能删除可执行（Executable）状态的步骤。

## URL
POST /api/fleet/orders/delete-steps

## 请求报文

## 名称

## 含义

## 类型

## 必填

## 说明

## orderId

## 运单 ID

## String

## 是

将被删除的步骤所属的运单的 ID。

## stepIds

## 步骤 ID 的列表

## List<String>

## 是

将被删除的步骤 ID 的列表。

## 响应报文
无。

## 请求示例

## {
  "orderId": "TO20250806-000030",
## "stepIds": [
    "68931FBCB805DD4E555D5085",
### "68931FBCB805DD4E555D5087"
## ]
## }

## 运单封口
将一个运单封口。

## URL
POST /api/fleet/orders/complete-order

## 请求报文
## OrderIdReq

## 名称

## 含义

## 类型

## 必填

## 说明

## orderId

## 运单 ID

## String

## 是

将被封口的运单的 ID。

## 响应报文
无。

## 请求示例

## {
  "orderId": "TO20250806-000027"
## }

## 批量封口运单
对多个运单进行封口操作。

## URL
POST /api/fleet/orders/complete-order-batch

## 请求报文
请求报文详见：OrderIdsReq 。

## 响应报文
Map<String, ParallelResult<Object>>

## 名称

## 含义

## 类型

## 说明

## Map 的 key

### robotName 或者 orderId

## String

## 并行操作相关的机器人名称或者运单 id

## ParallelResult

## 并行操作返回值

## ParallelResult

## 名称

## 含义

## 类型

## 说明

## ok

## 该机器人或者运单相关操作是否成功

## Boolean

## True 为成功

## result

## 单个并行的方法返回结果

## String

## errMsg

## 错误信息

## String

## 如果执行失败，返回失败的原因
## 请求示例

## {
## "orderIds": [
    "TO20250806-000023", 
## "TO20250806-000026"
## ]
## }
## 响应示例

## {
### "TO20250806-000023": {
        "ok": true,
        "result": {},
## "errMsg": null
    },
### "TO20250806-000026": {
        "ok": true,
        "result": {},
## "errMsg": null
## }
## }
## 取消一条运单
根据运单 ID 取消此运单。
如果目标运单已处于不能取消的状态（已完成-Done、已取消-Cancelled），系统不报错，并忽略相关操作。

## URL
### POST /api/fleet/orders/cancel

## 请求报文
## CancelOrderReq

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

运单所属场景的 ID。

## orderId

## 运单 ID

## String

## 是

运单 ID。

## 响应报文
无。

## 请求示例

## {
    "sceneId": "6870D00B569153590E058D3F",
    "orderId": "TO20250806-000025"
## }

## 取消多条运单
取消多条运单。
如果目标运单已处于不能取消的状态（已完成-Done、已取消-Cancelled），系统不报错，并忽略相关操作。

## URL
POST /api/fleet/orders/cancel-batch

## 请求报文
## OrderIdsReq

## 名称

## 含义

## 类型

## 必填

## 说明

## orderIds

## 运单的 ID 列表

## List<String>

## 是

## 取消的运单 id 集合

## 响应报文
## 见

## 请求示例

## {
## "orderIds": [
    "TO20250806-000009",
    "TO20250806-000006",
## "TO20250806-000005"
## ]
## }

## 取消所有运单
取消一个场景下的所有运单。

## URL
POST /api/fleet/orders/cancel-all

## 请求报文
## SceneIdReq

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 场景 id

## 响应报文
无。

## 请求示例

## {
  "sceneId": "6879DAD928161A50EF3C1ADE"
## }

## 设置运单优先级
批量调整非终态运单的优先级。

## URL
POST /api/fleet/orders/update-priority-batch

## 请求报文

## 名称

## 含义

## 类型

## 必填

## 说明

## orderIds

## 运单 ID 的列表

## List<String>

## 是

## 设置优先级的运单列表

## priority

## 优先级

## Int

## 是

## 优先级

## 响应报文
无。

## 请求示例

## {
  "priority": 3,
## "orderIds": [
    "TO20250806-000028",
## "TO20250806-000027"
## ]
## }

## 故障重试
尝试继续执行故障的运单。
支持批量请求。

## URL
POST /api/fleet/orders/retry-failed-orders

## 请求报文
参见：OrderIdsReq  。

## 响应报文
## 见

## 请求示例

## {
## "orderIds": [
    "TO20250806-000010",
    "TO20250806-000034",
## "TO20250806-000015"
## ]
## }

## 运单批量查询
批量查询运单。

## URL
### POST /api/entity/find/many

## 请求报文

## 名称

## 含义

## 类型

## 必填

## 说明

## entityName

## 实体名称

## String

## 是

## 传固定值 TransportOrder

## query

## 查询条件

## ComplexQuery

## 否

## 见 M4 接口文档

## fuzzy

## 模糊查询参数

## String

## 否

## 模糊搜索 order id 的值

## projection

## 指定返回的字段或属性

## List<String>

## 否

### 可以裁剪业务对象字段。null 表示返回全部字段

## sort

## 排序方式

## List<String>

## 否

字段名前 - 表示倒序。+ 或空表示正序。 支持按多个字段排序

## skip

## 跳过前面的 N 个对象

## Int

## 否

## null 为忽略跳过

## limit

## 最多返回多少个对象

## Int

## 否

## null 或负数表示不限制

## 响应报文
### List<TransportOrder>

## TransportOrder
### （注意：TransportOrder 中是没有步骤信息的）

## 名称

## 含义

## 类型

## 说明

## id

## 单号

## String

## 一般由系统负责生成

## sceneId

## 场景 id

## String

## 场景 id

## sceneName

## 场景名称

## String

## status

## 单据状态

## OrderStatus

## 运单的状态有 6 个枚举值：
## ToBeAllocated：
## Allocated、
## Pending、
## Executing、
## Done、
Cancelled。

## externalId

## 外部单号

## String

## containerId

## 搬运的容器编号

## String

## containerTypeName

## 容器类型名称

## String

## containerDir

## 放置容器的方向

## Double

## 弧度

## taskBatch

## 对于多负载机器人期望一起取放的任务

## String

## priority

## 优先级

## Int

## expectedRobotNames

## 期望执行机器人

## List<String>

## 机器人名称集合

## expectedRobotGroups

## 期望执行机器人组

## List<String>

## 机器人组名集合

## keyLocations

## 关键位置

## List<String>

## createdOn

## 创建时间

## Date

## kind

## 运单类型

## OrderKind

## 运单的类型有 4 个枚举值：
## Business 正常业务单
## Parking 停靠单
## Charging 充电单
## IdleAvoid 避让单

## noLoad

## 此运单不涉及装卸货

## Boolean

## true  为不涉及

## fault

## 运单执行出现故障

## Boolean

## true 为故障

## faultReason

## 故障原因

## String

## stepNum

## 运单由几个步骤构成

## Int

## stepFixed

## 运单是否已经封口

## Boolean

## true 为封口

## currentStepIndex

## 当前正在执行的运单步骤

## Int

## doneStepIndex

## 已完成执行的运单步

## Int

## actualRobotName

## 实际执行机器人

## String

## oldRobots

## 历史分配机器人

## List<String>

## 只有业务单才会有 oldRobots

## robotAllocatedOn

## 分派机器人的时间

## Date

## loadPoint

## 取货点位

## String

## unloadPoint

## 放货点位

## String

## loaded

## 是否已完成取货

## Boolean

## true 为完成

## unloaded

## 是否已完成放货

## Boolean

## true 为完成

## dispatchCost

## 派单成本

## Double

## doneOn

## 运单完成时间

## Date

## processingTime

## 运单处理时间

## Double

## 单位秒

## executingTime

## 运单执行时间

## Double

## 单位秒

## failureNum

## 故障次数

## Int

## faultDuration

## 故障时长

## Double

## 单位秒

## loadDuration

## 取货时长

## Double

## 单位秒

## unloadDuration

## 放货时长

## Double

## 单位秒

## waitExecuteDuration

## 执行等待时长

## Double

## 单位秒

## 请求示例

## ### 按 id 批量查询
## {
  "entityName": "TransportOrder",
## "query": {
    "type": "General",
    "field1": "id",
    "operator": "In",
    "value": ["TO20250806-000227","TO20250806-000226","TO20250806-000224"]
## }
## }

## ### 指定返回 id
## {
  "entityName": "TransportOrder",
## "query": {
    "type": "General",
    "field1": "id",
    "operator": "In",
    "value": ["TO20250806-000227","TO20250806-000226","TO20250806-000224"]
    },
### "projection": ["id"]
## }

### ### 模糊搜索 id 近似 "20250806" 的运单
## {
  "entityName": "TransportOrder",
  "query": null,
## "fuzzy": "20250806"
## }

### ### 查询已封口的并且运单状态已完成的运单
## {
  "entityName": "TransportOrder",
## "query": {
        "type": "Compound",
## "items": [
## {
                "type": "General",
                "field1": "stepFixed",
                "operator": "Eq",
## "value": "true"
            },
## {
                "type": "General",
                "field1": "status",
                "operator": "Eq",
## "value": "Done"
## }
## ]
## }
## }

## ### 限制查询 2 条
## {
  "entityName": "TransportOrder",
  "query": null,
## "limit": 2
## }

### ### 按照创建时间倒序排序 并限制 2 条
## {
  "entityName": "TransportOrder",
  "query": null,
   "limit": 50,
## "sort": [
## "-createdOn"
## ]
## }

## 响应示例

## ### 按 id 批量查询
## [
## {
        "unloadDuration": 25.4,
        "robotAllocatedOn": 1754471343888,
        "executingTime": 27.94,
        "oldRobots": null,
        "unloadPoint": "AP26",
### "expectedRobotGroups": [
## "BOX"
        ],
        "createdOn": 1754471343887,
        "stepFixed": true,
        "loaded": true,
        "expectedRobotNames": null,
        "taskBatch": null,
        "modifiedOn": 1754471371828,
        "loadDuration": 1.933,
        "failureNum": 0,
        "sceneId": "6879DAD928161A50EF3C1ADE",
        "actualRobotName": "Box-02",
        "modifiedBy": null,
        "id": "TO20250806-000224",
        "containerId": "container-TO20250806-000224",
        "doneStepIndex": 1,
        "waitExecuteDuration": 0.769,
        "kind": "Business",
        "sceneName": "考核111",
        "fault": false,
        "dispatchCost": 0.0,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 9,
        "processingTime": 27.941,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": true,
        "doneOn": 1754471371828,
        "createdBy": null,
        "loadPoint": "AP43",
        "stepNum": 2,
## "keyLocations": [
            "AP43",
## "AP26"
        ],
        "currentStepIndex": 1,
        "containerTypeName": null,
## "status": "Done"
    },
## {
        "unloadDuration": 19.383,
        "robotAllocatedOn": 1754471363206,
        "executingTime": 86.488,
        "oldRobots": null,
        "unloadPoint": "AP41",
### "expectedRobotGroups": [
## "BOX"
        ],
        "createdOn": 1754471363205,
        "stepFixed": true,
        "loaded": true,
        "expectedRobotNames": null,
        "taskBatch": null,
        "modifiedOn": 1754471449695,
        "loadDuration": 10.06,
        "failureNum": 0,
        "sceneId": "6879DAD928161A50EF3C1ADE",
        "actualRobotName": "Box-02",
        "modifiedBy": null,
        "id": "TO20250806-000226",
        "containerId": "container-TO20250806-000226",
        "doneStepIndex": 1,
        "waitExecuteDuration": 0.536,
        "kind": "Business",
        "sceneName": "考核111",
        "fault": false,
        "dispatchCost": 6.0,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 11,
        "processingTime": 86.49,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": true,
        "doneOn": 1754471449694,
        "createdBy": null,
        "loadPoint": "AP44",
        "stepNum": 2,
## "keyLocations": [
            "AP44",
## "AP41"
        ],
        "currentStepIndex": 1,
        "containerTypeName": null,
## "status": "Done"
    },
## {
        "unloadDuration": 35.049,
        "robotAllocatedOn": 1754471374404,
        "executingTime": 45.274,
        "oldRobots": null,
        "unloadPoint": "AP33",
### "expectedRobotGroups": [
## "BOX"
        ],
        "createdOn": 1754471374403,
        "stepFixed": true,
        "loaded": true,
        "expectedRobotNames": null,
        "taskBatch": null,
        "modifiedOn": 1754471419678,
        "loadDuration": 7.528,
        "failureNum": 0,
        "sceneId": "6879DAD928161A50EF3C1ADE",
        "actualRobotName": "Box-02",
        "modifiedBy": null,
        "id": "TO20250806-000227",
        "containerId": "container-TO20250806-000227",
        "doneStepIndex": 1,
        "waitExecuteDuration": 0.402,
        "kind": "Business",
        "sceneName": "考核111",
        "fault": false,
        "dispatchCost": 2.0,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 9,
        "processingTime": 45.276,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": true,
        "doneOn": 1754471419678,
        "createdBy": null,
        "loadPoint": "AP25",
        "stepNum": 2,
## "keyLocations": [
            "AP25",
## "AP33"
        ],
        "currentStepIndex": 1,
        "containerTypeName": null,
## "status": "Done"
## }
## ]

## ### 指定返回 id
## [
## {
### "id": "TO20250806-000224"
    },
## {
### "id": "TO20250806-000226"
    },
## {
### "id": "TO20250806-000227"
## }
## ]

### ### 模糊搜索 id 近似 "20250806" 的运单
## [
## {
        "unloadDuration": null,
        "robotAllocatedOn": 1754458859961,
        "executingTime": 8.468,
        "oldRobots": null,
        "unloadPoint": null,
        "expectedRobotGroups": null,
        "createdOn": 1754458859912,
        "stepFixed": true,
        "loaded": false,
### "expectedRobotNames": [
## "Box-01"
        ],
        "taskBatch": null,
        "modifiedOn": 1754458868430,
        "loadDuration": null,
        "failureNum": 0,
        "sceneId": "6879DAD928161A50EF3C1ADE",
        "actualRobotName": "Box-01",
        "modifiedBy": null,
        "id": "TO20250806-000001",
        "containerId": null,
        "doneStepIndex": 0,
        "waitExecuteDuration": 2.438,
        "kind": "Business",
        "sceneName": "考核111",
        "fault": false,
        "dispatchCost": 3.0,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 5,
        "processingTime": 8.574,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": false,
        "doneOn": 1754458868429,
        "createdBy": "__admin__",
        "loadPoint": null,
        "stepNum": 1,
## "keyLocations": [
## "LM31"
        ],
        "currentStepIndex": 0,
        "containerTypeName": null,
## "status": "Done"
    },
## {
        "unloadDuration": null,
        "robotAllocatedOn": 1754458980810,
        "executingTime": 1.338,
        "oldRobots": null,
        "unloadPoint": null,
        "expectedRobotGroups": null,
        "createdOn": 1754458980777,
        "stepFixed": true,
        "loaded": false,
### "expectedRobotNames": [
## "Box-01"
        ],
        "taskBatch": null,
        "modifiedOn": 1754458982149,
        "loadDuration": null,
        "failureNum": 0,
        "sceneId": "6879DAD928161A50EF3C1ADE",
        "actualRobotName": "Box-01",
        "modifiedBy": null,
        "id": "TO20250806-000002",
        "containerId": null,
        "doneStepIndex": 0,
        "waitExecuteDuration": null,
        "kind": "Business",
        "sceneName": "考核111",
        "fault": false,
        "dispatchCost": 0.0,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 4,
        "processingTime": 1.391,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": false,
        "doneOn": 1754458982148,
        "createdBy": "__admin__",
        "loadPoint": null,
        "stepNum": 1,
## "keyLocations": [
## "LM31"
        ],
        "currentStepIndex": 0,
        "containerTypeName": null,
## "status": "Done"
## }
## ]

### ### 查询已封口的并且运单状态已完成的运单
## [
## {
        "unloadDuration": null,
        "robotAllocatedOn": 1751002435713,
        "executingTime": 10.75,
        "oldRobots": null,
        "unloadPoint": null,
        "expectedRobotGroups": null,
        "createdOn": 1751002435754,
        "stepFixed": true,
        "loaded": false,
### "expectedRobotNames": [
## "SVJ-04"
        ],
        "taskBatch": null,
        "modifiedOn": 1751002446464,
        "loadDuration": null,
        "failureNum": 0,
        "sceneId": "685D0771127A41171F932D3B",
        "actualRobotName": "SVJ-04",
        "modifiedBy": "",
        "id": "TO20250627-000001P",
        "containerId": null,
        "doneStepIndex": 0,
        "waitExecuteDuration": 2.042,
        "kind": "Parking",
        "sceneName": "thisd",
        "fault": false,
        "dispatchCost": null,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 4,
        "processingTime": 10.75,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": false,
        "doneOn": 1751002446463,
        "createdBy": "",
        "loadPoint": null,
        "stepNum": 1,
## "keyLocations": [
## "PP88"
        ],
        "currentStepIndex": 0,
        "containerTypeName": null,
## "status": "Done"
## }
## ]

## ### 限制查询 2 条
## [
## {
        "unloadDuration": null,
        "robotAllocatedOn": 1751002435713,
        "executingTime": 10.75,
        "oldRobots": null,
        "unloadPoint": null,
        "expectedRobotGroups": null,
        "createdOn": 1751002435754,
        "stepFixed": true,
        "loaded": false,
### "expectedRobotNames": [
## "SVJ-04"
        ],
        "taskBatch": null,
        "modifiedOn": 1751002446464,
        "loadDuration": null,
        "failureNum": 0,
        "sceneId": "685D0771127A41171F932D3B",
        "actualRobotName": "SVJ-04",
        "modifiedBy": "",
        "id": "TO20250627-000001P",
        "containerId": null,
        "doneStepIndex": 0,
        "waitExecuteDuration": 2.042,
        "kind": "Parking",
        "sceneName": "thisd",
        "fault": false,
        "dispatchCost": null,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 4,
        "processingTime": 10.75,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": false,
        "doneOn": 1751002446463,
        "createdBy": "",
        "loadPoint": null,
        "stepNum": 1,
## "keyLocations": [
## "PP88"
        ],
        "currentStepIndex": 0,
        "containerTypeName": null,
## "status": "Done"
    },
## {
        "unloadDuration": null,
        "robotAllocatedOn": 1751002435791,
        "executingTime": 43.37,
        "oldRobots": null,
        "unloadPoint": null,
        "expectedRobotGroups": null,
        "createdOn": 1751002435793,
        "stepFixed": true,
        "loaded": false,
### "expectedRobotNames": [
## "SVJ-01"
        ],
        "taskBatch": null,
        "modifiedOn": 1751002479162,
        "loadDuration": null,
        "failureNum": 0,
        "sceneId": "685D0771127A41171F932D3B",
        "actualRobotName": "SVJ-01",
        "modifiedBy": "",
        "id": "TO20250627-000002P",
        "containerId": null,
        "doneStepIndex": 0,
        "waitExecuteDuration": 1.96,
        "kind": "Parking",
        "sceneName": "thisd",
        "fault": false,
        "dispatchCost": null,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 4,
        "processingTime": 43.37,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": false,
        "doneOn": 1751002479161,
        "createdBy": "",
        "loadPoint": null,
        "stepNum": 1,
## "keyLocations": [
## "PP82"
        ],
        "currentStepIndex": 0,
        "containerTypeName": null,
## "status": "Done"
## }
## ]

### ### 按照创建时间倒序排序 并限制 2 条
## [
## {
        "unloadDuration": null,
        "robotAllocatedOn": null,
        "executingTime": 0.0,
        "oldRobots": null,
        "unloadPoint": null,
        "expectedRobotGroups": null,
        "createdOn": 1754530846134,
        "stepFixed": true,
        "loaded": false,
### "expectedRobotNames": [
## "SVJ-02"
        ],
        "taskBatch": null,
        "modifiedOn": 1754530865976,
        "loadDuration": null,
        "failureNum": 0,
        "sceneId": "6879DAD928161A50EF3C1ADE",
        "actualRobotName": "SVJ-02",
        "modifiedBy": null,
        "id": "TO20250807-000005A",
        "containerId": null,
        "doneStepIndex": 0,
        "waitExecuteDuration": 0.857,
        "kind": "IdleAvoid",
        "sceneName": "考核111",
        "fault": false,
        "dispatchCost": null,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 4,
        "processingTime": 19.843,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": false,
        "doneOn": 1754530865976,
        "createdBy": null,
        "loadPoint": null,
        "stepNum": 1,
## "keyLocations": [
## "LM91"
        ],
        "currentStepIndex": 0,
        "containerTypeName": null,
## "status": "Done"
    },
## {
        "unloadDuration": null,
        "robotAllocatedOn": null,
        "executingTime": 0.0,
        "oldRobots": null,
        "unloadPoint": null,
        "expectedRobotGroups": null,
        "createdOn": 1754530704541,
        "stepFixed": true,
        "loaded": false,
### "expectedRobotNames": [
## "Box-01"
        ],
        "taskBatch": null,
        "modifiedOn": 1754530713373,
        "loadDuration": null,
        "failureNum": 0,
        "sceneId": "6879DAD928161A50EF3C1ADE",
        "actualRobotName": "Box-01",
        "modifiedBy": null,
        "id": "TO20250807-000004A",
        "containerId": null,
        "doneStepIndex": 0,
        "waitExecuteDuration": 0.472,
        "kind": "IdleAvoid",
        "sceneName": "考核111",
        "fault": false,
        "dispatchCost": null,
        "externalId": null,
        "containerDir": null,
        "priority": 0,
        "version": 4,
        "processingTime": 8.833,
        "faultReason": null,
        "faultDuration": null,
        "unloaded": false,
        "doneOn": 1754530713372,
        "createdBy": null,
        "loadPoint": null,
        "stepNum": 1,
## "keyLocations": [
## "LM29"
        ],
        "currentStepIndex": 0,
        "containerTypeName": null,
## "status": "Done"
## }
## ]

## 运单分页查询
分页查询运单。

## URL
### POST /api/entity/find/page

## 请求报文
## FindPageReq

## 名称

## 含义

## 类型

## 必填

## 说明

## entityName

## 实体名称

## String

## 是

## 传固定值 TransportOrder

## query

## 查询条件

## ComplexQuery

## 否

## fuzzy

## 模糊查询参数

## String

## 否

## projection

## 指定返回的字段或属性

## List<String>

## 否

## sort

## 排序方式

## List<String>

## 否

## pageNo

## 页号

## Int

## 是

## pageSize

## 每页的数量

## Int

## 是

## 响应报文
## FindPageResult

## 名称

## 含义

## 类型

## 说明

## pageNo

## 页号

## Int

## pageSize

## 每页的数量

## Int

## total

## 总数

## Long

## page

## 查询的数据

### List<TransportOrder>

## 这里返回运单实体

## 请求示例

POST http://localhost:5173/api/entity/find/page
## {
  "entityName": "TransportOrder",
  "query": null,
  "pageNo": 1,
  "pageSize": 50,
## "sort": [
## "-createdOn"
## ]
## }

## 响应示例
## 条数太多 这里只展示一条

## {
  "pageNo": 1,
  "pageSize": 50,
  "total": 1681,
## "page": [
## {
      "unloadDuration": null,
      "robotAllocatedOn": null,
      "executingTime": 0,
      "oldRobots": null,
      "unloadPoint": null,
      "expectedRobotGroups": null,
      "createdOn": 1754467545831,
      "stepFixed": true,
      "loaded": false,
### "expectedRobotNames": [
## "Box-01"
      ],
      "taskBatch": null,
      "modifiedOn": 1754467557706,
      "loadDuration": null,
      "failureNum": 0,
      "sceneId": "6879DAD928161A50EF3C1ADE",
      "actualRobotName": "Box-01",
      "modifiedBy": null,
      "id": "TO20250806-000059A",
      "containerId": null,
      "doneStepIndex": 0,
      "waitExecuteDuration": 0.014,
      "kind": "IdleAvoid",
      "sceneName": "考核111",
      "fault": false,
      "dispatchCost": null,
      "externalId": null,
      "containerDir": null,
      "priority": 0,
      "version": 4,
      "processingTime": 11.877,
      "faultReason": null,
      "faultDuration": null,
      "unloaded": false,
      "doneOn": 1754467557706,
      "createdBy": null,
      "loadPoint": null,
      "stepNum": 1,
## "keyLocations": [
## "AP9"
      ],
      "currentStepIndex": 0,
      "containerTypeName": null,
## "status": "Done"
## }
## ]
## }

## 查询单个运单详情
查询目标运单的详情。
## 注：
/api/entity/find/one 接口为查询单个实体，可以指定查询条件，也可以指定字段查询。如果查询 TransportOrder 只返回该实体。
/api/fleet/orders/query-order-detail?orderId=xxx 接口返回指定运单的全部详情，包含运单和运单步骤。

## URL
GET /api/fleet/orders/query-order-detail?orderId=xxx
## URL参数

## 名称

## 含义

## 类型

## 必填

## 说明

## orderId

## 运单 ID

## String

## 是

## 查询的运单 ID

## 请求报文
无。

## 响应报文

## 名称

## 含义

## 类型

## 说明

## id

## 单号

## String

## 一般由系统负责生成

## sceneId

## 场景 id

## String

## 场景 id

## sceneName

## 场景名称

## String

## status

## 单据状态

## OrderStatus

## 运单的状态有 6 个枚举值：
## ToBeAllocated：
## Allocated、
## Pending、
## Executing、
## Done、
Cancelled。

## externalId

## 外部单号

## String

## containerId

## 搬运的容器编号

## String

## containerTypeName

## 容器类型名称

## String

## containerDir

## 放置容器的方向

## Double

## 弧度

## taskBatch

## 对于多负载机器人期望一起取放的任务

## String

## priority

## 优先级

## Int

## expectedRobotNames

## 期望执行机器人

## List<String>

## 机器人名称集合

## expectedRobotGroups

## 期望执行机器人组

## List<String>

## 机器人组名集合

## keyLocations

## 关键位置

## List<String>

## createdOn

## 创建时间

## Date

## kind

## 运单类型

## OrderKind

## 运单的类型有 4 个枚举值：
## Business 正常业务单
## Parking 停靠单
## Charging 充电单
## IdleAvoid 避让单

## noLoad

## 此运单不涉及装卸货

## Boolean

## True  为不涉及

## fault

## 运单执行出现故障

## Boolean

## True 为故障

## faultReason

## 故障原因

## String

## stepNum

## 运单由几个步骤构成

## Int

## stepFixed

## 运单是否已经封口

## Boolean

## True 为封口

## currentStepIndex

## 当前正在执行的运单步骤

## Int

## doneStepIndex

## 已完成执行的运单步

## Int

## actualRobotName

## 实际执行机器人

## String

## oldRobots

## 历史分配机器人

## List<String>

## 只有业务单才会有 oldRobots

## robotAllocatedOn

## 分派机器人的时间

## Date

## loadPoint

## 取货点位

## String

## unloadPoint

## 放货点位

## String

## loaded

## 是否已完成取货

## Boolean

## True 为完成

## unloaded

## 是否已完成放货

## Boolean

## True 为完成

## dispatchCost

## 派单成本

## Double

## doneOn

## 运单完成时间

## Date

## processingTime

## 运单处理时间

## Double

## 单位秒

## executingTime

## 运单执行时间

## Double

## 单位秒

## failureNum

## 故障次数

## Int

## faultDuration

## 故障时长

## Double

## 单位秒

## loadDuration

## 取货时长

## Double

## 单位秒

## unloadDuration

## 放货时长

## Double

## 单位秒

## waitExecuteDuration

## 执行等待时长

## Double

## 单位秒

## allocationReject

## 此运单无法被分派的原因

## RejectReason

## executionReject

## 此运单无法被执行的原因

## RejectReason

## steps

## 运单的步骤

## List<EntityValue>

## steps

## 名称

## 含义

## 类型

## 说明

## id

## 步骤 id

## String

## orderId

## 运单 id

## String

## 所属运单

## stepIndex

## 运单的第几个步骤

## Int

## 从 0 开始

## status

## 步骤状态

## StepStatus

## 运单步骤的状态有 4 个枚举值：
## Executable：可执行
## Executing：正在被执行
## Done：完成
Skipped：暂未使用的状态。

### withdrawOrderAllowed

## 是否允许运单重分派

## Boolean

## True 为不允许

## location

## 点位或库位

## String

## percentage

## 路径百分比

## Double

## rbkArgs

## 动作参数

## String

## forLoad

## 在此步取货

## Boolean

## True 为取货

## forUnload

## 在此步卸货

## Boolean

## True 为卸货

## createdOn

## 创建时间

## Date

## startOn

## 开始时间

## Date

## endOn

## 结束时间

## Date

## processingTime

## 运单步骤处理时间

## Double

## 单位秒

## executingTime

## 运单步骤执行时间

## Double

## 单位秒

## nextStepSameOrder

## 强制要求下一步不能切换运单

## Boolean

## True 为要求

## RejectReason

## 名称

## 含义

## 类型

## 说明

## code

## 拒绝的编码

## String

## params

## 参数

## List<Object>

## 请求示例

GET http://localhost:5173/api/fleet/orders/query-order-detail?orderId=TO20250806-000055

## 响应示例

## {
    "unloadDuration": null,
    "robotAllocatedOn": null,
    "executingTime": null,
    "oldRobots": null,
    "unloadPoint": "AP125",
### "expectedRobotGroups": [
## "AMR"
    ],
    "createdOn": 1754467471825,
    "stepFixed": true,
    "loaded": false,
    "expectedRobotNames": null,
    "taskBatch": null,
    "modifiedOn": 1754467471825,
    "loadDuration": null,
    "failureNum": 0,
    "sceneId": "6879DAD928161A50EF3C1ADE",
    "actualRobotName": null,
    "modifiedBy": null,
    "id": "TO20250806-000055",
    "containerId": "container-TO20250806-000055",
    "doneStepIndex": -1,
    "waitExecuteDuration": null,
    "executionReject": null,
    "kind": "Business",
    "sceneName": "考核111",
    "fault": false,
    "dispatchCost": null,
    "externalId": null,
    "containerDir": null,
    "priority": 0,
    "version": 0,
## "steps": [
## {
            "executingTime": 0.0,
            "nextStepSameOrder": false,
            "withdrawOrderAllowed": true,
            "orderId": "TO20250806-000055",
            "startOn": null,
            "version": 0,
            "createdOn": 1754467471824,
            "processingTime": 0.0,
            "forUnload": false,
            "modifiedOn": 1754467471824,
            "stepIndex": "0",
            "createdBy": null,
            "endOn": null,
            "modifiedBy": null,
            "location": "AP110",
            "id": "TO20250806-000055-Step0",
            "rbkArgs": "{\"operation\":\"Load\"}",
            "status": "Executable",
## "forLoad": true
        },
## {
            "executingTime": 0.0,
            "nextStepSameOrder": false,
            "withdrawOrderAllowed": false,
            "orderId": "TO20250806-000055",
            "startOn": null,
            "version": 0,
            "createdOn": 1754467471824,
            "processingTime": 0.0,
            "forUnload": true,
            "modifiedOn": 1754467471824,
            "stepIndex": "1",
            "createdBy": null,
            "endOn": null,
            "modifiedBy": null,
            "location": "AP125",
            "id": "TO20250806-000055-Step1",
            "rbkArgs": "{\"operation\":\"Unload\"}",
            "status": "Executable",
## "forLoad": false
## }
    ],
    "processingTime": null,
    "faultReason": null,
    "faultDuration": null,
    "unloaded": false,
    "doneOn": null,
    "createdBy": null,
    "loadPoint": "AP110",
### "allocationReject": {
        "code": "AllRobotUnDispatchable",
## "params": [
## {
### "SW500": "RobotNoMoreBin"
## }
## ]
    },
    "stepNum": 2,
## "keyLocations": [
        "AP110",
## "AP125"
    ],
    "currentStepIndex": -1,
    "containerTypeName": null,
### "status": "ToBeAllocated"
## }

## WebSocket 查询运单详情
## 请求 action
WebSocket Fleet::OrderDetail::Query

## 请求报文
## OrderReq

## 名称

## 含义

## 类型

## 必填

## 说明

## orderId

## 运单 id

## String

## 是

## 响应 action
### Fleet::OrderDetail::Reply

## 响应报文
## 同查询单个运单详情

## 请求示例

## {
    "action": "Fleet::OrderDetail::Query",
    "content": "{"orderId":"TO20250806-000601"}" // 参考 HTTP 处的请求示例。
## }

## 响应示例

## {
    "action":"Fleet::OrderDetail::Reply",
### "content": "同查询单个运单详情"
## }

## 机器人
## 机器人常见业务对象
## RobotUiReport

## 名称

## 含义

## 类型

## 说明

## robotName

## 机器人名称

## String

## offDuty

## 不接单

## Boolean

## config

## 机器人配置

## SceneRobot

## groupId

## 机器人组 ID

## Int

## groupName

## 机器人组名

## String

## online

## 是否在线

## Boolean

## selfReport

## 机器人自身报告

## RobotSelfReport

## lastFetchError

## 最新一次获取机器人状态异常

## String

## stopAutoConnect

## 停止自动重连

## Boolean

## collisionModel

## 机器人的碰撞模型

## Polygon

## orderRecord

## 机器人运单方面的信息

## RobotOrderUiReport

## isMaster

## 是否有控制权

## Boolean

## ka

## 是否正在进行关键动作

## Boolean

## RobotSelfReport

## 名称

## 含义

## 类型

## 说明

## error

## 是否有错误

## String

## errorMsg

## 错误原因

## Boolean

## timestamp

## 获取机器人报告的时间

## Date

## main

## 主要字段

## RobotSelfReportMain

## stand

## 机器人当前位置

## RobotStand

## rawReport

## 原始报文

## EntityValue

## RobotSelfReportMain

## 名称

## 含义

## 类型

## 说明

## battery

## 电池电量

## Double

## 值为 0 到 1 之间

## x

## 机器人位置 x 坐标

## Double

## y

## 机器人位置 y 坐标

## Double

## direction

## 机器人车头朝向

## Double

## 单位 rad

## currentMap

## 当前地图名

## String

## currentMapMd5

## 当前地图 MD5

## String

### currentMapNotMatched

### 与机器人所在组的同名地图的 MD5 是否相等

## Boolean

## currentPoint

## 当前站点名称

## String

## blocked

### 是否被阻挡（障碍物、其他机器人、含义复杂……）

## Boolean

## blockedReason

## 被阻挡的原因

## Int

## 0 = Ultrasonic (超声)
## 1 = Laser (激光)
### 2 = Fallingdown (防跌落传感器)
### 3 = Collision (碰撞传感器)
### 4 = Infrared (红外传感器)
## 5 = Lock（锁车开关）
## 6 = 动态障碍物
## 7 = 虚拟激光点
## 8 = 3D 相机

## charging

## 充电中

## Boolean

## emergency

## 急停按下

## Boolean

## softEmc

## 软急停按下

## Boolean

## relocStatus

## 重定位状态

## RelocStatus

## Init -> 重定位初始化中
## Success -> 重定位成功
## Relocing -> 正在重定位
## Loading -> 地图载入中

## relocStatusLabel

## 重定位状态描述

## String

## confidence

## 定位置信度

## Double

## 值为 0 到 1

## todayOdo

## 今日里程

## Double

## 单位 m

## isControlled

## 是否有机器人控制权

## Boolean

## controller

## 控制权所有者名称

## String

## controlledByFleet

## 受调度控制

## Boolean

## noControlByFleet

## 不受调度控制

## Boolean

### controlledByFleetOrFree

## 受调度控制或自由

## Boolean

## alarms

## 告警

## List<RobotAlarm>

## loadRelations

## 机器人载货情况

### List<RobotLoadRelation>

## velocity

### 机器人在机器人坐标系的 x 方向的直线速度

## Double

## 单位 m/s，正值是正走，负值则倒走

## rotateVelocity

## 机器人角速度

## Double

## 单位 rad/s

## timestamp

## 获取机器人位置的时间

## Date

## moveStatusInfo

## 机器人导航信息

## String

## bins

## 机器人上的容器信息

## List<RobotSelfBin>

## RobotSelfBin

## 名称

## 含义

## 类型

## 说明

## rbkBinId

## 机器人库位编号

## Int

即背篓的编号，从 0 开始，支持 N+1 则货叉索引是 999

## containerId

## 容器编号

## String

## 即货物编号

## desc

## 容器的描述

## String

## 即货物描述

## occupied

## 是否占用

## Boolean

## true 表示已占用
## false 表示未占用

## RobotStand

## 名称

## 含义

## 类型

## 说明

## x

## 位置坐标 x

## Double

## y

## 位置坐标 y

## Double

## theta

## 朝向

## Double

## 单位 rad

## areaId

## 所在区域 id

## Int

## areaName

## 所在区域名

## String

## mapName

## 当前地图

## String

## pointName

## 当前点位名称

## String

## 可能为空，如果不在点位上

## pathPositions

## 距离机器人点位最近的路径集

## List<PathPosition>

## bestStartPointName

## 派单、规划起点

## String

## velocity

## 直线速度

## Double

## rotateVelocity

## 旋转角速度

## Double

## timestamp

## 获取机器人位置的时间

## Date

## collisionShape

## 碰撞框，注意是世界坐标系

## Polygon

## PathPosition

## 名称

## 含义

## 类型

## 说明

## pathId

## 所在路径 id

## Long

## pathKey

## 所在路径 key

## String

## pathPercentage

## 在路径上的位置

## Double

## pathDistance

## 机器人档位位置距离此路径的距离

## Double

## fromPointName

## 起点名称

## String

## toPointName

## 终点名称

## String

## EntityValue
EntityValue 是一个 Map<String, Object> 类型的业务对象。

## RobotOrderUiReport

## 名称

## 含义

## 类型

## 说明

## offDuty

## 不接单

## Boolean

## cmdStatus

## 机器人执行运单的状态

## RobotExecuteStatus

## faultMsg

## 机器人故障信息

## String

## orders

## 运单列表

## List<String>

## withBzOrder

## 是否存在业务单

## Boolean

## autoOrder

## 自动单

## String

## bins

## 机器人库位列表

## List<RobotBin>

## idleFrom

## 空闲时间

## Date

## chargingOrderDoneOn

## 充电完成时间

## Date

## reportingChargingOn

## 最新一次机器人上报正在充电的时间

## Date

## executingSelected

## 选择要执行的运单步骤

## StepExecuteDigest

## orderReject

## 机器人不能接单原因

## RejectReason

## stepReject

## 不能执行步骤的原因

## RejectReason

## trafficReady

## 机器人的交管状态是否初始化完成

## Boolean

## pendingTrafficTask

## 一个机器人需要交管规划执行的任务

## TrafficTaskRuntime

## robotBlockedBy

## 阻挡当前机器人的其他机器人列表

## List<String>

## containerBlockedBy

## 阻挡当前机器人的其他货架列表

## List<String>

## travelledPointNames

## 已经经过的点位名称列表

## List<String>

## unTravelPointNames

## 未经过的点位名称列表

## List<String>

## spaceResources

## 资源占用情况

## List<SpaceResource>

## recentReq3066s

## 最近 3 条 3066 指令

### List<NavTaskRuntime>

### currentTargetPointName

## 当前目标点

## String

## moveActions

## 移动动作

### List<MoveActionRuntime>

## extra

## 额外信息

## Object

## 添加一个机器人
为指定的场景添加一个机器人。

## URL
POST /api/fleet/scenes/{sceneId}/robot/create
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## SceneRobot

## 名称

## 含义

## 类型

## 必填

## 说明

## robotName

## 机器人名称

## String

## 否

## 默认是空字符

## disabled

## 是否停用

## Boolean

## 否

## 默认 false

## remark

## 备注

## String

## 否

## groupId

## 所属组 ID

## Int

## 否

## 默认是 0

## tags

## 标签

## List<String>

## 否

## 默认是空集合

## vendor

## 品牌

## RobotVendor

## 否

默认是 Seer。
## RobotVendor 的值：
## Seer -> 仙工
## Hai -> 海柔
## Hik -> 海康

## selfBinNum

## 机器人库位数

## Int

## 否

## 表示一次能载几个货，默认是 1

## image

## 图片

## Object

## 否

## connectionType

## 机器人和调度的连接方式

## RobotConnectionType

## 否

默认是 null。RobotConnectionType 的值：
### FleetToRobot -> 调度连接机器人
### RobotToFleet -> 机器人连接调度
## GwWs -> GW 上报

## robotIp

## 机器人 IP

## String

## 否

connectionType 为 FleetToRobot 时必填

## robotPortStart

## 机器人起始端口

## Int

## 否

## 请求机器人的 TCP 起始端口

## ssl

## 连接使用 SSL 加密

## Boolean

## 否

## 默认 null，海柔使用

## gwAuthId

## 网关用户 ID

## String

## 否

### connectionType 为 GwWs 时必填

## gwAuthSecret

## 网关登录密钥

## String

## 否

### connectionType 为 GwWs 时必填

## simulated

## 是否为仿真

## Boolean

## 否

### 默认 false，为 true 则自动创建仿真机器人

## plusOne

## 是否支持 N + 1

## Boolean

## 否

## 多储位机器人使用

## 响应报文
无。

## 请求示例

## ### 创建机器人
## # 成功响应 200

POST http://localhost:5173/api/fleet/scenes/4564564446Ssfd5/robot/create
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "robotName": "AMB-01",
  "disabled": false,
  "remark": "",
  "groupId": 1,
  "tags": [],
  "vendor": "Seer",
  "selfBinNum": 1,
  "image": null,
  "connectionType": "FleetToRobot",
  "robotIp": "192.168.10.205",
  "robotPortStart": null,
  "ssl": null,
  "gwAuthId": "",
  "gwAuthSecret": "",
  "simulated": false,
## "plusOne": false
## }

## 添加多个机器人
为指定场景批量创建机器人。

## URL
POST /api/fleet/scenes/{sceneId}/robot/create/batch
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文

## 名称

## 含义

## 类型

## 必填

## 说明

## 无

## 无

## List<SceneRobot>

## 是

## 机器人数组

## 响应报文
无。

## 请求示例

## ### 批量创建机器人
POST http://localhost:5173/api/fleet/scenes/4564564446Ssfd5/robot/create/batch
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## [
## {
      "robotName": "AMB-02",
      "disabled": false,
      "remark": "",
      "groupId": 1,
      "tags": [],
      "vendor": "Seer",
      "selfBinNum": 1,
      "image": null,
      "connectionType": "FleetToRobot",
      "robotIp": "192.168.10.201",
      "robotPortStart": null,
      "ssl": null,
      "gwAuthId": "",
      "gwAuthSecret": "",
      "simulated": false,
## "plusOne": false
    },
## {
      "robotName": "AMB-03",
      "disabled": false,
      "remark": "",
      "groupId": 1,
      "tags": [],
      "vendor": "Seer",
      "selfBinNum": 1,
      "image": null,
      "connectionType": "FleetToRobot",
      "robotIp": "192.168.10.206",
      "robotPortStart": null,
      "ssl": null,
      "gwAuthId": "",
      "gwAuthSecret": "",
      "simulated": false,
## "plusOne": false
## }
## ]

## 添加指定数量的机器人
为指定场景生成指定数量、指定前缀的机器人。

## URL
POST /api/fleet/scenes/{sceneId}/robot/create/auto-batch
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## CreateRobotBatchReq

## 名称

## 含义

## 类型

## 必填

## 说明

## robotNamePrefix

## 机器人名称前缀

## String

## 是

## 根据前缀生成有规律的名称

## count

## 创建机器人的数量

## Int

## 是

## remark

## 备注

## String

## 否

## groupId

## 所属组 ID

## Int

## 否

## 默认是 0

## tags

## 标签

## List<String>

## 否

## 默认是空集合

## vendor

## 品牌

## RobotVendor

## 否

默认是 Seer。
## RobotVendor 的值：
## Seer -> 仙工
## Hai -> 海柔
## Hik -> 海康

## selfBinNum

## 机器人库位数

## Int

## 否

## 表示一次能载几个货，默认是 1

## image

## 图片

## Object

## 否

## connectionType

## 机器人和调度的连接方式

## RobotConnectionType

## 否

默认是 null。RobotConnectionType 的值：
### FleetToRobot -> 调度连接机器人
### RobotToFleet -> 机器人连接调度
## GwWs -> GW 上报

## robotIpStart

## 机器人起始 IP

## String

## 否

connectionType 为 FleetToRobot 时必填

## robotPortStart

## 机器人起始端口

## Int

## 否

## 请求机器人的 TCP 起始端口

## ssl

## 连接使用 SSL 加密

## Boolean

## 否

## 默认 null，海柔使用

## gwAuthId

## 网关用户 ID

## String

## 否

### connectionType 为 GwWs 时必填

## gwAuthSecret

## 网关登录密钥

## String

## 否

### connectionType 为 GwWs 时必填

## simulated

## 是否为仿真

## Boolean

## 否

### 默认 false，为 true 则自动创建仿真机器人

## plusOne

## 是否支持 N + 1

## Boolean

## 否

## 多储位机器人使用

## 响应报文
无。

## 请求示例

## ### 添加指定数量的机器人
## # 成功响应 200

POST http://localhost:5173/api/fleet/scenes/4564564446Ssfd5/robot/create/auto-batch
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "robotNamePrefix": "AMB",
  "count": 10,
  "remark": "",
  "groupId": 1,
  "tags": [],
  "vendor": "Seer",
  "selfBinNum": 1,
  "image": null,
  "connectionType": "FleetToRobot",
  "robotIp": "192.168.10.205",
  "robotPortStart": null,
  "ssl": null,
  "gwAuthId": "",
  "gwAuthSecret": "",
  "simulated": false,
## "plusOne": false
## }

## 删除多个机器人
根据机器人名称为指定场景批量删除指定的机器人。

## URL
POST /api/fleet/scenes/{sceneId}/robot/remove
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## DeleteRobotsReq

## 名称

## 含义

## 类型

## 必填

## 说明

## robotNames

## 机器人名称集合

## List<String>

## 是

## 响应报文
无。

## 请求示例

## ### 删除多个机器人
## # 成功响应 200

POST http://localhost:5173/api/fleet/scenes/4564564446Ssfd5/robot/remove
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "robotNames": ["AMB-01", "AMB-02"]
## }

## 更新一个机器人配置
更新指定场景下的机器人配置。

## URL
POST /api/fleet/scenes/{sceneId}/robot/update/config
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## SceneRobot

## 响应报文
无。

## 请求示例

## ### 更新一个机器人配置
## # 成功响应 200

POST http://localhost:5173/api/fleet/scenes/4564564446Ssfd5/robot/update/config
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "robotName": "AMB-01",
  "disabled": false,
  "remark": "",
  "groupId": 1,
  "tags": [],
  "vendor": "Seer",
  "selfBinNum": 1,
  "image": null,
  "connectionType": "FleetToRobot",
  "robotIp": "192.168.10.205",
  "robotPortStart": null,
  "ssl": null,
  "gwAuthId": "",
  "gwAuthSecret": "",
  "simulated": false,
## "plusOne": false
## }

## 批量更新机器人配置
根据机器人名称与指定的配置更新指定场景中对应的机器人。

## URL
POST /api/fleet/scenes/{sceneId}/robot/update/config/batch
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
### UpdateRobotConfigBatchReq

## 名称

## 含义

## 类型

## 必填

## 说明

## robotNames

## 机器人名称集合

## List<String>

## 是

## config

## 机器人配置

### SceneRobotDefaultNull

## 是

### SceneRobotDefaultNull

## 名称

## 含义

## 类型

## 必填

## 说明

## disabled

## 是否停用

## Boolean

## 否

## 默认 false

## remark

## 备注

## String

## 否

## groupId

## 所属组 ID

## Int

## 否

## 默认是 0

## tags

## 标签

## List<String>

## 否

## 默认是空集合

## vendor

## 品牌

## RobotVendor

## 否

默认是 Seer。
## RobotVendor 的值：
## Seer -> 仙工
## Hai -> 海柔
## Hik -> 海康

## selfBinNum

## 机器人库位数

## Int

## 否

## 表示一次能载几个货，默认是 1

## image

## 图片

## Object

## 否

## connectionType

## 机器人和调度的连接方式

## RobotConnectionType

## 否

默认是 null。RobotConnectionType 的值：
### FleetToRobot -> 调度连接机器人
### RobotToFleet -> 机器人连接调度
## GwWs -> GW 上报

## robotIp

## 机器人 IP

## String

## 否

connectionType 为 FleetToRobot 时必填

## robotPortStart

## 机器人起始端口

## Int

## 否

## 请求机器人的 TCP 起始端口

## ssl

## 连接使用 SSL 加密

## Boolean

## 否

## 默认 null，海柔使用

## gwAuthId

## 网关用户 ID

## String

## 否

### connectionType 为 GwWs 时必填

## gwAuthSecret

## 网关登录密钥

## String

## 否

### connectionType 为 GwWs 时必填

## simulated

## 是否为仿真

## Boolean

## 否

### 默认 false，为 true 则自动创建仿真机器人

## plusOne

## 是否支持 N + 1

## Boolean

## 否

## 多储位机器人使用

## 响应报文
## 见

## 请求示例

## ### 批量更新机器人配置
## # 成功响应 200

POST http://localhost:5173/api/fleet/scenes/4564564446Ssfd5/robot/update/config/batch
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "robotNames": ["AMB-01", "AMB-02"],
## "config": {
      "disabled": false,
      "remark": "",
      "groupId": 1,
      "tags": [],
      "vendor": "Seer",
      "selfBinNum": 1,
      "image": null,
      "connectionType": "FleetToRobot",
      "robotIp": "192.168.10.205",
      "robotPortStart": null,
      "ssl": null,
      "gwAuthId": "",
      "gwAuthSecret": "",
      "simulated": false,
## "plusOne": false
## }
## }

## 重连
机器人断开时，使用此接口为机器人尝试重新连接。

## URL
POST /api/fleet/robots/reconnect

## 请求报文
## ReconnectReq

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## robotNames

## 机器人名称集合

## List<String>

## 否

## 默认为 null，即不更新任何机器人

## 响应报文
## 见

## 请求示例

## ### 指定的机器人尝试重连
## # 成功响应 200

POST http://localhost:5173/api/fleet/robots/reconnect
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "sceneId": "45748sdf450DB",
  "robotNames": ["AMB-01", "AMB-02"]
## }

## 设置不接单
当希望机器人不接单/接单的时候，可以通过此接口实现。

## URL
POST /api/fleet/robots/off-duty

## 请求报文
## UpdateOffDutyReq

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## robotNames

## 机器人名称集合

## List<String>

## 是

## offDuty

## 不接单

## Boolean

## 否

## 默认 false，表示不接单

## 响应报文
## 见

## 请求示例

## ### 设置不接单接口
## # 成功响应 200

POST http://localhost:5173/api/fleet/robots/off-duty
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "sceneId": "45748sdf450DB",
  "robotNames": ["AMB-01", "AMB-02"],
## "offDuty": true
## }

## 获取所有机器人信息
查询指定场景的所有机器人信息。

## URL
### GET /api/fleet/robots/all-all
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 否

指定 ID 则忽略 sceneName。sceneId 与 sceneName 至少填一个。

## sceneName

## 场景名称

## String

## 否

未填写 sceneId 时，使用 sceneName。sceneId 与 sceneName 至少填一个。

## rawAll

同时获取机器人的原始报文信息。

## Boolean

## 否

默认值为 false。
当 rawAll 的值为 true 时，获取的机器人信息还会包含机器人自身上报的，且调度系统未处理过的信息，对应的字段是 raw 。

## 请求报文
无。

## 响应报文
### Map<String, UiRobotReport>

## 名称

## 含义

## 类型

## 说明

## 无

## 无

### Map<String, UiRobotReport>

### key 为机器人名称，value 为机器人的信息

## 请求示例

## ### 获取所有机器人信息
## # 成功返回响应体

GET http://localhost:5173/api/fleet/robots/all-all?sceneId=5646fd4545Bd&sceneName=test&rawAll=true
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## 响应示例
UiRobotReport 的信息维度较多，示例中只展示一个机器人信息。

## {
## "AMB-01": {
    "name": "AMB-01",
    "offDuty": false,
    "online": true,
    "disabled": false,
    "mock": true,
    "rTime": 1762851797507,
    "x": -2.826,
    "y": -1.208,
    "d": 1.5708,
    "p": "PP2",
    "bp": "PP2",
    "v": 0.0,
    "rv": 0.0,
    "areaId": 1,
    "map": "4F_jack_yg_0701_2_1.smap",
    "mapMd5": "4d3770fd6347a2799a5ed561fdd28540",
    "cff": true,
    "controller": "M4QuickFleet",
    "emc": false,
    "sEmc": false,
    "blocked": false,
    "alarms": [],
## "collision": {
## "points": [
## {
          "x": -0.475,
## "y": 0.325
        },
## {
          "x": 0.475,
## "y": 0.325
        },
## {
          "x": 0.475,
## "y": -0.325
        },
## {
          "x": -0.475,
## "y": -0.325
## }
      ],
      "type": "Other",
      "ignoreRobotSafeDist": null,
      "ignoreLoadSafeDist": null,
## "empty": false
    },
    "battery": 1.0,
    "charging": false,
    "c": 0.67396,
    "reloc": "Success",
    "to": 0.0,
## "raw": {
      "ret_code": 0,
      "battery_level": 1.0,
      "x": -2.826,
      "y": -1.208,
      "vx": 0.0,
      "w": 0.0,
      "angle": 1.5707963267948966,
      "current_station": "PP2",
      "blocked": false,
      "charging": false,
      "current_map": "4F_jack_yg_0701_2_1.smap",
      "vehicle_id": "AMB-01",
## "current_lock": {
        "nick_name": "M4QuickFleet",
## "locked": true
      },
      "reloc_status": 1,
      "fatals": [],
      "errors": [],
      "warnings": [],
      "notices": [],
      "current_map_md5": "4d3770fd6347a2799a5ed561fdd28540",
      "goods_region": null,
      "confidence": 0.6739635307653263,
      "emergency": false,
      "soft_emc": false,
      "odo": 0.0,
      "today_odo": 0.0,
      "containers": [],
      "loadmap_status": 0,
      "task_status": 0,
      "task_type": 0,
      "target_id": "",
      "task_id": "",
## "fork_height": 0.0
    },
## "bins": [
## {
        "index": 0,
## "status": "Empty"
## }
    ],
    "noAc": false,
    "cmdStatus": "Idle",
    "withBzOrder": false,
    "orders": [],
    "ka": false,
    "unTravelled": [],
## "spaces": [
## {
        "type": "Rect",
## "points": [
## {
            "x": -3.151,
## "y": -1.683
          },
## {
            "x": -3.151,
## "y": -0.733
          },
## {
            "x": -2.501,
## "y": -0.733
          },
## {
            "x": -2.501,
## "y": -1.683
## }
        ],
## "radius": null
## }
    ],
    "tReady": true,
## "bbr": []
## }
## }

## 设置控制权
可以通过此接口对指定的机器人集合获取或释放控制权。

## URL
### POST /api/fleet/robots/master

## 请求报文
## ChangeMasterReq

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## robotNames

## 机器人名称的集合

## List<String>

## 是

## on

## 是否控制

## Boolean

## 否

默认 false，true 则获取控制权，false 则释放控制权

## 响应报文
## 见

## 请求示例

## ### 设置控制权
## # 成功响应 200

POST http://localhost:5173/api/fleet/robots/master
Content-Type: application/json
Cookie: Pycharm-e4ba248e=346b9c2a-5b5c-431d-bdb6-3ffd4a1a0d19; xzz-qyq-5173=__admin__; xzz-qyx-5173=XZ9ud1K0Uc4yrz5U15M25ewS;

## {
  "sceneId": "45748sdf450DB",
  "robotNames": ["AMB-01", "AMB-02"],
## "on": true
## }

## 查询机器人当前点位
通过此接口可以知道机器人当前所在的位置。

## URL
GET /api/fleet/robots/query-point?sceneId=xxx&robotName=yyy
## QueryParams

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## robotName

## 机器人名

## String

## 是

## 请求报文
无。

## 响应报文
## RobotPoint

## 名称

## 含义

## 类型

## 说明

## pointName

## 点位名

## String

## direction

## 方向，单位 rad

## Double

## x

## X 坐标，单位 m

## Double

## y

## Y 坐标，单位 m

## Double

## 响应示例

## {
  "pointName": "AP3",
  "direction": 0.12,
  "x": 2.12,
## "y": 3.41
## }

## 获取机器人当前所有地图
从场景中获取目标机器人可能会用到的所有地图。

## URL
GET /api/fleet/robots/{sceneId}/maps/{robotName}
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## robotName

## 机器人名

## String

## 是

## 请求报文
无。

## 响应报文
### List<RobotMapNameMd5>

## RobotMapNameMd5

## 名称

## 含义

## 类型

## 说明

## mapName

## 用户、接口使用的地图名称

## String

## mapMd5

## 地图 MD5

## String

## current

## 是机器人当前使用的地图

## Boolean

## 响应示例

## [
## {
    "mapName": "F2_1_1.smap",
    "mapMd5": "1c980cc53c4420a05fa6d4b30e333f41",
## "current": true
## }
## {
    "mapName": "F5.smap",
    "mapMd5": "a6d444200cc53ca05fb30e333f411c98",
## "current": false
## }
## ]

## 清除机器人自身错误
清除机器人自身的错误。

## URL
POST /api/fleet/robots/{sceneId}/clear-alarm
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## RobotNamesReq

## 名称

## 含义

## 类型

## 必填

## 说明

## robotNames

## 机器人名称 list

## List<String>

## 是

## 机器人名称集合

## 响应报文
## 见

## 请求示例

POST http://localhost:5173/api/fleet/robots/6879DAD928161A50EF3C1ADE/clear-alarm

## {
## "robotNames": [
## "Box-01"
## ]
## }

## 设置机器人的软急停状态
设置机器人软急停，让机器人立即停下。

## URL
POST /api/fleet/robots/{sceneId}/set-soft-emc
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## SetSoftEmcReq

## 名称

## 含义

## 类型

## 必填

## 说明

## robotNames

## 机器人名称列表

## List<String>

## 是

## enable

## 软急停

## Boolean

## 是

## true -> 设置软急停
## false -> 取消软急停

## 响应报文
## 见

## 请求示例

@sceneId = 6870D00B569153590E058D3F

## ### 启用、停用机器人
POST http://localhost:5800/api/fleet/robots/{{sceneId}}/set-soft-emc
Content-Type: application/json
## xyy-app-id: m4
### xyy-app-key: seer1234

## # 请求正文
# 将场景 6870D00B569153590E058D3F 中的机器人 Fork-02 置为软急停状态
## {
    "enable": true,
## "robotNames": [
## "Fork-02"
## ]
## }

## 启用停用机器人
可以将场景中的机器人置为启用状态或者停用状态。
停用状态的机器人将不会出现在调度场景中，除非再次启用此机器人。

## URL
POST /api/fleet/robots/{sceneId}/disabled
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## UpdateDisabledReq

## 名称

## 含义

## 类型

## 必填

## 说明

## robotNames

## 机器人名称列表

## List<String>

## 是

## disabled

## 是否停用机器人

## Boolean

## 是

## true -> 停用机器人
## false -> 启用机器人

## 响应报文
无。

## 请求示例

@sceneId = 6870D00B569153590E058D3F

## ### 启用、停用机器人
POST http://localhost:5800/api/fleet/robots/{{sceneId}}/disabled
Content-Type: application/json
## xyy-app-id: m4
### xyy-app-key: seer1234

## # 请求正文
# 停用场景 6870D00B569153590E058D3F 中的机器人 Fork-02
## {
    "disabled": true,
## "robotNames": [
## "Fork-02"
## ]
## }

## 智能恢复
## 调用此接口之后：
如果机器人离线，尝试重连。
如果机器人没有控制权，尝试获取（仅当机器人控制权为空）。
如果机器人当前没有运单，重置交管。
如果机器人当前有故障运单，尝试故障重试。
不论机器人有没有任务，都清除机器人告警。（如果有错误，系统会自动再上报，不会出现把错误清没了的情况。）

## URL
POST /api/fleet/robots/{sceneId}/auto-recovery
## URL 参数

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## 请求报文
## RobotNamesReq

## 名称

## 含义

## 类型

## 必填

## 说明

## robotNames

## 机器人名称的列表

## List<String>

## 是

目标场景中，需要尝试智能恢复的机器人的名称的集合。

## 响应报文
## 见

## 请求示例

@sceneId = 6870D00B569153590E058D3F

## ### 智能恢复
POST http://localhost:5800/api/fleet/robots/{{sceneId}}/auto-recovery
Content-Type: application/json
## xyy-app-id: m4
### xyy-app-key: seer1234

## # 请求正文
## {
## "robotNames": [
## "Fork-02"
## ]
## }

## 机器人实时与回放状态
## 注意：
UiRobotReport 中的 Double 类型的字段，传给页面时及回放中只保留 3 位小数。如 x、y、方向、速度、旋转速度、锁定空间的坐标

## UiRobotReport

## 名称

## 含义

## 类型

## 列表卡片

## 地图上

## 详情

## 回放
## 默认有

## 说明

## name

## 机器人名

## String

## 有

## 有

## 有

## offDuty

## 不接单

## Boolean

## 有

## 无

## 有

## online

## 在线

## Boolean

## 有

## 无

## 有

## connectMsg

## 连接报错

## Boolean

## 有

## 无

## 有

## 无

## rTime

## 获取机器人报告的时间

## Date

## 有

## 无

## 有

## x

## x 坐标

## Double

## 无

## 无

## 有

## 单位 m

## y

## y 坐标

## Double

## 无

## 无

## 有

## 单位 m

## d

## 机器人朝向

## Double

## 无

## 无

## 有

## 单位 rad

## p

## 当前站点名称

## String

## 无

## 无

## 有

## v

## 速度，m/s

## Double

## 无

## 无

## 有

机器人在机器人坐标系的 x 方向的直线速度, 单位 m/s，正值是正走，负值则倒走

## rv

## 机器人角速度

## Double

## 无

## 无

## 有

## 单位 rad/s

## areaId

## 所在区域 id

## Int

## 无

## 无

## 无

## map

## 当前地图名

## String

## 无

## 无

## 有

## mapMd5

## 当前地图 MD5

## Double

## 无

## 无

## 有

## 无

## cff

## 受调度控制或自由

## Boolean

## 有

## 无

## 有

## controller

## 控制权所有者名称

## String

## 有

## 无

## 有

## emc

## 是否急停

## Boolean

## 有

## 有

## 有

## sEmc

## 是否软急停

## Boolean

## 有

## 有

## 有

## blocked

## 机器人自身上报的被阻挡

## Boolean

## 有

## 有

## 有

## blockedMsg

## 被阻挡原因

## Int

## 有

## 无

## 有

## 查询机器人的被阻挡状态

## alarms

## 机器人自身告警

## List<RobotAlarm>

## 有

## 无

## 有

## collision

## 机器人上报的碰撞模型（机器人坐标系）

## Polygon

## 无

## 无

## 无

## loads

## 机器人载货情况

### List<RobotLoadRelation>

## 有

## 无

## 有

## battery

## 电池电量 0..1

## Double

## 有

## 无

## 有

## 取值范围 0～1

## charging

## 充电中

## Boolean

## 有

## 无

## 有

## c

## 定位置信度 0..1

## Double

## 无

## 无

## 无

## 取值范围 0～1

## reloc

## 重定位状态

## RelocStatus

## 有

## 无

## 有

## RelocStatus

## Init -> 重定位初始化中
## Success -> 重定位成功
## Relocing -> 正在重定位
## Loading -> 地图载入中

## to

## 今日里程

## Double

## 无

## 无

## 有

## 无

## navId

## 当前路径导航的 id

## String

## 机器人正在执行的 3066 指令 id

## navStatus

## 当前路径导航状态

## Int

## 机器人正在执行的 3066 指令状态

## bins

## 库位

## List<RobotBin>

## 有

## 无

## 有

## 有。但去掉  containerId

## noAc

## 已停止自动重连

## Boolean

## 有

## 无

## 无

机器人失败次数超过场景配置的限制后，会停止自动重连。默认 5 次。

## cmdStatus

## 机器人执行运单的状态

## RobotExecuteStatus

## 无

## 有

## 有

## RobotExecuteStatus
## Idle -> 空闲
## Moving -> 移动中
## Failed -> 失败

## oFaultMsg

## 故障运单的故障原因

## String

## 有

## 无

## 无

## withBzOrder

## 在执行业务单

## Boolean

## 有

## 有

## 有

## autoOrder

## 自动单单号

## String

## 有

## 有

## 有

## orders

## 运单列表

## List<String>

## 无

## 无

## 有

## cuOrderId

## 当前运单

## String

## 有

## 有

## 有

## cuStepIndex

## 当前步骤 index

## Int

## 有

## 有

## 有

## cuStepLocation

## 当前步骤作业点位或库位

## String

## 无

## 无

## 无

## op

## 当前步骤动作

## String

## 无

## 无

## 有

## targetPoint

当前目标点。如果有交管任务，则取交管的目标点位，否则取 step 对应的点位

## String

## 有

## 无

## 有

## unTravelled

## 未来将要走的路径

## List<String>

## 无

## 有

## 无

## spaces

## 申请空间资源信息

## List<SpaceResource>

## 无

## 有

## 无

## tReady

## 机器人在交管中已初始化

## Boolean

## 无

## 无

## 有

## ttId

## 交管任务 ID

## String

## 无

## 无

## 有

## ttStatus

## 交管任务状态

## TrafficTaskStatus

## 无

## 有

## 有

### TrafficTaskStatus，应该不用翻译
## Created
## TrafficAccepted
## Success
## Failed
## Cancelled

## bbr

## 阻挡当前机器人的其他机器人列表

## List<String>

## 无

## 无

## 有

## raw

## 机器人自身上报的，且调度未处理的信息

## EntityValue

## 无

## 无

## 无

## 无

默认值是 null 。
相关内容参见：Robokit API 1100 的 响应 。

## RobotAlarm

## 名称

## 含义

## 类型

## 说明

## level

## 告警级别

## RobotAlarmLevel

## Info
## Warning
## Error
## Fatal

## code

## 错误码

## String

## message

## 错误信息

## String

## times

## 重复次数

## Int

## timestamp

## 报错时间

## Date

## RobotLoadRelation

## 名称

## 含义

## 类型

## 说明

## type

## 货物类型

## String

## id

## 货物 ID

## String

## 一般为容器编号

## points

## 形状

## List<Point2D>

## direction

## 方向

## Double

## Point2D

## 名称

## 含义

## 类型

## 说明

## x

## x 坐标

## Double

## y

## y 坐标

## Double

## RobotBin

## 名称

## 含义

## 类型

## 说明

## index

## 库位索引

## Int

## 从 0 开始

## status

## 机器人库位状态

## RobotBinStatus

## Empty -> 空
## Reserved -> 预
## Filled -> 满
## Cancelled -> 取消

## orderId

## 与此库位关联的运单

## String

## containerId

## 与此库位关联的容器编号

## String

## SpaceResource

## 名称

## 含义

## 类型

## 说明

## type

## 类型

## SpaceResourceType

## Rect -> 矩形
## Circle -> 圆形
## Polygon -> 多边形

## points

## 顶点坐标 , 如果是圆，为圆心坐标

## List<Point2D>

## radius

## 圆的半径

## Double

## Polygon

## 名称

## 含义

## 类型

## 说明

## points

## 点位

## List<Point2D>

## type

## 形状

## PolygonShape

## Other
## Rect

## 场景
WebSocket Fleet::Scene::Query 查询场景实施数据

## 查询场景实时数据
## 请求 action
### WebSocket Fleet::Scene::Query

## 请求报文
## DevQueryReq

## 名称

## 含义

## 类型

## 必填

## 说明

## sceneId

## 场景 ID

## String

## 是

## excluded

## 不需要的属性

## Set<String>

## withRawReport

## 需要机器人的原始报文。已废弃

## Boolean

### 不要传 true。传 true，M4 页不返回原始报文

## orderQueryType

## 查询运单的类型

## OrderQueryType

## AllOrders -> 全部运单
### NoFinishedOrders  -> 未完成运单
## FaultOrders -> 失败运单

## 响应 action
### WebSocket Fleet::Scene::Query

## 响应报文

## 名称

## 含义

## 类型

## 说明

## status

## 场景状态

## SceneStatus

## Disabled -> 停用
### Initializing -> 初始化中
## Initialized -> 已初始化
## Disposed -> 已销毁

## robots

## 机器人状态

### Map<String, UiRobotReport>

## orders

## 运单，受查询条件影响

## List<EntityValue>

## goingCount

## 执行中的运单数

## Long

## faultCount

## 失败的运单数

## Long

## ordersRecords

## 运单状态

Map<String, OrderRuntimeRecord>

## dispatchProfile

## 派单性能跟踪

## DispatchProfile

## trafficDebug

## 交管 Debug 数据

## Object

## 不用管

## sceneConfig

## 场景配置

## SceneConfig

## trafficConditions

## 路况

## Map<String, Int>

## 只返回拥堵情况大于 0 的路径
路径 key -> 拥堵度数。

## keepOrderNum

## 随机发单保持的单数

## Int

## randomOrdersOn

## 是否启用随机罚单

## Boolean

## replayOrdersStop

## 运单重放

## Boolean

## doors

## 所有门的实时状态

### Map<String, UiDoorReport>

## lifts

## 所有电梯的状态

### Map<String, UiLiftReport>

## OrderRuntimeRecord

## 名称

## 含义

## 类型

## 说明

## orderId

## 运单 id

## String

## status

## 运单状态

## OrderStatus

## allocationReject

## 此运单无法被分派的原因

## RejectReason

## executionReject

## 此运单无法被执行的原因

## RejectReason

## createdOn

## 创建时间

## Date

## SceneConfig

## 名称

## 含义

## 类型

## 说明

### robotStateFetchDelay

## 机器人状态获取间隔

## Long

## 单位 ms

robotStateFailureNumToAutoConnect

## 连续几次读取失败，自动重连

## Int

读取一个机器人的状态，如果连续失败次数超过这个值，将断开当前连接，重新连接

robotStateErrorToAutoReconnectMax

## 自动重连最多几次

## Int

连续读取失败自动重连，如果连续重连 N 次都失败，将放弃自动重连，界面将提示人工介入，手工重连

## noOpLog

## 关闭运行记录

## Boolean

## True 为关闭

## opLogKeepDays

## 运行记录保留天数

## Int

## noRbkMsgLog

## 不记录与机器人的报文

## Boolean

## True 为不记录

## dispatchMethod

## 派单策略

## DispatchMethod

## 枚举值有 2 个：
## KM
## Greedy

## withdrawnMinCost

## 重新分派运单成本差异阈值

## Double

robotPlanPausedOnSelfReportErrorOrFatal

机器人上报 Error 或 Fatal 错误时，暂停机器人派单

## Boolean

## True 为暂停

## notCancelRobot

### 取消正在执行的运单步骤时，不向机器人发取消

## Boolean

## True 为不取消

### parkingCollisionCheckMode

## 停靠充电碰撞检测模式

## CollisionCheckMode

## 现在枚举值只有 1 个：
## None

## parkingPaused

## 暂停停靠

## Boolean

## True 为暂停

## noReallocation

## 不重新分配运单

## Boolean

## True 为不重新分配

## dispatchSort

## 派单排序

## List<String>

### dispatchAcceptableTimeout

### 最大允许的派单等待时机，超过后将优先处理

## Long

## 单位 s

## chargingPaused

## 暂停充电

## Boolean

## True 为暂停

## chargingConfig

## 充电配置

## SceneChargingConfig

## cpnEnabled

## 是否启用光通讯，目前这个是立即生效的！

## Boolean

## 光通讯配置

## cpnDispatchPeriod

## 光通讯派单周期

## Long

## 光通讯配置

## trafficPlanPaused

## 暂停交管规划

## Boolean

## 交管公共配置

### trafficPlanPausedOnFailedPlan

## 规划失败暂停继续规划

## Boolean

## 交管公共配置

## closePointDistance

判断机器人在点位上，允许的最大距离。单位米。判断机器人是否在一个点位上，计算机器人和点位的距离，如果在此距离内，认为机器人在点位上。

## Double

## 交管公共配置

## closePathDistance

判断机器人在路径上，允许的最大距离。单位米。判断机器人是否在一条路径上，机器人机器人和路径的距离，如果在此距离内，认为机器人在路径上。

## Double

## 交管公共配置

### trafficPlanIntervalMs

TP1 交通规划重规划间隔（毫秒）。默认 5 秒。

## Long

## 交管公共配置

## trafficDevConfigStr

## 交管高级配置

## String

## 交管公共配置

## cbsHighW

## Double

## TP2 配置

## cbsLowW

## Double

## TP2 配置

## linkMinDistance

## 死锁环推最小距离

## Double

## TP1 配置

## linkMaxDistance

## 死锁环推最大距离

## Double

## TP1 配置

## reverseDeadlock

## 倒车解死锁

## Boolean

## TP1 配置

## enablePrevent

## 是否开启死锁预防

## Boolean

## TP1 配置

## preRotate

## 前置圆同时申请

## Boolean

## TP1 配置

## trafficMethod

## 交管策略

## TrafficMethod

## Distributed -> TP1
## Venus -> TP2

## selectConfig

## 批量选择元素的配置

## String

## SceneChargingConfig

## 名称

## 含义

## 类型

## 说明

## defaultConfig

## 默认配置

## BasicChargingConfig

## groupedConfigs

## 机器人组充电配置

### Map<Int, BasicChargingConfig>

## BasicChargingConfig

## 名称

## 含义

## 类型

## 说明

## chargeOnly

## 强充电量

## Double

低于 chargeOnly 后，机器人不接普通运单，直接去充电，至少充到 chargeOk 后，再解除强充状态

## chargeNeed

## 可充电量

## Double

低于 chargeNeed 后，机器人会生成充电任务，充电时可接普通运单

## chargeOk

## OK 电量

## Double

机器人低电量强充，至少充到此电量后才能接单；非强充，充到此电量前也能接单。大于此电量后，机器人可接普通运单，执行完后去停靠点，不去充电点

## chargeFull

## 满电量

## Double

高于 chargeFull 后，机器人生成停靠运单离开充电点

## minChargingTime

## 最小充电时间

## Int

## 单位 s

## idleTime

## 生成充电任务前的空闲时间

## Int

## 单位 s

## UiLiftReport

## 名称

## 含义

## 类型

## 列表卡片

## 地图上

## 详情

## 说明

## id

## 电梯 ID

## Int

## 无

## 无

## 有

## name

## 名称

## String

## 无

## 有

## disabled

## 禁用

## Boolean

## 无

## 有

## online

## 在线

## Boolean

## 无

## 有

## uTime

## 最后一次更新时间

## Date

## 无

## 无

## auto

## Boolean

## 无

## cf

## 当前楼层

## String

## 无

## tf

## 目标楼层

## String

## 无

## doors

## 电梯门的状态

### List<LiftDoorStatus>

## 无

## Unknown -> 门状态未知
## Closed -> 门已关闭
## Closing -> 门正在关闭
## Opening -> 门正在打开
## Opened -> 门已打开

## people

## Boolean

## 无

## fault

## 故障

## Boolean

## 无

## faultMsg

## 故障信息

## String

## 无

## UiDoorReport

## 名称

## 含义

## 类型

## 列表卡片

## 地图上

## 详情

## 说明

## id

## 门 ID

## Int

## name

## 名称

## String

## disabled

## 禁用

## Boolean

## online

## 在线

## Boolean

## uTime

## 最后一次更新时间

## Date

## status

## 门的状态

## DoorMainStatus

## Unknown -> 门状态未知
## Closed -> 门已关闭
## Closing -> 门正在关闭
## Opening -> 门正在打开
## Opened -> 门已打开

## fault

## 故障

## Boolean

## faultMsg

## 故障信息

## String

## openRobots

## 开门机器人列表

## List<String>

## 获取单个场景
## （本次先不写）

## 外部地图资源申请
## 外部地图资源申请
增量申请外部资源，可重入。
## 注：谁申请需要谁去释放
一次请求，只允许同一个 owner 申请一个场景的某个区域中的资源。

## URL
POST  /api/fleet/external-map-res/request

## 请求报文
### List<MapResourceUnit>

## 名称

## 含义

## 类型

## 必填

## 说明

## unitId

## 单元 ID

## String

## 是

## 每次申请必须唯一，建议使用 UUID

## owner

## 所有者

## String

## 是

可以是机器人名称、外部系统名称。
约束：实施时，外部系统名不能与机器同名，不同外部系统名称唯一。如果是系统内机器人，则使用系统内机器人名称。

## reason

## 原因

## String

## 是

## 填写申请的原因，默认可填 “ ”

## sceneId

## 场景 ID

## String

## 否

在哪个场景下进行申请外部空间资源，即填写那个场景的 ID，如果不填，则会根据 sceneName 查找所属场景。

## sceneName

## 场景名称

## String

## 否

只有在不使用 sceneId 时，此字段才会生效。
在哪个场景下进行申请外部空间资源，即填写那个场景的名称，如果不填，调度系统会从启用的场景中选择一个。

## areaId

## 区域 ID

## Int

## 否

即所在场景下的区域 id，即想在哪个场景下哪个区域来申请外部空间资源。如果不填会根据 areaName 查找所属场景的当前区域。

## areaName

## 区域名称

## String

## 否

只有在不使用 areaId 时，此字段才会生效。
即所在场景下的区域的名称，即想在哪个场景下哪个区域来申请外部空间资源。如果不填，调度系统会从所属场景中选择一个可用区区域。

## pointNames

## 占用点位

## List<String>

## 否

申请点位（名称）集合的空间资源，默认占有点位 0.05m 范围的矩形空间。见下文示例。

## pathKeys

## 占用路径

## List<String>

## 否

申请路线集合的空间资源，默认占有路线 0.05m 范围的空间。见下文示例。

## spatialZones

## 占用空间区域

## List<Polygon>

## 否

## 申请空间资源

## Polygon

## 名称

## 含义

## 类型

## 必填

## 说明

## points

## 坐标的集合

## List<Point2D>

## 是

## 每个多边形的顶点坐标

## type

## 类型

## PolygonShape

## 是

## 多边形的类型
## PolygonShape
## Other  其他类型
## Rect  矩形

## Point2D

## 名称

## 含义

## 类型

## 必填

## 说明

## x

## X 坐标

## Double

## 是

## y

## Y 坐标

## Double

## 是

## 响应报文
结果为 true  表示申请成功； false 表示申请失败，部分情况下还会描述失败的原因：

## 序号

## 失败的原因

## 1

## owner 不能为空

## 2

## units 不能为空

## 3

同一次申请的资源必须属于同一个场景的相同区域，且 owner 也得相同

## 4

## ID 为 “aaa” 的场景已停用

## 5

## 不存在 ID 为 “aaa” 的场景

## 6

## 名称为 “bbb” 的场景已停用

## 7

## 不存在名称为 “bbb” 的场景

## 8

## 没有启用的场景

## 9

### 存在多个启用的场景，请提供明确的场景信息

## 10

目标场景（ID=aaa，name=bbb）中 ID 为 1 的区域已停用，区域名称为 ccc

## 11

目标场景（ID=aaa，name=bbb）中名称为 ccc 的区域已停用

## 12

目标场景（ID=aaa，name=bbb）中没有 ID 为 1 的区域

## 13

目标场景（ID=aaa，name=bbb）中没有名称为 ccc 的区域

## 14

目标场景（ID=aaa，name=bbb）中所有区域都被禁用了

## 15

目标场景（ID=aaa，name=bbb）存在多个启用区域，请提供明确的区域信息

## 16

申请的部分点位或线路不存在于目标区域内，场景 ID=aaa，场景名称=bbb，区域 ID=1，区域名称=ccc

## 17

申请的资源（unitId=a-004-327）已经被 owner=AMB-06 占用

## 请求示例
## 申请占用点位

## [
## {
        "unitId": "a-004-1",
        "owner": "AMB-06",
        "sceneId": "685A06C1373D9D2CFD46D1A4",
        "areaId": 0,
## "pointNames": [
            "AP1090",
            "AP1089",
## "AP1088"
## ]
## }
## ]

## ## 不传 场景 区域
## [
## {
        "unitId": "a-004-327",
        "owner": "Box-02",
## "pointNames": [
            "AP5",
## "LM29"
## ]
## }
## ]
## 申请占用路径

## [
## {
        "unitId": "a-004-1",
        "owner": "AMB-06",
        "sceneId": "685A06C1373D9D2CFD46D1A4",
        "areaId": 0,
## "pathKeys": [
            "AP33->LM32",
## "AP16->LM22"
## ]
## }
## ]

## ## 不传 场景 区域
## [
## {
        "unitId": "a-004-327",
        "owner": "Box-02",
## "pathKeys": [
## "AP5->AP6"
## ]
## }
## ]
## 申请占用空间区域

## [
## {
        "unitId": "a-004-3",
        "owner": "AMB-06",
        "sceneId": "685A06C1373D9D2CFD46D1A4",
        "areaId": 0,
## "spatialZones":[
            {"points":[{"x":4.386,"y":-31.06},{"x":6.386,"y":-31.06},{"x":6.386,"y":-33.06},{"x":4.386,"y":-33.06}],"type":"Other"}
## ]
## }
## ]

## ## 不传 场景 区域
## [
## {
        "unitId": "a-004-327",
        "owner": "Box-02",
## "spatialZones":[
            {"points":[{"x":4.386,"y":-31.06},{"x":6.386,
            "y":-31.06},{"x":6.386,"y":-33.06},{"x":4.386,"y":-33.06}],"type":"Other"}
## ]
## }
## ]

## 响应示例
## 成功的响应示例

## {
## "ok": true
## }

## 失败的响应示例-没有失败的原因

## {
## "ok": false
## }

## 失败的响应示例-有失败的原因

## {
  "args": [],
  "code": "errExternalMapResOwnerBlank",
  "message": "owner 不能为空" // 失败的原因
## }

## 外部地图资源释放
通过 unitId 释放部分或者全部资源。

## URL
POST /api/fleet/external-map-res/release

## 请求报文
请求释放资源的 unitId 集合 类型为 List<String> 。

## 响应报文
无。

## 请求示例

## ["a-004-3"]

## 根据所有者释放外部地图资源
通过所有者来释放外部资源。

## URL
POST /api/fleet/external-map-res/release-by-owner

## 请求报文

## 名称

## 含义

## 类型

## 必填

## 说明

## owner

## 所有者

## String

## 必填

## 所有者，之前申请资源时用的所有者名称

## sceneId

## 场景 ID

## String

## 非必填

## sceneName

## 场景名称

## String

## 非必填

不使用 sceneId 时，此字段才生效。

## areaId

## 区域 ID

## Int

## 非必填

## areaName

## 区域名称

## String

## 非必填

不适用 areaId 时，此字段才生效。

## 响应报文
无。

## 请求示例
## 释放 AMB-06 已经申请的所有资源

## {
## "owner":"AMB-06"
## }

释放 AMB-06 已经申请的 A 场景（sceneId 是 A）中的所有资源

## {
    "owner":"AMB-06",
## "sceneId": "A"
## }

释放 AMB-06 已经申请的 B 场景（sceneName 是 B）中的所有资源

## {
    "owner":"AMB-06",
## "sceneName": "B"
## }

## 释放所有外部地图资源
释放之前申请的所有外部资源接口，慎用，若多个外部申请时，可能会误删除。

## URL
POST  /api/fleet/external-map-res/release-all

## 请求报文
无。

## 响应报文
无。

## 仿真
## （本次先不写）

## 设备
## （本次先不写）
