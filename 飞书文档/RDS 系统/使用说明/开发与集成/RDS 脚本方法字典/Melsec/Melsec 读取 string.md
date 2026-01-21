# Melsec 读取 string

## Melsec 读取 string
## 方法说明
Melsec 读取 string。
function readMelsecString(ip: string, port: string, address: string, length: number) : string | null

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
address，string 类型，读取的地址位。
length，number 类型，读取的长度。
## 输出参数
null，读取失败。
string，读取成功的返回值。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
