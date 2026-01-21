# SRC 控制器任务下发的 JSON 示例

### SRC 控制器任务下发的 JSON 示例
      为了方便大家使用 SRC 控制器造车之后，能够快速使用控制器的路径导航功能验证车体的路径导航功能，特别是对一些做移动机器人 RCS 调度的厂商，需要知道控制器的导航任务是如何进行下发的，该文档从通用任务下发，不同车型任务下发进行说明（通用任务适用于所有移动机器人，不针对车辆类型，不同类型车体会有特殊字段以适应不同功能类型车体的动作要求）
## 通用任务
### 通用任务指令中的关键字，可以在带动作的任务指令中使用
## 导航到目标点(API 编号：3051)
## {
    "angle": 0.0, // 目标点角度，单位 rad， 可选
    "max_speed": 1.0, // 最后一段线路的速度，单位m/s, 可选
    "max_acc": 1.0，// 最后一段线路的最大加速度速度，单位m/s^2, 可选
    "max_dec": 1.0, // 最后一段线路的最大减速度速度，单位m/s^2, 可选
    "reach_angle": 0.01, // 到点角度精度，单位 rad, 可选
    "reach_dist": 0.005, // 到点线路距离精度，单位 m, 可选
    "max_wspeed": 1.0， // 最后一段线路上的最大角速度，单位 rad/s， 可选
    "max_wacc": 0.1, // 最后一段线路的最大角加速度，单位 rad/s^2, 可选
    "max_wdec": 0.1, // 最后一段线路的最大角加速度，单位 rad/s^2, 可选
    "method": "backward", // 正走(forward)和倒走(backward), 可选
    "orientation": 90.0， // 全向车导航时角度在地图坐标下的角度，单位 deg, 可选
    "hold_dir": 90.0， // 全向车导航时角度在地图坐标下的角度，单位 deg, 可选
    "goods_dir": 90.0,  // 最后一段线路的速度，带随动电机agv，货架在地图坐标系下的角度，单位 deg， 可选
    "start_rot_dir": 1.0,  // 最后一段线路上，起步原地旋转朝向 -1: right, 1: left, 0: 就近原则，可选
    "end_rot_dir": 1.0, // 最后一段线路上，终点原地旋转朝向 -1: right, 1: left, 0: 就近原则，可选
    "forbiddenRotAngle": 30.0,// 最后一段线路上, 禁止旋转通过的角度，可选
    "disable_virtual_laser": false, // 最后一段线路上，禁止虚拟激光避障, 可选
    "disable_depth_laser_id": false,  // 最后一段线路上，被禁用3D摄像头的id， 可选
    "disable_laser_id": false,// 最后一段线路上，被禁用激光的id， 可选
    "disable_infrared_id": false,// 最后一段线路上，被禁用红外的id， 可选    
    "disable_fallingDown_id": false,// 最后一段线路上，被禁用防跌落传感器的id， 可选    
    "disable_collision_id": false,// 最后一段线路上，被禁用防跌落传感器的id， 可选    
    "disable_ultrasonic_id": false, // 最后一段线路上，被禁用超声的id， 可选
    "disable_distanceNode_id": false, // 最后一段线路上，被禁用距离传感器的id， 可选
    "duration": 1000.0, // 任务结束前，原地等待的时间，单位 ms，可选
    "delay": 1000.0, // 任务结束前，原地等待的时间，单位 ms，可选
    "spin": false, // 对于含有随动电机的agv 是否开启随动功能，可选
    "path_width": 1.0, // 线路宽度，当配置有线路宽度，并且非识别，非离开充电的任务下，当有障碍物会绕行，可选
    "allow_free_go": false, // 最后一段线路的任务,如果有路宽的情况下，是否开启绕行功能， 可选
    "tcp":"TCP1", // 最后一段线路的任务，指定特定的TCP参数
    "reachTargetDI": [1,2], // 到位DI， 可以是json array， 也可以是单个uint数字, 可选
    "skill_name": "GotoSpecifiedPos", // 最后一段线路的任务名称， 可选
    "id": "AP1", // 导航任务的目标点id， 当 id 为 SELF_POSITION 时，表示原地任务
## }
## 指定线路导航(API 编号：3066)
## {
    "angle": 0.0, // 目标点角度，单位 rad， 可选
    "max_speed": 1.0, // 指定线路的速度，单位m/s, 可选
    "max_acc": 1.0，// 指定线路的最大加速度速度，单位m/s^2, 可选
    "max_dec": 1.0, // 指定线路的最大减速度速度，单位m/s^2, 可选
    "reach_angle": 0.01, // 到点角度精度，单位 rad, 可选
    "reach_dist": 0.005, // 到点线路距离精度，单位 m, 可选
    "max_wspeed": 1.0， // 指定线路上的最大角速度，单位 rad/s， 可选
    "max_wacc": 0.1, // 指定线路的最大角加速度，单位 rad/s^2, 可选
    "max_wdec": 0.1, // 指定线路的最大角加速度，单位 rad/s^2, 可选
    "method": "backward", // 正走(forward)和倒走(backward), 可选
    "orientation": 90.0， // 全向车导航时角度在地图坐标下的角度，单位 deg, 可选
    "hold_dir": 90.0， // 全向车导航时角度在地图坐标下的角度，单位 deg, 可选
    "goods_dir": 90.0,  // 指定线路的速度，带随动电机agv，货架在地图坐标系下的角度，单位 deg， 可选
    "start_rot_dir": 1.0,  // 指定线路上，起步原地旋转朝向 -1: right, 1: left, 0: 就近原则，可选
    "end_rot_dir": 1.0, // 指定线路上，终点原地旋转朝向 -1: right, 1: left, 0: 就近原则，可选
    "disable_virtual_laser": false, // 指定线路上，禁止虚拟激光避障, 可选
    "disable_depth_laser_id": false,  // 指定线路上，被禁用3D摄像头的id， 可选
    "disable_laser_id": false,// 指定线路上，被禁用激光的id， 可选
    "disable_infrared_id": false,// 指定线路上，被禁用红外的id， 可选    
    "disable_fallingDown_id": false,// 指定线路上，被禁用防跌落传感器的id， 可选    
    "disable_collision_id": false,// 指定线路上，被禁用防跌落传感器的id， 可选    
    "disable_ultrasonic_id": false, // 指定线路上，被禁用超声的id， 可选
    "disable_distanceNode_id": false, // 指定线路上，被禁用距离传感器的id， 可选
    "duration": 1000.0, // 任务结束前，原地等待的时间，单位 ms，可选
    "delay": 1000.0, // 任务结束前，原地等待的时间，单位 ms，可选
    "spin": false, // 对于含有随动电机的agv 是否开启随动功能，可选
    "path_width": 1.0, // 线路宽度，当配置有线路宽度，并且非识别，非离开充电的任务下，当有障碍物会绕行，可选
    "allow_free_go": false, // 指定线路的任务,如果有路宽的情况下，是否开启绕行功能， 可选
    "reachTargetDI": [1,2], // 到位DI， 可以是json array， 也可以是单个uint数字, 可选
### "tcp":"TCP1", // 指定特定的TCP参数
    "skill_name": "GotoSpecifiedPos", // 指定线路的任务名称， 可选
    "percentage": 1.0, // 导航终点所在线路的百分比，需要在 0~1 范围内
    "id": "AP1", // 指定线路的目标点id, 当 id 为 SELF_POSITION 时，表示原地任务
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }

## 叉车任务
## 抬升货叉任务
## {
    "operation": "ForkHeight", // 抬升货叉
    "start_height": 0.0, // 导航前的货叉抬升高度，可选
    "fork_mid_height": 0.5, // 导航过程中的货叉抬升高度，可选
    "end_height": 1.0, // 导航到点的货叉抬升高度，可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }
## 非识别取货任务

## {
    "operation": "ForkLoad", // 取货任务
    "start_height": 0.0, // 导航前的货叉抬升高度，可选
    "fork_mid_height": 0.5, // 导航过程中的货叉抬升高度，可选
    "end_height": 1.0, // 导航到点的货叉抬升高度，可选
    "min_safe_height": 0.3,  // 货叉下降小于此高度，后视激光被触发时，会触发阻挡报错，单位 m。 可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }

## 非识别放货任务
## {
    "operation": "ForkUnload", // 抬升货叉
    "start_height": 0.0, // 导航前的货叉抬升高度，可选
    "fork_mid_height": 0.5, // 导航过程中的货叉抬升高度，可选
    "end_height": 1.0, // 导航到点的货叉抬升高度，可选
    "min_safe_height": 0.3,  // 货叉下降小于此高度，后视激光被触发时，会触发阻挡报错，单位 m。 可选
    "max_down_height": 0.1,  // 叉车放货时，超声 DI 触发时的最大下降距离，m。需要大于0。可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }

## 识别取货任务
## {
    "operation": "ForkLoad", // 取货任务
    "recfile": "pallet/p0001.pallet", // 识别文件
### "recognize": true, // 识别
    "start_height": 0.0, // 导航前的货叉抬升高度，可选
    "end_height": 1.0, // 导航到点的货叉抬升高度，可选
    "min_safe_height": 0.3,  // 货叉下降小于此高度，后视激光被触发时，会触发阻挡报错，单位 m。 可选
    "rec_height": 1.0, // 叉车识别后，货叉抬升的高度， 单位m。 如果小于0，会被忽略。可选
    "rec_back_dist": 1.0, // 叉车识别栈板的后退距离，单位m， 可选
    "rec_min_ahead_dist": 1.0, //  叉车识别栈板的最小前置距离，单位m， 可选
    "rec_ahead_dist": 1.0, //  叉车识别栈板的前置距离最大值，单位m, 可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }

## 识别放货任务
## {
    "operation": "ForkUnload", // 取货任务
    "recfile": "pallet/p0001.pallet", // 识别文件
### "recognize": true, // 识别
    "start_height": 0.0, // 导航前的货叉抬升高度，可选
    "fork_mid_height": 0.5, // 导航过程中的货叉抬升高度，可选
    "end_height": 1.0, // 导航到点的货叉抬升高度，可选
    "min_safe_height": 0.3,  // 货叉下降小于此高度，后视激光被触发时，会触发阻挡报错，单位 m。 可选
    "max_down_height": 0.1,  // 叉车放货时，超声 DI 触发时的最大下降距离，m。需要大于0， 可选
    "rec_fail_go": false, // 叉车，如果识别失败，可以直接去往目标点，一般用于叉车卸货时，要先识别目标点是否有货物， 可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }

## 识别货物高度放货
## {
    "operation": "ForkUnload", // 取货任务
    "recInitHeight": true,
    "obs_width": 1.0,// 叉车放货前识别目标点的矩形内是否有激光点，矩形的宽度，m。需要大于0
    "obs_length": 1.0,// 叉车放货前识别目标点的矩形内是否有激光点，矩形的宽度，m。需要大于0
    "start_height": 0.0, // 导航前的货叉抬升高度，可选
    "fork_mid_height": 0.5, // 导航过程中的货叉抬升高度，可选
    "end_height": 1.0, // 导航到点的货叉抬升高度，可选
    "min_safe_height": 0.3,  // 货叉下降小于此高度，后视激光被触发时，会触发阻挡报错，单位 m。 可选
    "max_down_height": 0.1,  // 叉车放货时，超声 DI 触发时的最大下降距离，m。需要大于0， 可选
    "recAdjust": true, // 识别前是否要调整车体朝向， 可选
    "rec_dir": 0.0, // 车子识别前的朝向， 单位deg, 可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }

## 潜伏顶升车任务
## 导航前调托盘
## {
    "spinZeroFirstly": true, // 表示做动作前要将随动电机强制转到0度先
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id 
## }

## 抬升顶升任务
## {
    "operation": "JackHeight", // 抬升顶升
    "jack_height": 0.0, // 顶升高度，单位 m
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
## }

## 非识别取货任务
## {
    "operation": "JackLoad", // 抬升顶升
    "jack_adjust_dist": 0.1, //顶升车去load时为了触发对位DI调整范围，单位m, 可选
    "jack_adjust_vel": 0.1, //顶升车去load时为了触发对位DI，调整范围内的速度，单位m/s， 可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id    
## }

## 非识别放货任务
## {
    "operation": "JackUnload", // 抬升顶升
    "jack_rot": 1.57, // 顶升车到点后，先原地旋转特定角度，再做顶升动作，单位 deg, 可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id    
## }a

## 识别取货任务
## {
    "operation": "JackLoad", // 抬升顶升
    "recfile": "shelf/p0001.shelf", // 识别文件
### "recognize": true, // 识别
    "recAdjust": true, // 前置点调整角度开， 可选
    "jack_rot": 1.57 // 顶升车到点后，先原地旋转特定角度，再做顶升动作，单位 rad, 可选
    "jack_adjust_dist": 0.1, // 顶升车去load时为了触发对位DI调整范围，单位m, 可选
    "jack_adjust_vel": 0.1, // 顶升车去load时为了触发对位DI，调整范围内的速度，单位m/s， 可选
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id    
## }

## 识别取货前识别库位是否有货任务
## {
    "operation": "JackLoad", // 取货
    "preRecFile": "shelf/p0001.shelf", // 识别文件
    "recfile":"shelf/s0002.shelf", // 识别文件
### "recognize": true, // 识别
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id    
## }
## 识别放货前识别库位是否有货任务
## {
    "operation": "JackUnload", // 放货
    "preRecFile": "shelf/p0001.shelf", // 识别文件
    "recfile":"shelf/s0002.shelf", // 识别文件
### "recognize": true, // 识别
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id    
## }
## 全向车，识别从货架下钻出来
## {
    "recGoOut": true, // 识别从库位中走出来
    "recfile": "shelf/p0001.shelf", // 识别文件
### "recognize": true, // 识别
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id    
## }

## 原地任务
## 打开上视 pgv 的照明灯
## {
    "id":"SELF_POSITION",
## "OpenBlinkDO": true
## }
## PGV 二次调整
## PGV 二次调整和 二维码对齐
## {
    "use_down_pgv":true,   //启动下视pgv进行调整, 和 use_pgv 二选一
    "use_pgv": true,     // 启用上视pgv进行调整，和 use_down_pgv 二选一
    "pgvDx":0.01,           //修改PGV的x偏移量，单位m，可选
    "pgvDy":0.01,           //修改PGV的y偏移量，单位m， 可选
    "pgvDtheta":90,        //修改PGV的角度偏移量，单位度，可选
    "pgv_adjust_180": true, // 二次调整角度，允许机器人和二维码保持180度， 可选
    "pgv_adjust_90": true, // 二次调整角度，允许机器人和二维码保持90，-90，0，180度
    "pgv_omni_diff_dir":90.0， // 指定双舵轮车的前进方向，单位为度，是车体坐标下的角度，作为行驶的正方向。
    "pgv_x_adjust": true,  //只对沿着车子方向的偏差进行调整， 可选
    "pgv_x_angle_adjust": true,  //沿着车子方向的偏差进行调整，并且到点后调整角度偏差， 可选
    "pgv_adjust_dist": 0.3,  //最大的调整半径，单位m, 可选
    "pgv_adjust_cx": -0.3,    //调整范围的圆心偏移量, 单位m，可选
    "pgv_adjust_cy": 0， //调整范围的圆心偏移量, 单位m，可选
    "operation":"GoPGV", // 可选，如果是原地任务，则需要必填
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id   
## }
## 顶升任务与二次调整联合任务
## {
    "use_down_pgv":true,   //启动下视pgv进行调整, 和 use_pgv 二选一
    "use_pgv": true,     // 启用上视pgv进行调整，和 use_down_pgv 二选一
    "pgvDx":0.01,           //修改下视PGV的x偏移量，单位m，可选，如果是上视用 upPgvDx
    "pgvDy":0.01,           //修改下视PGV的y偏移量，单位m， 可选,如果是上视用 upPgvDy
    "pgvDtheta":90,        //修改下视PGV的角度偏移量，单位度，可选，如果是上视用 upPgvDtheta
    "pgv_adjust_180": true, // 二次调整角度，允许机器人和二维码保持180度， 可选
    "pgv_adjust_90": true, // 二次调整角度，允许机器人和二维码保持90，-90，0，180度
    "pgv_omni_diff_dir":90.0， // 指定双舵轮车的前进方向，单位为度，是车体坐标下的角度，作为行驶的正方向。
    "pgv_x_adjust": true,  //只对沿着车子方向的偏差进行调整， 可选
    "pgv_x_angle_adjust": true,  //沿着车子方向的偏差进行调整，并且到点后调整角度偏差， 可选
    "pgv_adjust_dist": 0.3,  //最大的调整半径，单位m, 可选
    "pgv_adjust_cx": -0.3,    //调整范围的圆心偏移量, 单位m，可选
    "pgv_adjust_cy": 0， //调整范围的圆心偏移量, 单位m，可选
    "operation":"GoPGV", // 可选，如果是原地任务，则需要必填
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id   
    "operation": "JackLoad", // 抬升顶升
## }
## 随动托盘控制
## 随动
## 不指定托盘方向
## {
    "id": "AP1", // 指定线路的目标点id, 当 id 为 SELF_POSITION 时，表示原地任务
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
    "spin":true,
### "goods_dir": 0 //货物朝向，单位角度
## }
## 指定托盘方向
## {
    "id": "AP1", // 指定线路的目标点id, 当 id 为 SELF_POSITION 时，表示原地任务
    "source_id": "AP2", //  指定线路的起点id
    "task_id": "uuid", //  任务的唯一 id
### "goods_dir": 0 //货物朝向，单位角度
## }

## 转托盘
## {
    "skill_name": "GoByOdometer",
    "increase_spin_angle":1.57, // 按增量式旋转托盘时，托盘的角度，单位rad， 可选
    "robot_spin_angle":-1.57, // a，单位rad， 可选
    "global_spin_angle":-1.57, // 旋转托盘到世界坐标下的角度，单位rad， 可选
    "spin_direction":0.0 // 转动托盘的方向，1 表示逆时针旋转，-1表示顺时针旋转，其他就近方向旋转, 可选
## }
## 绕行
## 绕行去指定目标坐标
## {
    "id":"SELF_POSITION",
## "freeGo":{
## "x": 1.0, // 坐标，单位m
## "y": 1.0,// 坐标，单位m
### "theta": 1.57// 坐标，单位m
## }
## }
## 绕行去指定站点
## {
    "id":"SELF_POSITION",
## "freeGo":{
### "id":"LM2" // 坐标，单位m
## }
## }
## 在指定绕行区域，绕行
## {
  "id": "LM1", 
  "source_id":"LM2", 
## "avoid":
## {
## "target":{
## "x":0.0, // 坐标，单位m
## "y":0.0, // 坐标，单位m
### "theta":0.0 // 坐标，单位m
    },
## "circle":{
### "radius":1.0, // 坐标，单位m
## "x":0.0, // 坐标，单位m
## "y":0.0 // 坐标，单位m
## }
  }, 
## "task_id":"1111"
## }
## 开环指令
## 平动
## {
  "skill_name": "GoByOdometer",
### "move_dist": 1.0, //平移距离， 可选
### "speed_x": 1.0 //最大x方向速度， 可选
### "speed_y": 1.0 //最大y方向速度， 可选
### "maxAcc": 1.0 // m/s^2， 可选
### "maxDec": 1.0 // m/s^2， 可选
### "jerkAcc": 1.0 // m/s^3，可选
## }
## 原地旋转
## {
  "skill_name": "GoByOdometer",
  "move_angle": 1.57, //旋转弧度， 可选
  "speed_w": 1.57, //弧度每秒，最大角速度，可选
  "maxRotAcc": 1.57, // rad/s^2， 可选
  "maxRotDec": 1.57, // rad/s^2，可选
### "jerkRot": 1.57 //rad/s^3， 可选
## }
## 走弧线
## {
  "skill_name": "GoByOdometer",
### "rot_radius": 1.0, //m，圆弧半径
### "rot_degree": 90.0 //度，圆弧对应角度
### "rot_speed": 1.0 //m/s, 最大线速度
### "maxAcc": 1.0 // m/s^2，可选
### "maxDec": 1.0 // m/s^2，可选
### "jerkAcc": 1.0 // m/s^3， 可选
## }

## 其他
## 两段线路去目标点
## {
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id   
    "plan_method":"twoLine", //两条线段方法去前置点
    "plan_loc":"odo", // 基于里程定位回前置点， 可选
    "line1_dist":1 // 第一条线段的长度， 单位 m
## }
## QuickGo
## {
    "id":"SELF_POSITION",
    "recfile": "pallet/p0001.pallet", //所叉取栈板的识别文件名
                "start_height": 0.1, //起始货叉高度 单位：m
    "end_height": 0.5, //抬升货物的高度 单位：m
    "back_height":0.4, //返回前置点后的货叉高度 单位：m
### "operation":"QuickGo"
## }
## 去库位执行 binTask
## {
  "id":"DK-4-4-1",
## "binTask":"load"
## }
## 禁用线路
{"disablePath":{"id":"LM1-LM2"}}
## 启用线路
{"enablePath":{"id":"LM1-LM2"}}
## 执行脚本任务
## {
  "setup_script": "syspy/setDO.py", // 前置脚本名称，可选
  "setup_script_args": {            // 前置脚本参数，可选
    "DO":[{"id":1,"status": true}]
  },
  "script_stage": 0, // 0:导航前，1：导航中，2：导航后，3， 脚本控制导航，不写默认为2
  "script_name": "*.py", // 脚本名称，可选
  "script_args": {},     // 脚本参数，可选
  "post_script": "syspy/setDO.py", // 后置脚本，可选
  "post_script_args": {  // 后置脚本参数，可选
    "DO":[{"id":2,"status": true}]
  },
    "operation":"Script",
### "id": "AP1", // 指定线路的目标点id
    "source_id": "AP2", // 指定线路的起点id
    "task_id": "uuid", // 任务的唯一 id   
## }
