# Fins 写入 bit

## Fins 写入 bit
## 方法说明
Melsec 写入 string。
function writeFinsBit(ip: string, port: number, area: number, finsIoAddr: number, bitOffset: number, value: boolean) : void

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
area，number 类型，写入的存储区域代码，16进制值如：（0x82） 。
finsIoAddr，number 类型，写入的地址位 。
bitOffset，number 类型，写入的偏移量 。
value，boolean类型，写入的值。
## 输出参数
该方法无输出参数。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
