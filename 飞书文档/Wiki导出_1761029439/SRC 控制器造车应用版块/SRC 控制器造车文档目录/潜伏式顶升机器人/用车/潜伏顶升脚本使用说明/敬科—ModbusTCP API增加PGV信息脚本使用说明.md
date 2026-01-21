# 敬科—ModbusTCP API增加PGV信息脚本使用说明

### 敬科—ModbusTCP API增加PGV信息脚本使用说明
## 使用要求
## RBK 3.4.6.18以上
### 必须安装或者上传modbus_tk的库文件
## image.png

## 脚本名称与位置
Roboshop uuid: c3a95caa-e126-4dcb-b4d0-cf8ee15294d8 
IbWZbvKVBozvGYxymRncrR7EnDb.png

## 脚本使用方式
登录脚本项目平台，公用账号  账号：user 密码：user
## image.png

## 下载项目对应脚本
## image.png

## 下载到本地后推送
## image.png

若无法登录脚本管理平台通过uuid下载，可以直接脚本pro_jingke_modbustcp_pgv.py上传到机构脚本界面并推送
配置periodRun（才能保证按照正常频率上报）——推送模型文件
## image.png

## 测试与api读取
ModbusTCP API 样例如下，99-116位为pgv信息，可以通过工具 Modbus Poll进行测试。详细ModbusTCP信息请参考链接概述

## seer_status1.mbp
## image.png
