# Melsec 读取 number

## Melsec 读取 number
## 方法说明
Melsec 读取 number。
function readMelsecNumber(ip: string, port: string, address: string): number | null

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
address，string 类型，读取的地址位。
## 输出参数
null，读取失败。
number，读取成功的返回值。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
