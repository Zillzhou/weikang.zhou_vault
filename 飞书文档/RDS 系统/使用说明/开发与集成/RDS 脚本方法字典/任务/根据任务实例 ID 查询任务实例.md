# 根据任务实例 ID 查询任务实例

## 根据任务实例 ID 查询任务实例
## 方法说明
根据任务实例 ID 查询任务数据。
function getTaskRecordById(taskRecordId: string): string

## 输入参数
taskRecordId，string 类型，是任务实例的唯一 ID。
## 输出参数
如存在指定 ID 的任务实例，返回 JSON 格式的字符串。
## {
## // 任务实例id
    "id":"123",
## // 天风任务id
    "defId":"6743",
## // 天风任务名
    defLabel:"agvMove",
    // 任务实例状态（1000：运行中，1001：终止，1002：暂停，1003：结束，1004：异常结束，1005：重启异常，1006：异常中断）
    "status":1000,
## // 任务创建时间
    "createdOn":"2022-02-12 11:23:12.0",
## // 结束原因
    "endedReason":"运行结束",
## // 任务结束时间
    "endedOn":"2022-02-12 11:26:12.0",
## // 任务状态描述
    "stateDescription":"物料搬运中",
## // 外部订单号
### "outOrderNo":"D6567812"
## }

如果不存在指定 ID 的任务实例，返回 “null”。
## 异常
本方法不抛出异常。
