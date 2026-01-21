# Func10 批量写入保持寄存器

## Func10 批量写入保持寄存器
## 方法说明
Modbus 写入保持寄存器多个连续地址，功能码 10。
function batchWriteHoldingRegister(ip: string, port: number, slaveId: number, offset: number, value: Array<number>): boolean

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
slaveId，number 类型，从机 slave ID。
offset，number 类型，Modbus 地址。
value，number 数组类型，多个需写入的值。
## 输出参数
true，写入成功。
false，写入失败。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
