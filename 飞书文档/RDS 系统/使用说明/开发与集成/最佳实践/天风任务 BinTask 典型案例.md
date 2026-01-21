# 天风任务 BinTask 典型案例

## 天风任务 BinTask 典型案例
## 1、 令机器人执行 binTask 动作
首先对机器人通用动作做简要介绍。
### 点击【C通用机器人】拖到箭头所指方向，如图所示：
1689820247759-2aa4f000-caf4-41bb-a60f-1387e38d3b76.png

### 【指令】参数提供一个下拉菜单，可以选择机器人的动作：
1689820314762-a11853b7-fc24-41de-9199-729fd2586f79.png

【无】指定机器人不做任何动作。
【binTask】当机器人到达最后一段线路时，会依据 binTask 对应键中的 JSON，执行对应动作。
【preBinTask】当机器人到达最后一段线路前，会依据 binTask 对应键中的 JSON，执行对应动作。
【顶升JackLoad】将顶升机构顶起。支持顶升机器人的料架识别，机器人到达指定的目标站点会上升顶升机构（Roboshop 会显示载货中）。
【设置顶升高度JackHeight】设置顶升车高度，不改变顶升状态。
【顶降JackUnload】将顶升降下。支持顶升机器人的识别动作，当机器人到达目标站点会放下顶升机构。
【等待Wait】即移动机器人到达指定的目标站点，不做任何动作。
【叉车取货ForkLoad】货叉升起。支持叉车的识别动作。
【降低叉齿ForkUnload】货叉降下。支持叉车的识别动作。
【边走边升降叉齿ForkHeight】设置货叉高度，不改变货叉状态
【自定义指令CustomCommand】可以在RDS界面自定义指令operation或者script。
【ctuNoBlock.py】料箱机器人执行动作的脚本。
### 【syspy/setDO.py】 控制多个DO的开关
【syspy/waitDI.py】让车等待某些 DI 达到某种状态，或者达到超时条件，则算完成任务。

以上动作中，binTask 最为特殊。对 binTask 类型动作，除了动作名称外，所有的动作参数都无需指定，都可在 Core 层配置和处理。
我们以叉车取货和卸货为例。首先，连接core，进入core，点击【开始编辑】，选中中间的地图，点击【编辑地图】。 
1689822666086-74088451-7d88-489c-a21a-2518d7453138.png 

新建一个库位和站点绑定，此处绑定AP84，表示机器人需要走到 AP32 来操作该库位。然后找到动作轨迹，点击【编辑】。
1689822811585-e086768a-59ad-4580-a732-f503343c6593.png

点击【添加】，在【键】处填入 “Load”（注意，“Load” 字符串是自定义的，用户可自行编写），值处填入下图所示的 JSON 字符串，点击【确定】。
1689824289561-52b727c2-deb5-473d-a44c-36b1f6a53ffa.png

### JSON字符串的填写可以参照该文档。任务指令
用同样的方法，在AP83建一个库位，然后添加一个键值对，【Unload】
1689824321141-e905773a-3a3f-4ac8-85b2-fb51f3ba289a.png

【保存地图】，并关掉该界面，点击【同步场景】。core中退出编辑，推送场景。
1689823787006-c8ee6e90-36b9-4f30-a16c-628573e9912b.png

进入rds界面，编辑一个天风任务，拖入以下三个代码块，填入对应信息。在第一个【机器人通用动作】的目标站点名中填入AP84，【指令】处选择binTask，参数写为【Load】；在第二个机器人通用动作的目标站点名填入AP83，【指令】处选择binTask，参数写为【Unload】。
1689824650039-c2d36d42-0fee-49cc-ae22-f32fdf5b6d6c.png

保存天风任务，然后运行。可以在运行监控页面看到机器人运行到对应位置，执行任务。
1689824904942-ceafc703-cc39-4fed-b4e9-e33184a1f239.png

### 2、  binTask 典型案例
本节为配置 binTask 的典型案例，可参考使用。
## 叉车
## 叉车识别取货
## 键（key）：
## ForkLoad
## 值（value）：
## {
    "end_height": 0.5,
    "operation": "ForkLoad",
    "recognize": true,
### "start_height": 0.09
## }
## 叉车放货
## 键（key）：
## ForkUnload
## 值（value）：
## {
    "end_height": 0.09,
### "operation": "ForkUnload"
## }
## 料箱机器人
## 料箱机器人取货
## 键（key）：
## load
## 值（value）：
## {
    "operation": "Script",
## "script_args": {
        "lift": 580,
        "operation": "load",
        "recAdjust": 1,
        "rotate": -1.57,
        "stretch": 900,
        "visionBinType": "code",
## "visionType": "box"
    },
    "script_name": "ctuNoBlock.py"
## }
## 料箱机器人放货
## 键（key）：
## unload
## 值（value）：
## {
    "operation": "Script",
## "script_args": {
        "lift": 640,
        "operation": "unload",
        "recAdjust": 1,
        "rotate": -1.57,
        "stretch": 900,
        "visionBinType": "code",
### "visionType": "shelf"
    },
    "script_name": "ctuNoBlock.py"
## }
## 顶升车
## 顶升车识别取货
## 键（key）：
## JackLoad
## 值（value）：
## {
   "operation": "JackLoad",
## "recognize": true
## }
## 顶升车放货
## 键（key）：
## JackUnload
## 值（value）：
## {
### "operation": "JackUnload"
## }
## 辊筒车/皮带车
## 辊筒预载货（预上料）
## 键（key）：
## RollerPreLoad
## 值（value）：
## {
    "operation": "RollerPreLoad",
## "direction": "left"
## }
## 辊筒载入货物（上料）
## 键（key）：
## RollerLoad
## 值（value）：
## {
    "operation": "RollerLoad",
## "direction": "left"
## }
## 辊筒卸载货物（下料）
## 键（key）：
## RollerUnload
## 值（value）：
## {
    "operation": "RollerUnload",
## "direction": "left"
## }
## 辊筒保持滚动
## 键（key）：
## RollerRoll
## 值（value）：
## {
    "operation": "RollerRoll",
## "direction": "left"
## }
## 双侧挡板下降并保持辊筒滚动
## 键（key）：
## RollerPass
## 值（value）：
## {
    "operation": "RollerPass",
## "direction": "left"
## }
## 辊筒停止滚动
## 键（key）：
## RollerStop
## 值（value）：
## {
### "operation": "RollerPass"
## }
