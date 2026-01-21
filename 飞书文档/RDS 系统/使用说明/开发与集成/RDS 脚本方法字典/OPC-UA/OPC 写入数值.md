# OPC 写入数值

## OPC 写入数值
## 方法说明
读取 OPC server 中的某个地址位的值。
function writeOpcValue(address: string, value: any): boolean

## 输入参数
address，string 类型，表示写入的地址。
value，any 类型， 写入的值。
## 输出参数
成功返回 true，失败返回 false。
## 异常
本方法不抛出异常。
