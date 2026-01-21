# OPC 读取数值

## OPC 读取数值
## 方法说明
读取 OPC server 中的某个地址位的值。
function readOpcValue(address: string): any

## 输入参数
address，string 类型，读取的地址位。
## 输出参数
读到的值。
## 异常
读取失败会抛出 RuntimeException 异常 “readOpcValue error”。
