# Func06 写入保持寄存器

## Func06 写入保持寄存器
## 方法说明
Modbus 写入保持寄存器，功能码 06。
function writeHoldingRegister(ip: string, port: number, slaveId: number, offset: number, dataType: number, value: number): boolean

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
slaveId，number 类型，从机 slave ID。
offset，number 类型，Modbus 地址。
dataType，number 类型，2：无符号整形，3：有符号整形。
value，number 类型，写入的值。
## 输出参数
true，写入成功。
false，写入失败。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
