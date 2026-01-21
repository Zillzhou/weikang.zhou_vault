# Ethercat使用和Ethercat 单电机调试模式

### Ethercat使用和Ethercat 单电机调试模式

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2025.8.14

## 初版发布

## 使用中

## 1.使用限制条件
⽬前只⽀持 SRC2000.4 控制器（可在Roboshop⾼级配置界⾯右下⽅看到 SRC2000.x ）且需要是最新的ubuntu20.04 实时系统，如果不是请更换满足要求的控制器
## image.png

Roboshop版本：≥v2.4.2.16，
rbk 整包版本:≥1013，且需要如下 增量包 ，rbk增量包如下（需升级下面的增量包，后续新版本整包支持）：

DSP-x86-3.4.7.1015-20250729135705.zip
.底盘伺服电机数量大于8轴时，建议使用 EtherCAT通讯类型的伺服，目前实际客户使用 最大数量16轴，双臂人形全向底盘20轴，由于 无法实际测试更多数量，建议可以小幅度上探， 如2轴为增加幅度
目前已支持的电机类型有转向电机、行走电机,其中转向电机仅支持绝对值编码器类型电机,其余电机类型按需求开发测试，如果需要 Canopen 和 Ethercat 混用，底盘的电机使用 Ethercat ,其余使用 Canopen;
当使用该通信时,注意独立网口不能用于其他使用,如外接AP 以及其他网络设备；
### 当前已经适配  Ethercat 的驱动器品牌实测清单：
## image.png

## Ethercat 协议驱动器调试步骤
### 2. 电气连接方式
## 1.EtherCAT设备之间的网线规格:带屏蔽的超5类或电气性能规格六类及以上的网线,且网线建议远离功率线
### 2.电气连接方式遵循线性拓扑(其他类型不建议,难以描述清楚),从SRC2000独立网口出线，实物如下图（单电机链接）:
## image.png

## EtherCAT 设备之间的网线规格：
网线使用带屏蔽的超 5 类 或电气性能规格六类及以上的网线，且网线建议远离 功率线 
电气连接方式遵循线性拓扑（其他类型不建议，难以描述清楚），从SRC2000独立网口出线，相互间采用手拉手的方式链接，具体如下图所示：
## image.png

### 3. 模型文件配置
## 差速底盘模型文件配置：

## AMB-150.model
### 相比CAN通讯类型的电机,截图中的绿框配置不需要关
### 注,默认即可,其余的参数按照实际进行配置
## image.png

## Ethercat模块配置步骤如下
模型EtherCAT勾选isEnable，对应驱动设置为ethercat通讯模式
### 勾选isSupportPdoMap（目前只有同毅不支持）
positon为EtherCAT通讯线性连接的位置,从0开始
vendorID 为⼚家在 EtherCAT注册的供应商 ID，鸣志为 0x00000168 
productID 为⼚家在 EtherCAT 注册的产品 ID，鸣志为 0x00000011
## velocityRatio为转换比:
目的将cnt/s这个统一单位转换为鸣志驱动器的控制单位,公式为:
(60*10)/(4 * encoderLine=16384) = 0.009155
注意:encoderLine需要根据实际替换计算。
positionSpeedRation为位置模式速度转换比:
目的将°/s这个统一单位转换成鸣志驱动器的控制单位若为0.1rpm,则°/s转换成
### 0.1rpm,举例:(10*60* reductionRatio=275)/360=458.33425
img_v3_02or_27b2ff06-b894-4f5e-bde0-daf814b4d25g.jpg

img_v3_02or_a62fa8f9-73ba-482f-a861-9ca157b6ff1g.jpg

vendorID 为⼚家在 EtherCAT注册的供应商 ID，鸣志为 0x00000168 
productID 为⼚家在 EtherCAT 注册的产品 ID，鸣志为 0x00000011
## 两种方法可以得到:
## 1.咨询驱动器供应商
### 2.在控制器中使用指令（通过mobax指令查询，指令如下图）:
sudo /etc/init.d/ethercat start
### sudo ethercat cstruct -p 0
img_v3_02on_1112cbdc-9e92-4163-993c-d5ed49f0714g.jpg

## 多差速模组底盘配置：

## 四差速模组（华锐-同毅）.model
## 行走驱动模型配置
首先，配置一个多差速模型，可以在设备配置里新建模型或者导入上述已经配置完成的模型
## image.png

模型的motor坐标位置需要与模组实际位置相同，可参考实际机械图纸去填写（如下图）
## image.png

0862df3a-fe9f-4599-89be-55716d09fea3.png

需要确认模型里最大转速（maxRPM）、编码器线（encoderLine）、是否反转（inverse）、是否是被动电机（passive）、是否具备抱闸（brake）、轮半径（wheelRadius）、减速比（reductionRatio）：参考实际驱动器数值去填写，PS:encoderLine是电机单圈脉冲值/4。
## image.png

## 驱动参数示例：
## image.png

## 转向编码器模型配置
编码器需要勾选被动电机，是通过差速轮的速度差转向，编码器作为被动电机反馈角度
## image.png

外置编码器的outEncoderID、canport与实际配置对应（PS：outEncoderID与上面canID保持一致）
外置编码器的分辨率配置与实际对应，参考分辨率配置说明（PS：符合cia协议的编码器可使用）
## image.png

## ethercat通讯配置
### 模型EtherCAT勾选isEnable
## 对应驱动设置为ethercat通讯模式
## image.png

## image.png

### isSupportPdoMap（目前同毅不支持）
positon 为 EtherCAT 通讯线性连接的位置，从0从站开始
vendorID 为⼚家在 EtherCAT注册的供应商 ID，同毅为 0x0000034e 
productID 为⼚家在 EtherCAT 注册的产品 ID，同毅为 0x00445566 
### （该ID可通过mobax指令查询，指令如下图）
### 企业微信截图_17537794545573.png

velocityRatio 为转换⽐: （PS:速度控制指令单位转换，如果配置驱动器0x60FF控制单位为cnt/s默认填1即可）
⽬的将 cnt/s 这个统⼀单位转换为统⼀驱动器0x60FF对象字典的控制单位，同毅为 0.1rpm
## image.png

故cnt/s转换成0.1rpm 举例：(60*10) / (4 * encoderLine=16384) = 0.009155 
positionSpeedRation为位置模式速度转换⽐: 
⽬的将°/s这个统⼀单位转换成同毅驱动器6081对象字典的控制单位，同毅为0.1rpm 
## image.png

故°/s转换成0.1rpm  举例： (10*60* reductionRatio=275) / 360 = 458.33425 
## 多差速模组功能测试
## 行走功能测试
车辆吊起处于悬空状态，将四个轮组手动转至0度（误差0.05内），下发平动速度，查看实际的电机速度状态是否匹配，电机旋转方向是否为正常前进方向，正常是8个驱动速度转速一致、旋转方向一致，如不一致，查看motor模型配置电机参数与inverse是否设置正确。（PS:前提是ethercat从站对应与实际物理位置一致，可以逐步增加查看，mobax从站状态指令：sudo ethercat slaves）

### 20250731104120_rec_.mp4
## 全向横移功能测试
将四个轮组手动转至90°，下发平动横移速度，查看运行状态是否正常（如下视频）

### 20250731104846_rec_.mp4
## 原地中心旋转功能测试
将四个轮组手动转至与对角轮组连接线的垂线方向（如图测量角度，误差0.05内），下发转动测试，观察轮组的运行方向和速度是否正常，轮组内轮速度与方向一致，外轮速度与方向一致（如下视频），如不一致，查看转向配置编码器inverse设置是否正确。
## image.png

img_v3_02on_fef045eb-5383-4d04-840e-26123d8d58eg.jpg

## 调整轮组角度，下发转动测试：

### 20250731112506_rec_.mp4
如上测试完成之后可以进行落地测试导航和定位。
## Ethercat 单电机功能测试
能正常通讯后，打开roboshop，进行行走电机的功能测试。
左下角点设置图标，设置外部控制模式，勾选对应电机，下发速度指令。
以双舵轮车型为例，点击左下角齿轮,切换至如图电机控制页面。(此时屏蔽导航相关速度,包括手动控制键盘方向键。)
## image.png

## 1.如图中会将模型文件中所有motor设备列出,并根据电机func类型列出可下发的数据类型为速度类型或位置类
型。(目前限定func为steer为位置类型,walk为速度类型。其他也暂不支持。)
### 2.举例:
a.勾选motor电机,在speed输入框输入0.01m/s,点击发送。此时该电机将一直保持下发速度,停止电机需要
发送0m/s或者关闭该调试窗口。
b.勾选motor1电机,在pos输入框输入90°,电机发送。此时该电机将转动至该角度后保持不动。(根据车体模
型坐标位置。),关闭该调试窗口,该电机在未收到其他指令时仍保持该位置。
## FAQ 问题集
## Ethercat 通讯问题排查方法：
## 获得从站的状态:
sudo /etc/init.d/ethercat start
### sudo ethercat slaves
通过mobax指令 ethercat slave 或 sudo /etc/init.d/ethercat start指令是否能看到节点信息，如果可以则说明通讯没有问题（也可能是没有权限导致看不到节点信息）。
## 没有权限导致的无法通讯的情况如下：
## image.png

### 正常能看到如下节点信息（以鸣志电机为例）：
img_v3_02on_f7489ade-9413-402d-bbc1-0b6f2986587g.jpg

若是由于控制器刷镜像包后导致的无法通讯（可能是换硬盘导致的），可以通过如下指令排查：
主要就是看 /etc/sysconfig/ethercat的MAC和ifconfig -a中eth1是不是一致
还有lsmod | grep ec有没有ec_master和ec_igb
2000控制器下网口用ethercat占用，需要使用无线模块如何使用
首先使用 ethercat 方式时候网络客户端只能链接上网口，不能使用下网口，然后按照配置 3000 控制器的方式配置该客户端，且客户端的工作方式必须是 Client-root。
## image.png

### 如何通过Mobax 判断是否是ubuntu20.04版本：
### 使用 cat /etc/os-release 如下图所示
img_v3_02om_4cb3b4ab-38a3-444c-af40-3c5817a8ec0g.jpg

Ethercat 和 Canopen 是否可以混用，最大支持的 Ethercat 个数是多少
可以，保证底盘用 Ethercat,上层机构使用 Canopen 即可。单就 Ethercat 节点个数而言，16轴以下比较稳妥，16 轴以上需要根据实际业务需求来进行评估。
