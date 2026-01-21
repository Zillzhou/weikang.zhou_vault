# ZAPI驱动器-ACECOMBIX

## ZAPI驱动器-ACECOMBIX

## 版本

## 更新日期

## 更新说明

## 文档状态

## 维护责任人

## V1.0

### 2024.4.29

## 语雀迁移飞书

## 使用中

## 一、适用范围
本文适用于 ZAPI-COMBIX 驱动器的调试与配置。
### 二、调试资源
## 调试工具：
通过ZAPI驱动器的手持端进行调试。
1663141929103-8204b2b1-4913-4cc6-b382-9fa56627e43f.png

## 连接和调试方法，可以参照文档：

Smart  Console使用指导_简易操作版（中文）V0.0.pdf
### 三、接线与修改参数步骤
### 3.1 电气接线
驱动器接线按照车辆电气图进行接线。
## 注意：
不同型号的叉车使用同一款驱动器的电气接线图有可能是不相同的，需要按照实际的电气原理图进行检查。
### 3.2 修改参数步骤
### 连接 smart Console 手持端
将 smart Console 手持端连接到 ZAPI 驱动器上面，注意区分 CANH 、 CANL 的定义。
按照四、驱动器配置参数步骤进行参数调整。
### 3.3 注意事项
## 注意：
在连接 smart Console 手持端 的时候，一定要检查 CANH 、 CANL 和 F2A 的 CANH 、 CANL 连接是否正确。
注意选择正确的波特率进行连接。
### 四、驱动器配置
### 4.1 ZAPI驱动器运动学参数修改
针对问题：车辆在正常状态和正常速度的情况下，会出现车辆无法动作，经过检查，此时SRC控制器下发的速度是正常的，ZAPI驱动器也有驱动电流输出，但是电机仍处于堵转状态。
参数所在的菜单位置： MAIN MENU - HARDWARE SETTING
1663144714962-ae200c85-ccb7-4780-bcd4-328150b0e70a.png

## 修改参数：
1663143013364-169f122f-182b-4c15-85dc-b2a7fcafc336.png

修改 SAT FREQUENCY 为 160HZ ；
修改 BRAKING MODUL 为 150HZ ；
修改 BOOST AT HI FREQ 为 63% ；
修改 BOOST CORNER FRE 为 60 ；
修改 MAXSLIP 0 为 4 ；
修改 MAXSLIP 1 为 9 ；
修改 FREQSLIP 1 为 25 ；
修改 MAXSLIP 2 为 8 ；
修改 FREQSLIP 2 为 45 ；
修改 MAXSLIP 3 为 8 ；
修改 FREQSLIP 3 为 50 ；
修改 MAXSLIP 4 为 10 ；
修改 FREQSLIP 4 为 55 ；
## 注意：
将参数表左侧的参数，调整成右侧红框中的参数。可以解决小速度下电机因为扭矩产生的堵转问题。
当出现ZAPI驱动器也有驱动电流输出，但是电机仍处于堵转状态的问题时，可以对应调整上面的参数。
### 4.2 ZAPI驱动器比例阀参数修改
针对下降过程中的问题：下降过程中出现货叉抖动，出现比例阀阀门控制速度不稳的情况，根据现有的分析出来的问题调整了参数【此部分参数调整完成之后并不能解决控制比例阀没有较好响应下发速度的问题】
参数所在的菜单位置： MAIN MENU - SPECIAL AJUSTMENTS
1663144445678-6bcd0ab3-17aa-45cd-abd3-94de3c146950.png 

1663144529686-1acebb2e-a848-46a2-a10b-fa26999bcb35.png

修改 MINEVP 为 6.3 ；
修改 MAXEVP 为 70.2 ；
修改 EVP OPEN DELAY 为 0 ；
修改 EVP SHUT DOWN DELAY 为 0 ；
修改 DITHER AMPLITUDE 【高频振荡振幅】为 0 ；
修改 DITHER FREQENCY 【高频振荡频率 】为 83.3 ，如果产生抖动，则将此值修改为 41.6 ；
