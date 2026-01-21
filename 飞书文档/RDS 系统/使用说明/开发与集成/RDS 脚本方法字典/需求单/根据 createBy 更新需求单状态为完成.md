# 根据 createBy 更新需求单状态为完成

### 根据 createBy 更新需求单状态为完成
## 方法说明
根据 createBy 将指定的需求单的状态，更新为完成。
function updateDemandFinishedByCreateBy(createBy: string, supplementContent: string, handler: string): number

## 输入参数
createBy，string 类型，表示当前需求单的创建者。
supplementContent，string 类型，表示补充需求内容，为序列化之后的 JSON 数据。
handler，string 类型，表示处理人。
## 输出参数
成功时，返回值为 1。
失败时，返回值为 0。
## 异常
本方法会抛出异常。
