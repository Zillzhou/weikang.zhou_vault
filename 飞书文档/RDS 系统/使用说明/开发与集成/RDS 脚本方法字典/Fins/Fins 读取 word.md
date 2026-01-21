# Fins 读取 word

## Fins 读取 word
## 方法说明
Fins 读取 word。
function readFinsWord(ip: string, port: number, area: number, finsIoAddr: number, bitOffset: number) : number

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
area，number 类型，读取的存储区域代码，16进制值如：（0x82） 。
finsIoAddr，number 类型，读取的地址位 。
bitOffset，number 类型，读取的偏移量 。
## 输出参数
null，读取失败。
number，读取成功的返回值。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
