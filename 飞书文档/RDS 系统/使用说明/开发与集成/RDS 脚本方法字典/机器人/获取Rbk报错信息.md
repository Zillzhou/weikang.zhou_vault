# 获取Rbk报错信息

## 获取Rbk报错信息

## 方法说明
获取Rbk报错信息。
function getRbkAlarms():string;
## 输入参数
无。
## 输出参数
## {
## "errors": [{
                "code": 52200,
                "desc": "Blocked by : chengpin-02",
                "times": 1,
### "timestamp": 1681116822857
## }, {
                "code": 52200,
                "desc": "Blocked by : AMB-01",
                "times": 1,
### "timestamp": 1681116822858
## }]
## }
当与调度断连时，本方法会返回 null。
## 异常
本方法不抛出异常。
