# Func02 读取线圈量

## Func02 读取线圈量
## 方法说明
Modbus 读取线圈量，功能码 02。
function readInputStatus(ip: string, port: number, slaveId: number, offset: number): boolean | null

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
slaveId，number 类型，从机 slave ID。
offset，number 类型，Modbus 地址。
## 输出参数
null，读取失败。
boolean，读取成功的返回值。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
