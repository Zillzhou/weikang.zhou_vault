# 获取Core报错信息

## 获取Core报错信息

## 方法说明
获取Core报错信息。 
function getCoreAlarms():string;
## 输入参数
无。
## 输出参数
## {
## "errors": [{
                "code": 52106,
                "desc": "(AMB-01: blocked by chengpin-02 in occupy path. blocked by chengpin-02 in block group's edge BlockGroup102)(AMB-02: blocked by AMB-05 in occupy path. blocked by AMB-05 in block group's edge BlockGroup94)(AMB-03: blocked by AMB-05 in block group's lm BlockGroup94)(AMB-05: blocked by AMB-03 in block group's lm BlockGroup105)(chengpin-02: blocked by AMB-01 in occupy path. blocked by AMB-01 in block group's edge BlockGroup102)",
                "times": 5,
### "timestamp": 1681110467
## }, {
                "code": 52201,
                "desc": "(AMB-01,chengpin-02, in BlockGroup102, whose max number is 1 ), The number of robots in same block group is over max number.",
                "times": 1,
### "timestamp": 1681110452
        }],
## "warnings": [{
                "code": 54000,
                "desc": "AMB-02,AMB-05,chengpin-03,chengpin-08, unable to reach park point.",
                "times": 1,
### "timestamp": 1681110462
## }]
## }
当与调度断连时，本方法会返回 null。
## 异常
本方法不抛出异常。
