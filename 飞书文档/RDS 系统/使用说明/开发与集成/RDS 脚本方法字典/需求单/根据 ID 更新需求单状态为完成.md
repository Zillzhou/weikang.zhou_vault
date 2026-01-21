# 根据 ID 更新需求单状态为完成

## 根据 ID 更新需求单状态为完成
## 方法说明
根据 ID 将指定的需求单的状态，更新为完成。
function updateDemandFinishedById(demandId: string, supplementContent: string, handler: string): number

## 输入参数
demandId，string 类型，表示需求单的 ID。
supplementContent，string 类型，表示补充需求内容，为序列化之后的 JSON 数据。
handler，string 类型，表示处理人。
## 输出参数
成功时，返回值为 1。
失败时，返回值为 0。
## 异常
本方法会抛出异常。
