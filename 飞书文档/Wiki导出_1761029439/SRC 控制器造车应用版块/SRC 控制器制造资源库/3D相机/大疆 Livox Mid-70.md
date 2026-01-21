# 大疆 Livox Mid-70

## 大疆 Livox Mid-70

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.3.25

## 语雀迁移至飞书

## 使用中

## 一、适用范围

本文档针对 Livox-mid70 激光传感器的配置方法进行说明。
### 二、调试资源

https://www.livoxtech.com/cn/downloads ，软件名称为： Livox_viewer
### 三、参数配置
## 1. Livox_viewer参数配置
首先通过以下地址下载上位机软件 https://www.livoxtech.com/cn/downloads，软件名称为：Livox_viewer。
本公司选用的是 大疆 Livox-mid70 的 3D 激光雷达用于检测障碍物， Livox-mid 的有效视场角为 70.4 度，如下图所示：
1677576484320-5b5cf3de-a84a-424d-8189-0669c6a33df6.png

如将 PC 与 Livox 设备直连，则需要将 PC 的 IP 类型设置为静态 IP，IP 地址为 192.168.1.2，子网掩码为 255.255.255.0，默认网关为 192.168.1.1。然后通过大疆官网的下载软件 Livox_viewer 连接激光，打开 Livox Viewer 将会进入主界面。 Livox Viewer 主界面由工具栏、设备管理器和点云显示界面组成。
1677576501394-ba6e16ca-a80f-477a-885f-b57e4d939336.png

Livox 设备成功连接后，点击工具栏中 Device settings 按钮，可打开设备的参数设置界面，查看当前 Livox 设备的基本状态以及对其参数进行更改
1677576521426-4bd50a2c-cd25-4c91-813c-b13491bbc0fd.png

然后将激光 ip 设置为 static ip，设置成 192.168.192.XXX 网段内的 ip，确认保存后重启。

### 2. Roboshop 配置使用
跟上面两个 3D 激光类似，在模型文件 Camera 中选择相机类型为 DJI-mid70-tcp ，如果默认只了一个 3D 激光，则将 PercipioID 设置为空，可以自动找到当前激光。若安装两个以上的激光，在 PercipioID 中需要填写在 livox 激光背后二维码下面的一串 SN 码，在使用 Livox_viewer 也能看到。如下图：
1677576540476-b56e9895-a05d-411d-8bf2-4da7f21f185f.png

1677576557703-37dedf7d-0e62-4e2c-8b4e-8f7e31487ed7.png

其中 mid-70 最后一位也是 1。

1677576573089-07a818ee-ad1a-4229-a775-ead58a821323.png

如果只是用一个livox传感器，在roboshop配置界面中的2部分可以空白，不需要填写SN码，如果使用多个livox传感器，则需要根据安装位置来准确填写对应传感器的SN码。
在roboshop配置界面中，在brand选择DJI-mid7-TCP型号，ip和port不需要填写。
1部分指的是传感器的外参，可通过标定获得；
在用作避障的时候，参数onlyForPalletDetection应不勾选。
## 上图中1号框内从上到下依次为：
## x 安装位置
## y 安装位置
## z 安装高度，标定输出
## roll 俯仰角，标定输出
## pitch 水平安装，默认为0
## yaw 偏航角，用于调节相机朝前/朝后
## RBK 3.4.6
Ie9ibkwxzoK8MYxlaR7cO3VBntb.png

rbk3.4.6 之后的Mid-70 在roboshop上的配置界面如上图所示。相机配置参数如下：
maxLength : 最远成像距离，超过该距离的相机数据会被截断
