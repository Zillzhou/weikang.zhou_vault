# 根据Id获取Core报错信息

## 根据Id获取Core报错信息

## 方法说明
根据Id获取Core报错信息。
function getCoreAlarms(code:int):string;
## 输入参数
code，int类型，错误码。
## 输出参数
## {
## "errors": [{
                "code": 52201,
                "desc": "(AMB-01,chengpin-02, in BlockGroup102, whose max number is 1 ), The number of robots in same block group is over max number.",
                "times": 1,
### "timestamp": 1681110452
## }]
## }
当与调度断连时，本方法会返回 null。
## 异常
本方法不抛出异常。
