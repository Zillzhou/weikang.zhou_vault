# SRDMutex 组

## SRDMutex 组
## @杨达
## >0.1.8.230105
SRDMutex 组是用于和 SRD 的“交通管制区”对接的互斥图元组。
## 应用场景
## SRDMutex 组仅适用于以下场景：
现场同时运行了 SRD 和 Core，并且 SRD 调度的 AMR 与 Core 调度的 AMR 需要在特定的区域进行交通管制。

## 操作说明
### SRD 的交管功能文档：SRD 交通管制区详细说明

SRD 和 Core 的关系。
对于 SRD 来说，Core 作为一个电控柜存在。
1685695071335-c1ab1a5b-bde2-494b-af3d-873056e094d0.jpeg

## Core 的必要操作
1685692739579-4784860e-6a7e-4be6-b940-c47f13be86c5.png

① 通过 Roboshop Pro 连接 Core，并启用编辑。
② 点击界面左侧的「高级组」标签。
③ 添加一个新的「高级组」。
④ 在「高级组」列表中选择要编辑的「高级组」。
⑤ 修改「高级组」的类型（type）为 SRDMutex。
⑥ 根据实际情况，配置对应的 ModbusTCP 地址；默认配置如下图所示。
1674889259995-61662cfb-3d72-4e39-aff5-3c5c70e997a7.png

⑦ 保存，退出编辑，推送场景。
## SRD 中对应的关键配置
## SRD 的交通管制区的配置如下所示：
# 设备服务器配置项。  
## synchronizer:
  #电梯服务地址, 默认值http://localhost:7100/api/
  mutexZoneRoute: "http://localhost:7100/api/"

## # 交通管制区的配置
## zones:
  mz1:                        # 管制区域的名称，唯一值
    resetBtn: true            # true(默认): 区域空闲时，按下后再释放按钮，区域状态才会变为被占用，再次按下并释放，区域状态才会恢复为空闲；false: 区域空闲时，按钮按钮后，区域状态变为被占用，释放按钮后，区域状态恢复为空闲
    writeBatch: false         # false(默认)：逐个写入信号；true：批量写入信号。
    involvedSystems:          # 参与交通管制的调度系统 sysId:ip
      self: 127.0.0.1         # 服务器自身信息。 此项必填！！！
      core: "Core 的 IP 地址"        # 如果 SRD 和 Core 运行在同一台服务器上，可以注释这一行的配置。
    boxes:                    # 此管制区域中的标准电控柜信息，此案例中，对应的是 Core 的 ModbusTCP Slave。
      - boxId: box_core                                # 电控柜名称，值唯一。
        standAlone: false                        # false(默认)：工作时会读写 ModbusTCP 信号；true：工作时不会读写 ModbusTcp 信号。
        host: 127.0.0.1       # PLC 的 IP 地址；此案例中，对应的是 Core 的 IP 地址。
        port: 502             # 端口号，默认为502。
        unitId: 0             # 控制柜中 PLC 的 SlaveId，默认值为0，一般写 1。
        lightRedAddr:                            # 红灯的地址配置。
          addrNo: 100                                        # 地址编号。
          funcNo: "05"        # "05":可读写线圈量，"06":可读写寄存器。此案例用 "05"。
        lightGreenAddr:                          # 绿灯的地址配置。
          addrNo: 102                                        # 地址编号。
          funcNo: "05"                                # "05":可读写线圈量，"06":可读写寄存器。此案例用 "05"。
        lightYellowAddr:                        # 控制黄灯亮/灭的地址，可选配 
          addrNo: 101                                        # 地址编号。        
          funcNo: "05"                                # "05":可读写线圈量，"06":可读写寄存器。此案例用 "05"。
        switchAddr:                                                # 复位开关对应的地址，可选配          
          addrNo: 103                # 地址编号。
          funcNo: "05"        # "01":可读写线圈量，"02":只读线圈量，"03":可读写寄存器，"04":只读寄存器。此案例用 "05"。

## 运行逻辑
开关（switch），srd读取，core写入；被core控制的机器人占用时，为1，否则为0，被srd占用时，没人占用时，都为0。
红灯，core读取，srd写入；srd写入1时，core会尝试将互斥组分配给 SRD ，分配成功则将红灯地址写为1，分配失败保持红灯地址读取结果为0。分配成功后，通过http接口查询互斥组占用情况， id 为 SRD 。
绿灯，core读取，srd写入；srd写入1时，core会尝试以id SRD 来释放互斥组，释放成功则将绿灯地址写为1，在 SRD 占用互斥组或者无人占用互斥组时，都可以释放成功，释放失败保持绿灯地址读取结果为0。绿灯地址读取结果为1时，互斥组一定没有被占用。core不会在点亮绿灯时自动清除红灯，srd会清除。
黄灯由srd维护。
