# 查询子任务的任务id列表

## 查询子任务的任务id列表
## 查询子任务的任务id列表
## 方法说明
本方法是非阻塞方法，可根据父任务的id查询所有子任务的id。
function getChildrenTaskRecordId(recordId: string): string
## 输入参数
recordId，父任务的recordId。
## 输出参数
包含子任务id的 JSON 字符串，转换成 JSON 对象之后的示例数据如下所示：
## [
"child-58d549a4-6d4c-4e01-a327-269e3d4c1863",
"child-sdsdfvsd-qrwe-4e02-a327-269eqwcfs863"
## ]
        如果不存在满足条件的子任务，返回 "[]"。
## 异常
本方法会抛出异常。
