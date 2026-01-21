# 北阳 UAM 激光传感器

## 北阳 UAM 激光传感器

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.16

## 初版编辑

## 使用中

## 适用范围

本文适用于北阳 UAM激光传感器配置。
## 调试资源

## 资源名称

## 资源作用说明

## 资源下载链接

## 官方网站

## www.by-sensor.com

## 上位机调试软件工具

### UAM Project Designer2.2.zip

## 硬件资源

## 网线、DC24V 电源等

## 无
1653385831823-6ebf837b-511a-46a8-b4c2-da741070196b.png

也可直接访问sick官网下载最新版。
## 线束连接
## image.png

激光信号线 8 接头应拧入激光传感器以太网接口，RJ45 接头应当接入 SRC 的六口交换机任意一个。
激光电源线中较粗的棕色线与蓝色线分别为 24V+,GND。

## 上电检测：
UAM-05LP-T301 已自带电源线，使用 24V（棕色线为 24V+、蓝色线为 GND）；
UAM-05LP-T301 需要外接信号线（ 4 针 M8 接头）；
### 正常上电后表面 7 段显示器将显示数字，如图所示：
## image.png

## 参数配置
在笔记本电脑上安装 UAM Project Designer 软件。
启动 UAM Project Designer 软件，使用使用 USB-MicroUSB 线，连接电脑 USB 接口与激光传感器上 MicroUSB 接口
## image.png

软件将自动检测传感器连接的串口，点击 Connect 进行连接
## image.png

### 当软件连接上传感器后默认进入【Monitor】(监控模式)
## image.png

点击【Configuration】（配置模式），此时软件要求我们输入八位密码

## 默认密码为 12345678
## image.png

进入配置模块后屏幕上方将显示配置流程，即【General】→【Function】→【Area】→【Confirm】→【Transmit to Sensor】
## image.png

在【General】模块配置：IP 为：192.168.192.100，子网掩码：255.255.255.0，默认网关：192.168.192.1
## image.png

在【Function】模块内配置：所需区域，将最小检测宽度更改为 30mm,将 Active areas 的数量设置为 1，其余参数均使用默认值
## image.png

在【Area】模块设置区域的大小：在 Protection 规定的区域画上任意大小区域。由于我们使用方法为取激光传感器中点云参数，此区域的大小对我们使用没有影响
注：只有将区域画上后配置才能继续。
## image.png

在【Confirm】模块，使用鼠标依次点击屏幕右侧【General】、【Function】、【Area】等选项，依次检查并确认前几个步骤中设置的参数
## image.png

注：在【Confirm】界面中的每个模块都要依次点击确认一遍，否则最后的 【Transmit to Sensor】将一直处于灰色（不可用状态）。
在确认完所有模块的参数后，【Transmit to Sensor】将变为黑色（可用状态）：
## image.png

最后用鼠标点击【Transmit to Sensor】按钮，此时以前步骤中设置的参数将全部下载到传感器内
## image.png

激光设备配置完成后需要对激光设备进行重启，等待重启完成可以使用 CMD 命令行进行 Ping 通测试【ping 192.168.192.100】，测试通过，则说明激光设备 IP 已经配置正确。
注： 激光设备配置完成后，必须对设备进行重启。

至此，UAM-05LP-T301 激光传感器配置结束。

## 模型配置
UAM-05LP-T301 在 Roboshop 中的模型配置如下：

## 故障速查表

https://assets.seer-group.com/robokit/wangguan/robokit_book/sensor/laser/laser_uam_14.png

https://assets.seer-group.com/robokit/wangguan/robokit_book/sensor/laser/laser_uam_14.png
