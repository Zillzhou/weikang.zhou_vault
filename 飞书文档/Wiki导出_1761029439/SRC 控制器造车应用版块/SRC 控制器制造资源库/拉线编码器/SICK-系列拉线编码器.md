# SICK-系列拉线编码器

## SICK-系列拉线编码器

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.30

## 语雀迁移至飞书

## 使用中

## 一、适用范围

本文档针对 SICK-系列 拉线编码器的配置方法进行说明。
### 二、调试资源

## 上位机软件：

### USB_CAN+TOOLSetup(V9.12).zip

### 三、检查接线

### 将拉线编码器的 can 接到 can模块 上面
## 注意事项：

### 注意： canH 接 canH ， canL 接 canL

注意：在操作过程中，必须保证 can 总线上面只有拉线编码器这一个设备。( can 模块和拉线编码器直接相连)
### 四、拉线编码器参数配置

## 前言

## 拉线编码器的安装方向情况：

 正方向：一般情况下拉线编码器安装在货叉的底部，正速度的是拉线拉长的方向。

 反方向 ：拉线编码器安装在货叉的顶部， 正速度是拉线缩短的方向。若是此种安装方向，则需要配置2.2.4(将拉线编码器的计数方向反向)。

### <2> 拉线编码器要根据模型文件的配置情况进行配置激活：

（1）在叉车上面只使用一个拉线编码器的情况，配置方法请参照 二、使用单个拉线编码器配置方法；

（2）如果是三向叉等使用多个拉线编码器的车辆，配置方法参照三、使用多个拉线编码器配置方法；

配置多个拉线编码器时候要对照相关模型文件的配置文档，对应好不同的拉线编码器对应的货叉的动作。

## <3> 拉线编码器的初始状态

sick 系列的拉线编码器的初始波特率是125K，初始 nodeID 是 5
### 4.1 单个拉线编码器配置方法

## 配置拉线编码器的步骤：
打开 can 上位机软件 USB-CAN ,利用软件打开设备，将波特率设置为125K。

### 注：【 sick系列 拉线编码器初始波特率是125k】

### 4.1.1 配置拉线编码器的波特率

确认拉线编码器接在哪一条can总线上面，找到这个总线传输的波特率是多少，再决定是否修改编码器的波特率，例如：总线上面的波特率的125k，则直接进行第二步，跳过设置波特率的步骤

### Default baudrate: 125kbit/s

### node id: 5 (CAN ID 0x705)

The encoder will broadcast a message once power on:

ID: 0x705 DataFrame Standard DLC: 1 Data: 0x00

Then user can use CANopen LSS protocol to configure baudrate.

### 4.1.1.1 使能拉线编码器模式

### enable the LSS mode:

The encoder will not reply to this message.

ID: 0x7E5 DataFrame Standard DLC: 8 Data: 0x04 0x01 0x00 0x00 0x00 0x00 0x00 0x00

1677572618727-93ec85ee-f06c-45a3-8cbe-85d52ad8b1e1.png

### 4.1.1.2 设置波特率

## 按照要求设置波特率，为要求的波特率

Configure baudrate to 250kbit. Send CAN frame with USB-CAN:

ID: 0x7E5 DataFrame Standard DLC: 8 Data: 0x13 0x00 0x03 0x00 0x00 0x00 0x00 0x00

The byte[2] means the baudrate that user wants to set.

## 0  1000kbit

## 1  800kbit

## 2  500kbit

## 3  250kbit

## 4  125kbit

## 5  100kbit

## 6  50kbit

## 7  20kbit

## 8  10kbit

1677572642062-3c809be1-544c-4934-8a73-838712ede790.png

## 【注意此时是将拉线编码器配成125k】

### The encoder will reply:

ID: 0x7E4 DataFrame Standard DLC: 8 Data: 0x13 0x00 0x00 0x00 0x00 0x00 0x00 0x00

### 4.1.1.3 保存配置

Save the configure.

ID: 0x7E5 DataFrame Standard DLC: 8 Data: 0x17 0x00 0x00 0x00 0x00 0x00 0x00 0x00

## Reply:

1677572664711-e6358a77-5c19-417b-bd2d-50953e985d9b.png

ID: 0x7E4 DataFrame Standard DLC: 8 Data: 0x17 0x00 0x00 0x00 0x00 0x00 0x00 0x00

Then cut the power of the encoder. When power up again, the baudrate is already becom 250kbit

## 完成之后，拉线编码器重新上电

### 4.1.2 使能拉线编码器

设置波特率完成之后（若波特率使用250K，则设置为250k，波特率根据第一步设置的打开）

## 使能拉线编码，使之能够正常发送数据

We suppose that the baudrate of the encoder is already configured to 250k.

### Suppose node id: 0x05

Then after the encoder power on, the LED of the encoder will blink, indicating that the CANopen node is at: pre-operational mode.

The configurations followed should be done during pre-operational mode.

It's better to send a pre-operational mode command to engage that the mode is right.

### 4.1.2.1 设置拉线编码器为预操作模式

## ID: 00 00 00 00

## DATA: 80 05

1677572698139-45b945ac-50ae-4bef-b366-6fbfd2328e08.png

此步骤没有回复数据。

### 4.1.2.2 设置传输的模式为PDO模式

## ID: 00 00 06 05

### DATA: 2F 00 18 02 FE 00 00 00

note: ID should be 0x600 + nodeID

1677572718019-d8e3b73f-f89e-4dff-bb24-ed3c4aca41ca.png

此步骤检查 接收到的数据是：0x585      60 00 18 02 00 00 00 00

### 4.1.2.3 设置传输的周期为10ms

## ID: 00 00 06 05

### DATA: 2B 00 18 05 0A 00 00 00

data[4:7] indicats the period in milliseconds.

1677572738021-bec571b9-7f07-4e7b-8177-831083158bfc.png

此步骤接收到的数据 0x585      60 00 18 05 00 00 00 00

注意：拉线编码器若是反装，则使用2.2.4.2的配置方法，配置为反向

### 4.1.3 配置拉线编码器的计数方向

### 4.1.3.1 配置拉线编码器的计数方式为正方向

## ID: 00 00 06 05

DATA:  2b 60 00 00 00 00 00 00   (或者2b 60 00 01 00 00 00 00)

### 4.1.3.2 配置拉线编码器的计数方式为反方向

## ID: 00 00 06 05

DATA:  2b 60 00 01 00 00 00 00)

### 4.1.3.3 保存配置

## ID: 00 00 06 05

### DATA: 23 10 10 01 73 61 76 65

## 's' = 0x73

## 'a' = 0x61

## 'v' = 0x76

## 'e' = 0x65

1677572776060-0dbd0f2d-9f4a-40e6-9ba8-e2cec293f636.png

接收到数据为：0x585      60 10 10 01 00 00 00  00

### 4.1.3.4 激活拉线编码器

### 操作拉线编码器的node为可操作的node

## ID: 00 00 00 00

## DATA: 01 05

Note change to [01 00] when use for Triowin absolute encoder.

1677572792358-348fa972-46e4-4922-bd6b-e6e4ebe10808.png

## 检查接收到的数据：

 着重注意 0x185 的数据一直更新，此时接到控制器的can2上，会能检测到拉线的高度。

确认货叉降到底，标零拉线编码器。

注：若需要修改拉线编码的 nodeID ，请看 3、修改拉线编码器的 nodeID
### 4.2 使用多个拉线编码器的配置方法

注意：在使用多个拉线编码器的情况，由 outencoder ID 来区分不同
### 4.2.1 修改拉线编码器的node-ID、通信波特率

注：此步骤在多向叉车上面配置多个拉线编码器，若叉车上面只有一个拉线编码器，则忽略此步骤
1677572811471-f872f697-1c5e-465a-a8c3-2de68cd059cd.png

### 4.2.1.1 进入配置模式

## ID: 00 00 07 E5

### DATA: 04 01 00 00 00 00 00 00

## 返回数据：

## ID: 00 00 07 E4

### DATA: 04 00 00 00 00 00 00 00

### 4.2.1.2 设置node ID

（设置nodeID 为5 若nodeid为6，则设置为6即可）

## ID: 00 00 07 E5

### DATA: 11 05 00 00 00 00 00 00

## 返回数据：

## ID: 00 00 07 E4

### DATA: 11 05 00 00 00 00 00 00

## 若要将拉线编码器的nodeID改成6:

## 发送数据：

## ID: 00 00 07 E5

### DATA: 11 06 00 00 00 00 00 00

## 返回数据：

## ID: 00 00 07 E4

### DATA: 11 06 00 00 00 00 00 00

### 4.2.1.3 设置波特率

### （设置波特率为125k，其他可参照上图的内容）

## ID: 00 00 07 E5

### DATA: 13 00 04 00 00 00 00 00

## 返回数据

## ID: 00 00 07 E4

### DATA: 13 00 00 00 00 00 00 00

### 4.2.1.4 保存设置

## ID: 00 00 07 E5

### DATA: 17 00 00 00 00 00 00 00

## 返回数据

## ID: 00 00 07 E4

### DATA: 17 00 00 00 00 00 00 00

### 4.2.1.5 重新上电或者reset通讯

### 4.2.2  使能拉线编码器

设置波特率完成之后（若波特率使用250K，则设置为250k，波特率根据第一步设置的打开）

## 使能拉线编码，使之能够正常发送数据

注意：此时将相关的 nodeID 改成 3.1设置的 nodeID 在进行发送
### 4.2.2.1 设置拉线编码器为预操作模式

## ID: 00 00 00 00

## DATA: 80 05

### 此时可将nodeID改成3.1设置的nodeID

若nodeID是6,即发送的数据是 DATA： 80  06

### 可以将下面的所有的nodeID由05 改成 06

1677572835352-79ab061a-742e-4640-9eb0-a44e16a09271.png

此步骤没有回复数据。

### 3.2.2 设置传输的模式为PDO模式

## ID: 00 00 06 05

### DATA: 2F 00 18 02 FE 00 00 00

note: ID should be 0x600 + nodeID

### 接收到ID为：0x580 + nodeID

1677572855420-e977e38a-a3ad-4b17-a7ac-fd1c2d71bba6.png

此步骤检查 接收到的数据是：0x585      60 00 18 02 00 00 00 00

### 3.2.3 设置传输的周期为10ms

## ID: 00 00 06 05

### DATA: 2B 00 18 05 0A 00 00 00

data[4:7] indicats the period in milliseconds.

1677572875346-ef401e4d-9b91-4a1f-a1cc-f1c1eecc3b69.png

此步骤接收到的数据 0x585      60 00 18 05 00 00 00 00

注意：拉线编码器若是反装，则使用2.2.4.2的配置方法，配置为反向
### 3.2.4 配置拉线编码器的计数方向

### 3.2.4.1 配置拉线编码器的计数方式为正方向

## ID: 00 00 06 05

DATA:  2b 60 00 00 00 00 00 00   (或者2b 60 00 01 00 00 00 00)

### 3.2.4.2 配置拉线编码器的计数方式为反方向

## ID: 00 00 06 05

DATA:  2b 60 00 01 00 00 00 00)

### 3.2.5 保存配置

## ID: 00 00 06 05

### DATA: 23 10 10 01 73 61 76 65

## 's' = 0x73

## 'a' = 0x61

## 'v' = 0x76

## 'e' = 0x65

1677572900752-52b9ca54-0fa7-4dbc-99cd-ca9ae156852b.png

接收到数据为：0x585      60 10 10 01 00 00 00  00

### 3.2.6激活拉线编码器

### 操作拉线编码器的 node 为可操作的 node

## ID: 00 00 00 00

## DATA: 01 05

Note change to [01 00] when use for Triowin absolute encoder.

1677572948462-1b61287e-b556-4bb3-a961-83b1899ff1f6.png

## 检查接收到的数据：

着重注意0x180+ nodeID 的数据一直更新，此时接到控制器的 can2 上，会能检测到拉线的高度。

如果 nodeID 改成06 的话，会接受到 0x186 的数据

确认货叉降到底，标零拉线编码器。

### 六、测试

### 七、常见问题
