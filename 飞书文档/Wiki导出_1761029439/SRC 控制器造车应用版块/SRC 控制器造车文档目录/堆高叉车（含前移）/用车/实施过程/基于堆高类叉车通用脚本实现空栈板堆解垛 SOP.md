# 基于堆高类叉车通用脚本实现空栈板堆解垛 SOP

### 基于堆高类叉车通用脚本实现空栈板堆解垛 SOP
## 更新记录：

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.29

## 更新发布

## 使用中

## 车型
仙工智能标准车型 CDD14 堆高叉车。对于堆解垛场景推荐使用车型为平衡重叉车或者前移叉车，此处为了方便测试，临时选用堆高叉车。
## 传感器方案
## 主要传感器如下
## 导航：单线激光*1，倍加福R2000
## 避障：单线激光*2，欧镭LR1BS
## 识别：深度相机*1，图漾FM851
## 功能流程
## 解垛流程

## 图 3.1 解垛流程
## 堆垛流程

## 图 3.2 堆垛流程
## 识别文件配置
## 取货/解垛识别文件配置参考
## image.png

## 图 4.1 取货/解垛识别文件
## 示例识别文件：

## unstack.plt
## 堆垛识别文件配置参考
## image.png

## 图 4.2 堆垛识别文件
## 示例识别文件：

## stack.plt
## 地图示例
## image.png

## 图 5.1 测试地图示例
## 脚本使用说明
## 脚本获取说明
## 机构脚本使用通识介绍
项目UUID：be638687-15e5-4c25-b9fe-e0479a42e62f
## 脚本参数说明
## 以下参数根据实际情况配置：
## image.png

## image.png

## image.png

### "omniModel": "底盘类型是否为全向omni"
### "liftMotor": "升降电机名称"
### "liftZero":  "货叉升降机构的零位"
### "liftVel": "lift电机的规划最大速度"
"liftMaxHeight": "lift电机的最大高度"
### "stretchMotor": "前后移电机名称"
### "stretchZero": "伸出机构的零位"
### "stretchZeroDi": "货叉前移零位DI"
### "stretchMaxDi":  "货叉前移极限DI"
"stretchVel": "stretch电机的规划最大速度"
"stretchMaxLength": "货叉伸出最大距离"
"stretchType": "叉尺前后伸展电机控制类型，速度控制speed或位置控制position"
### "pitchMotor": "俯仰电机"
### "pitchVel": "pitch电机的规划最大速度"
### "pitchZeroDi": "货叉俯仰零位DI"
### "pitchMaxDi": "货叉俯仰极限DI"
### "reachDI1": "货物到位DI1的ID"
### "reachDI2": "货物到位DI2的ID"
### "checkAllDi": "是否检测全部货物到位DI"
"distanceNodeId1": "distanceNode1的ID号"
"distanceNodeId2": "distanceNode2的ID号"
"ObsStopDist": "# 报警距离， 这个距离传感器的死区为0.2m，因此不能配置成小于0.2m"
### "backLaserId1": "后置激光1id"
### "backLaserId2":"后置激光2id"
"AheadDist": "双折线识别调整时，调整距离不够时，第二段折线长度"
"minAheadDist":  "识别调整偏差后进栈板前，里程中心在栈板前的直线距离"
## 平衡重

"backDist": "识别调整结束时，里程中心在栈板后的直线距离，不进入栈板为负"
"adjustForStr": "识别调整不足时，前进距离"
"recBeizer": "识别调整时是否使用贝塞尔曲线行驶，beizer、straight"
"beizerDist":  "识别后贝塞尔曲线调整时是否需要先向前行驶的最小调整距离，机器人当前位置与栈板前置点（minAheadDist）之间的距离"
"reachDist": "识别后调整，检测货物到位DI时的位置范围"
"reachAngle": "识别后调整，检测货物到位DI时的角度范围，单位是° "
"liftUpHeight":  "取货时取到货后抬升高度，单位是m "
"liftDownHeight": "放货时到点后下降高度，单位是m "
"readjust": "是否开启二次识别调整取货，以提高取货精度 "
### "adjustMaxTimes": "二次识别调整次数 "
### "distPrecision":  "识别调整位置精度"
### "anglePrecision": "识别调整角度精度"
"readjustForwardDist": "二次识别调整前进距离"
### "classifyRang": "解垛分类宽度差范围"
### "errorRang": "解垛类别判断宽度差范围"
"palletNormalCount": "解垛最大栈板层数"
"palletCheck":  "解垛过程是否检查栈板类型及数量"
### "weightGoodOn": "是否启用称重检测"
"maxWeightDetectTimes": "重量检测最大次数"
"detectPeriodTime": "重量检测时间间隔"
## "maxWeight": "重量上限"
## 任务参数说明
### "adjustHeight": 是否根据识别结果调整高度
## "checkDi": 是否检测到位DI
### "endHeight": 结束时货叉高度
"forkMidHeight":边走边升降行走过程中的货叉高度
### "forkMoveMode": 边走边升降货叉任务结束模式
"forwardDist": 机器人整体前进距离，沿X正方向
### "goodsCheckOn": 是否启用货物有无及超限检测
"liftDownHeight": 放货时 在liftHeight基础上，再下降此距离
"liftUpHeight": 取货类操作会在startHeight基础上再上升此距离
"loadAll": 取货过程中是否取所有货物，false为取解垛时使用
"loadMoveHeight": 完成取货后行走前调整到的高度，即叉车行走高度
"loadMoveHeightOn":取货流程完成后是否调整货叉高度至安全高度
### "loadPitch":取放货过程中是否调整货叉俯仰
### "locName":库位货物有无检测的库位名称
## "massageName":消息名称
## "operation":动作类型
"pitchMode":货叉俯仰类型，forward前倾、backwards后仰
"readjust":是否开启二次识别调整取货，以提高取货精度
## "recFile": 识别文件
## "recognize":是否识别
"startHeight": 开始时货叉高度，如需识别，则在识别前前调整货叉高度，会在此高度进行识别
## "stationList": 站点列表
"stepHeight": 货叉步进升降调整高度值，正值为上升，负值为下降
"stepLength": 货叉步进伸缩调整高度值，正值为伸出，负值为收回
### "stretchLength":货叉伸出长度
### "stretchMode": 货叉前移或收回
### "unStackNum": 每次解垛叉取的空栈板数量
## 任务连测试
## 解垛任务参数
## {
    "operation": "Script",
    "recognize": true,
## "script_args": {
        "adjustHeight": true,
        "checkDi": true,
        "forwardDist": 1.35,
        "liftUpHeight": 0.4,
        "loadAll": false,
        "loadMoveHeightOn": false,
        "operation": "load",
        "readjust": true,
        "recFile": "plt/unstack.plt",
        "startHeight": 0.084,
## "unStackNum": 1
    },
    "script_name": "forkGeneral.py",
## "script_stage": 3
## }
## 解垛后边走边升降至前置点任务参数
## {
    "id": "LM12",
    "operation": "Script",
## "script_args": {
        "forkMidHeight": 0.2,
        "forkMoveMode": 6,
        "operation": "goStationWithFork"
    },
    "script_name": "forkGeneral.py",
## "script_stage": 3
## }

## 堆垛任务参数
## {
    "operation": "Script",
## "script_args": {
        "adjustHeight": true,
        "checkDi": false,
        "endHeight": 0.25,
        "forwardDist": 1.2,
        "loadMoveHeightOn": false,
        "operation": "unload",
        "readjust": true,
        "recFile": "plt/stack.plt",
        "recognize": true,
### "startHeight": 0.084
    },
    "script_name": "forkGeneral.py",
## "script_stage": 3
## }

## 示例任务链文件

## stack.task
## RDS调度测试（待补充）
## 实测视频

## 空栈板堆解垛.mp4
## 视频 9.1 实测视频
