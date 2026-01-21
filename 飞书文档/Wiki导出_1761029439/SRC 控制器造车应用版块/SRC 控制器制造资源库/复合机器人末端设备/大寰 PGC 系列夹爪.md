# 大寰 PGC 系列夹爪

## 大寰 PGC 系列夹爪

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.3.25

## 语雀迁移至飞书

## 使用中

## 一、说明
## 1. RBK 版本
### 3.4.0.4+

### 2. 型号
## PGC系列

### 3. 通信方式
## RS485

### 4. 支持平台及连接方式
## SRC800
## 使用 RS485 连接
### 通过 USB 转换器（USB 转 RS485）
## SRC2000
### 通过 USB 转换器（USB 转 RS485）

### 5. 线序
### 不同型号夹爪可能存在差异，以实际线序说明为准
1657507838851-0cad07a8-7ad0-4b9e-a460-3d33fb5213a1.png

### 6. 供电
## 24V

### 7. 设备手册

## pgc系列操作手册_v2-5.pdf

### 8. 调试软件
dh-robotics-user-interface-v1-0-6-20210609-setup.exe

### 二、设备参数配置
## 1. 连接上位机
使用 USB 转 485 正确连接设备 485 后，软件可以自动识别到串口，点击自动连接

1657508004283-debab354-a731-4db0-9d3a-36434cd9eb0b.png

### 2. 配置串口参数
## 配完串口参数后点击保存按钮
1657508027327-5dfc01ad-c9d1-48bc-8f64-210fc90a9e64.png

### 3. 重启设备
## 设备参数配置完成后重上电生效

### 三、模型文件配置
## 1. brand
1657509365097-f7add81e-5c39-42ef-a64a-e462d96b0ca6.png

### 2. RS485 直连
## 模型参数目录：type-devName
## /dev/ttyUart x
## 具体Uart几按照 串口说明 填写配置

例如： 通信线接 pin1 和 pin2，devName 填 dev/ttyUart0

### 3. 通过 USB 转换器
### 该配置方法 SRC800 和 SRC2000 通用
### SRC 只保留与夹爪相连的 USB 转换器，其余全部拔掉
使用 MobaXterm_Personal 远程进 SRC， MobaXterm_Personal 使用方法 
输入指令：ls /dev （ls 与 /dev 之间 有空格 ）
观察返回数据是否有 ttyUSBx 字样，把 ttyUSBx 填到模型文件 devName 中
有新 USB 设备时，重复步骤 1~4，新增 USB 设备时，无需拔掉第一个已经识别到的 USB 设备，否则 USB 名称会重新分配

如下图，识别出来的 USB 设备名为 ttyUSB0，devName 填写 dev/ttyUSB0

1657512204502-c11e2e61-f8c1-4d9c-9094-f7864360fe3f.png

### 4. arm 模型相关项配置
## 模型参数路径：
## arm-GripperPare
### 4.1 SRC800
### GripperPare 配为 EndWorker-TCP
1657517163456-9d2902af-88fb-4b81-a96e-93460db15d8f.png

### 4.2 SRC2000
arm-GripperPare 配为 GripperIn2000
1657517238739-3ca9499e-2830-4154-937b-c766512f75c3.png

### 5. Trigger配置DI触发夹爪标零
1683266687129-49411e5f-393a-4480-8399-0a4d5a04a66a.png

### 四、FAQ
## 1. 报错 53110
### 夹爪断连，检查模型文件配置是否正确，检查接线是否正确
