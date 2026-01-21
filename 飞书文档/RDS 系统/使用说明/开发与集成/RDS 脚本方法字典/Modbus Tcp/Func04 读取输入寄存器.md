# Func04 读取输入寄存器

## Func04 读取输入寄存器
## 方法说明
Modbus 读取只读输入寄存器，功能码 04。
function readInputRegister(ip: string, port: number, slaveId: number, offset: number, dataType: number): number | null

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
slaveId，number 类型，从机 slave ID。
offset，number 类型，Modbus 地址。
dataType，number 类型，2：无符号整形，3：有符号整形。
## 输出参数
null，读取失败。
number，读取成功的返回值。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
