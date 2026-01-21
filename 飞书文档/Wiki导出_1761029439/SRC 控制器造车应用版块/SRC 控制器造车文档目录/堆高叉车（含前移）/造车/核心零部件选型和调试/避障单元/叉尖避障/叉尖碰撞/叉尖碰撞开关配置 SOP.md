# 叉尖碰撞开关配置 SOP

## 叉尖碰撞开关配置 SOP
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

## 文档资源文件清单：

## 文件名

## 资源文件

## 文档内编号

## 常见类型叉车模型文件

## 模型文件.zip

## 模型文件-01
## 模型文件配置
## 配置 DISensor
### 屏幕截图 2024-03-21 135122.png

## 图 1.1 模型文件配置页面
进入DISensor；
复制并添加新的DISensor；
重命名设备，并启用设备；
选择DISensor 的用途类型：选择collision；
1703643661970-f7845d0a-af12-4cf7-a25c-5ae2d2ca13d1.png

### 屏幕截图 2024-03-21 130514.png

## 图 1.2 type 配置
配置id，注意此id与电气接线的DI端口的id对应；
1703644068237-cdb8a57d-6ab4-48ad-ae5c-b768ad9ffd20.png

## 图 1.3 basic 配置
配置形状为arc；
1703644051502-fb91f49e-18a9-4515-97d5-bc09e4002fe4.png

## 图 1.4 配置形状
选择触发后的功能为stop;
配置安装位置：x、y、z、yaw。
### 屏幕截图 2024-03-21 135122.png

## 图 1.5 配置安装参数
说明：此处的minDist、maxDist、range无实际意义，可不用配置。
## 配置 DI
设备名：根据传感器实际电气连接，配置 DI ，修改 DI 设备的设备名，便于检查线路；
inverse：根据实际需要选择 DI 信号状态是否反转。
1701933222022-3bcc38d4-ec07-48a8-9d00-16fb6b37e345.png

## 图 1.6 配置 DI
## 配置结果测试
配置完成后点击上传；
确定并推送至机器人控制器；
## image.png

## image.png

## 图 3.1 上传修改后的模型文件
返回地图与控制页面，下发路径导航任务，并手动按下碰撞条，如配置正常将会显示碰撞并有错误告警，如下图所示。
### 屏幕截图 2024-03-27 134511.png

## 图 3.2 机器人发生碰撞告警信息
## 参数说明

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## 支持版本

## id

### 模型文件- DIsensor -basic

## —

## 0

## 0

## 88

### 3.3.4.20~latest

## IO 传感器模块设置对应的 id 号

## type

### 模型文件- DIsensor -type

## —

## none

## collision
## infrared
## fallingDown
## goodsDetect
## ignoreTask

### 3.3.4.20~latest

IO 传感器用途，货物检测（goodsDetect）、忽略任务（ignoreTask）、碰撞检测（collision）、红外传感器（infrared）、超声波传感器（ultrasonic）、防跌落（fallingDown）；

## arc

### 模型文件- DIsensor -shape

## —

## arc

## arc
## vertx

### 3.3.4.20~latest

## IO 传感器形状

## x

## 模型文件-DIsensor-shape

## m

## 0

## -1000

### 3.3.4.20~latest

## I/O传感器在车体坐标系下x轴方向的值

## y

## 模型文件-DIsensor-shape

## m

## 0

## -1000

## 1000

### 3.3.4.20~latest

## I/O传感器在车体坐标系下y轴方向的值

## z

## 模型文件-DIsensor-shape

## m

## 0

## -1000

## 1000

### 3.3.4.20~latest

## I/O传感器在车体坐标系下z轴方向的值

## yaw

## 模型文件-DIsensor-shape

## °

## 0

## -180

## 180

### 3.3.4.20~latest

### I/O传感器在车体坐标系下 z 轴的角度偏移值

## minDist

## 模型文件-DIsensor-shape

## m

## —

### 0.0

## 999

### 3.3.4.20~latest

### I/O传感器最小检测距离（盲区），单位为 m

## maxDist

## 模型文件-DIsensor-shape

## m

## —

### 0.0

## 999

### 3.3.4.20~latest

### I/O传感器最大检测距离（盲区），单位为 m

## rang

## 模型文件-DIsensor-shape

## °

## —

## 0

## 360

### 3.3.4.20~latest

## I/O传感器检测角度范围，单位为°

## slowdown

### 模型文件- DIsensor -func

## —

## stop

## stop
## slowdown

### 3.3.4.20~latest

## 触发后的功能
