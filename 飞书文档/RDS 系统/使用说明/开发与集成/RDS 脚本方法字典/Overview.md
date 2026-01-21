# Overview

## Overview
## 一、什么是 RDS 脚本方法
RDS 脚本方法是在 RDS 系统管理后台的在线脚本功能中使用的JavaScript方法，使用这些方法可以实现创建 RDS 天风任务、读写Modbus、查询 RDS 库位状态等功能，具体可参看脚本方法说明。

### 二、脚本方法结构说明
以下以 异步创建并运行一个任务的脚本方法为例进行说明，该方法介绍如下：
*************************************************************************************************************
## 方法说明
代表该方法具体实现的功能介绍。
本方法是非阻塞方法，将在脚本中创建一个线程，来执行天风任务的实例。
function createWindTask(param: string): void
createWindTask 代表该方法的方法名，（param: string）是该方法的输入参数和参数类型，":"之后的部分，代表该方法的返回值类型，此处的void代表该createWindTask方法不存在返回值。

## 输入参数
### 其参数 param 是 JSON 字符串，结构如下：
以下是该方法输入参数的demo，输入参数是json格式。
## {
## // 任务类型 id
    "taskId": "12345",
## // 任务名称
    "taskLabel": "Ps2Ws",
## // 指定创建的任务实例的id
    "taskRecordId": "04d11f4f-d17c-4a41-9cf7-67de468035f9",
## // 在天风任务中定义的参数
    "inputParams": "{\"fromSite\":\"PS-01\"}"
## }

注意 ： taskId 和 taskLabel 两者必选其一。如果同时存在，将首选使用 taskId 字段。

注意，inputParam 字段，是个 JSON 字符串。需要在脚本里将任务参数序列化为 JSON。

## 输出参数
## 无
此处是该方法的输出参数，如果该方法没有输出参数，则此处为”无“。
## 异常
本方法不抛出异常。
该异常部分代表调用该脚本方法可能返回的程序异常。如果该方法抛出异常，可用JavaScript的try catch进行捕捉，并根据业务在catch中做后续程序处理。
*************************************************************************************************************
### 三、脚本方法使用示例
### 以下以 异步创建并运行一个任务的脚本方法为例进行演示：
// 构建createWindTask脚本方法所需要的输入参数
var inputParams = {"fromSite": "PS-01"};// 天风任务的输入参数
## var taskParam = {
## // 任务类型 id
  "taskId": "12345",
## // 任务名称
  "taskLabel": "Ps2Ws",
## // 指定创建的任务实例的id
  "taskRecordId": "04d11f4f-d17c-4a41-9cf7-67de468035f9",
## // 在天风任务中定义的参数
  "inputParams": JSON.stringify(inputParams)
};

// 调用createWinTask方法，传入上面的输入参数
jj.createWindTask(JSON.stringify(taskParam));

## 附录

RdsScriptMethods_zh_20250402104830.pdf
