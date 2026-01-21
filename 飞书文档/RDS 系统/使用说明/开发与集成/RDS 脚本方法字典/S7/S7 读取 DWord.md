# S7 读取 DWord

## S7 读取 DWord
## 方法说明
S7 读取 DWord。
function readS7DWord(type: string, ip: string, blockAndOffset: string): number;

## 输入参数
type，string 类型，PLC 类型，可选值（区分大小写）：S1200/S300/S400/S1500/S200Smart/S200。
ip，string 类型，PLC IP。
blockAndOffset，string 类型，读取的地址，支持的区域取值示例如下（区分大小写）： 

## 地址名称

## 地址代号

## 示例

## 中间寄存器

## M

## M100,M200

## 输入寄存器

## I

## I100,I200

## 输出寄存器

## Q

## Q100,Q200

## DB块寄存器

## DB

## DB1.100,DB1.200.7

## V寄存器

## V

## V100,V200

## 定时器的值

## T

## T100,T200

## 计数器的值

## C

## C100,C200

## 智能输入寄存器

## AI

## AI100,AI200

## 智能输出寄存器

## AQ

## AQ100,AQ200
## 输出参数
null，读取失败。
number，读取成功的返回值。
## 异常
本方法不抛出异常，异常捕获后只作日志记录。
