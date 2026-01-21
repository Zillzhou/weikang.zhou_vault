# Melsec 写入 string

## Melsec 写入 string
## 方法说明
Melsec 写入 string。
function writeMelsecString(ip: string, port: string, address: string, value: string) : void

## 输入参数
ip，string 类型，从机 IP。
port，number 类型，从机端口。
address，string 类型，写入的地址位。
value，string 类型，写入的值。
## 输出参数
成功返回 true，失败返回 false。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
