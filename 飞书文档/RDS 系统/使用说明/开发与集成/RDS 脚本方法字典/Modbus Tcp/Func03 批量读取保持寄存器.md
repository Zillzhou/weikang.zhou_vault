# Func03 批量读取保持寄存器

## Func03 批量读取保持寄存器
## 方法说明
Modbus 读取保持寄存器多个连续地址值，并以数组的形式返回其中的值，功能码 03。
function batchReadHoldingRegisters(ip: string, port: number, slaveId: number, offset: number, len: number): Array<number> | null

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
slaveId，number 类型，从机 slave ID。
offset，number 类型，Modbus 首地址。
len，number 类型，Modbus 地址数量。
## 输出参数
null，读取失败。
Array，读取成功后，返回结果数组。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
