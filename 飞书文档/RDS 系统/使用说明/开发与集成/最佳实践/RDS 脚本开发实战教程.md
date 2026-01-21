# RDS 脚本开发实战教程

## RDS 脚本开发实战教程

## 一，概述

RDS 系统，以 天风低代码任务引擎 + JavaScript 在线脚本 的组合方式，来支持业务流程二次开发。

本文讲解一些实战案例，供编写业务参考。

## 二，脚本加载及运行方式

在菜单栏点击【脚本操作】按钮，打开在线脚本页面。

1662364251172-f94cb951-8d1d-4108-a877-ab888043a420.png

在该页面，可以直接编辑、保存、运行 Javascript 脚本，以及观测脚本执行过程中输出的日志信息。所有的 。js 文件都保存在 data/rds/script/ 目录下。

在脚本代码中，必须存在唯一的 boot（） 方法。 boot 方法是 RDS 所有脚本的入口方法，相当于 C 语言、Java 中的 main 方法，RDS 系统每次启动，会加载脚本，并自动执行一次 boot（） 方法。如果脚本被在线修改，也可以手动点击【启动】按钮，重新执行 boot（） 方法。boot（） 方法中，将进行注册接口、注册事件处理器、注册按钮等操作，这些必须在业务开始前，被执行一次。

## 三，脚本开发实战案例

### 3.1 注册接口并创建任务实例

如下图所示，创建一个天风任务，将任务命名为 "DemoWindTask"。
1694583404138-9f10d25f-37f9-4929-9fd2-778b00859b08.png

1694583459482-e5d24155-4135-40bd-ad39-3af3197b1190.png

设置一个输入参数 destination，字符串类型，并将该参数填入【机器人通用动作】块的【目标站点名】内。
1657160024212-a8f45db4-d021-40cc-9ff1-fe2cecb0453d.png

 打开脚本操作页面，新建脚本 boot.js，将如下代码（下文有源代码）写入脚本，并点击【保存】。
1694584122023-444e695b-259a-4ce8-8e8d-144d76728d8e.png

 点击【启动】按钮，令 RDS 系统重新加载脚本，执行 boot（） 方法 。点击【日志】按钮，打开日志输出栏。
## 使用 Postman 工具，发送请求：
POST  http://127.0.0.1:8080/script-api/postOrder

请求体是 JSON 格式，将 destination 参数赋值为 “AP123”，如图所示： 
1653450914232-b875efff-1501-48f0-84bd-109455f30c1a.png

若在脚本中自定义了响应信息，可在发送请求后，获得该响应信息：
1694584601821-0f431789-c092-4cf8-8efb-32a2a8f41072.png

## 在脚本日志界面，可见日志输出：
1661158348838-f21cc691-1cdc-4200-8bdd-4adbc5829211.png 

同时，在任务管理界面，生成一个 DemoWindTask 的实例。调试成功。

## 脚本解析：

## function boot() {
    // 注册一个接口，并关联到 postOrder 方法，且不需要登录权限。
    // jj 是 Java - JavaScript Bridge 的简称。是脚本中可以使用的全局对象。
    // 关于 jj 提供方法的详细说明，请参考《脚本方法字典》。
    // 注意！！！注册的接口，前缀必须是 /script-api/，才能保证不需要登录就可以访问此接口。
    jj.registerHandler("POST", "/script-api/postOrder", "postOrder", false);
## }

### function postOrder(param) {
    // 解析 param。包括请求的内容，均是通过 param 字段传入。
    var paramObject = JSON.parse(param);
    // 使用请求参数，构造任务的输入参数。
## let inputParams =
## {
            // destination 字段，是从 POST 请求的 body 中获取。
            destination: paramObject["destination"]
        };
    // 构造任务参数。
## let taskParam = {
        // 上文定义的天风任务名称。
        taskLabel: "DemoWindTask",
        // 注意，输入参数需要格式化为 JSON 字符串。
        inputParams: JSON.stringify(inputParams)
    };
    // 下发任务。注意，任务参数，需要格式化为 JSON 字符串。
    jj.createWindTask(JSON.stringify(taskParam));
    // 输出日志。使用该方法，可以将日志打印到 RDS 前端日志栏内。
    jj.scriptLog("info", "postOrder", "Hello world")

## //自定义返回信息
    var resp = { message: "请求成功", code: 0 };
    return { code: 200, body: JSON.stringify(resp) };
## }

### 3.2 通过监听 Modbus Tcp 信号创建任务实例

打开脚本操作页面，新建脚本 boot.js，将如下代码写入脚本，并点击【保存】。
var modbusIp = "127.0.0.1" //modbus ip 
var modbusPort = 503 //modbus 端口
var modbusSlaveId = 3 //modbus 主/从服务slaveId
### var modbusOffset = 0 //信号读取地址
var modbusAddrType = "4x" //信号数据类型 0x:读线圈 1x:读离散量输入 3x:读输入寄存器 4x:读保持寄存器

## function boot() {
    // 将postOrder注册为定时任务，rds服务启动4秒后，每隔10秒执行一次postOrder方法 定时读取Modbus信号
    jj.defineScheduledFunctions(true, 4000, 10000, "postOrder", [])
## }

### function postOrder(){
## // 读取 Modbus 信号值
    var signalValue = jj.readSingleModbusValue(modbusIp, modbusPort, modbusSlaveId, modbusAddrType, modbusOffset)

## // 事先约定1为信号的发起值
### // 判断读取到的信号是否为1 若为1则发起任务
### if(signalValue == 1){
## // 构造天风任务内部输入参数
## let inputParams = {
### destination:"AP1"  //机器人目标站点
## }

## // 构造实例任务输入参数
## let orderReq = {
            taskLabel:"DemoWindTask", //天风任务名
            inputParams:JSON.stringify(inputParams)
## }

## // 下发任务
        jj.createWindTask(JSON.stringify(orderReq))

## // 打印日志
        jj.scriptLog("INFO", "postOrder", "post order to wind engine of rds")
## }
## }

点击【启动】按钮， 令 RDS 系统重新加载脚本，执行 boot（） 方法，RDS 脚本引擎将加载注册的定时方法，按照配置的时间间隔执行 postOrder（） 方法。 点击【日志】按钮，打开日志输出栏，可查看方法运行过程中的日志。

### 3.2.1 利用缓存变量，避免重复创建任务实例

## 假设场景：

需要机器人从产线出货口 A 处，将料箱运送到仓库入口 B。

上节中的 signalValue 是产线出货口 A 处的光电传感器信号，当该处存在料箱时，signalValue 的值为 1。此时，希望 RDS 自动发起运输任务。

按上节的脚本代码，在机器人将 A 处的料箱取走前，由于 RDS 每次读取 signalValue 的值都是 1，将导致不断的创建任务实例。本节案例将解决此问题。

思路：针对出货口 A 处的光电传感器信号，当该处存在料箱时，signalValue 的值为 1。由于当前已经满足条件，生成任务实例， A 处料箱尚未取走，下次轮询仍将满足条件，将重复创建任务实例；因此在首次满足条件时，可在缓存中记录一个变量，再生成任务。通过该变量，可过滤 signalValue 信号，避免重复创建任务实例。然后在天风任务中，当 A 处料箱被取走后，清空该缓存变量。（提示：任务终止、异常结束需要在系统事件（任务终止事件）中复位这个变量）。

## 脚本实现

var modbusIp = "127.0.0.1" //modbus ip 
var modbusPort = 503 //modbus 端口
var modbusSlaveId = 3 //modbus 主/从服务slaveId
### var modbusOffset = 0 //信号读取地址
var modbusAddrType = "4x"    //数据类型 0x:读取线圈量 1x:读离散寄存器 3x:读取输入寄存器 4x:读取保持寄存器

## function boot() {
    // 将postOrder注册为定时任务，rds服务启动4秒后，每隔10秒执行一次postOrder方法
    jj.defineScheduledFunctions(true, 4000, 10000, "postOrder", [])
## }

### function postOrder(){
## // 读取 Modbus 信号值
    var signalValue= jj.readSingleModbusValue(modbusIp, modbusPort, modbusSlaveId, modbusAddrType, modbusOffset)
    // 生成任务之前不仅要先读取 Modbus 生成任务信号，还要读取缓存变量 name
        var flag = jj.getCacheParam("name")// 获取缓存变量 name。

    // 发起任务的信号值 == 1 && 缓存变量 name != true
    if(signalValue == 1 && flag != "true"){
## // 构造任务输入参数
## let inputParams = {
### destination:"AP1"  //机器人目标站点
## }

## // 构造任务参数
## let orderReq = {
### taskLabel:"test1", //任务名
            inputParams:JSON.stringify(inputParams)
## }

## // 发起任务
        jj.createWindTask(JSON.stringify(orderReq))
        // 将 name 变量设置为 true。此处仅举例说明，缓存变量的处理，应按实际业务需求进行。
                jj.putCacheParam("name", "true")

## // 打印日志
        jj.scriptLog("INFO", "postOrder", "post order to wind engine of rds")
## }
## }

重新创建一个天风任务，命名为“test1”，按下图填写， 需要清空缓存变量 name 。

1657160086202-e46c44eb-207c-47fd-a08b-a2596881ccc1.png

### 3.3 注册事件处理器

目前 RDS 有如下几种系统事件处理器。

agvActionDone 动作完成事件。机器人动作完成后触发该事件。
agvActionStopped 动作终止事件。机器人动作终止后触发该事件。
agvActionFailed 动作失败事件。机器人动作失败后触发该事件。
taskDone 任务完成事件。运单任务完成后触发该事件。
taskStopped 任务终止事件。运单任务终止后触发该事件。
taskFailed 任务失败事件。任务异常结束后后触发该事件。
taskManualEnd 任务人工完成事件。任务由人工完成后触发该事件。
workSiteChanged 库位数据改变事件。数据库保存的库位数据发生改变后触发该事件。
robotsStatusInfoChanged 机器人 errors 或 fatals 信息改变事件。机器人报警信息发生改变后触发该事件。
robotsStatusDispatchableChanged 机器人接单状态改变事件。机器人接单状态改变后触发该事件。
listenTcpData 监听 TCP 事件。监听到 TCP 连接传输的数据后触发该事件。
distributeDone 分拨单事件。监听小车在当前的分拨目标已完成放货后触发该事件。
distributeStatus 执行分拨单任务时，机器人在前往每一个站点或者库位的运单，状态发生改变都会触发此事件。
robotAlarm 机器人产生新的 Fatal，Error，Warning 事件。若机器人产生新的 Fatal，Error，Warning 则触发该事件。
onWebsocketMsg 监听 WebScoket 事件。若 WebScoket 客户端向当前服务端建立连接 则触发该事件。

打开脚本操作页面，新建脚本 boot.js，将如下代码写入脚本，并点击【保存】。
## 使用示例：
希望机器人动作完成后，自动获取该机器人的  Id  和库位。该需求便可以在脚本中通过注册事件方法实现。具体 步骤为：
## 1.进入在线脚本编辑界面，创建 boot（）方法。
### 2.创建 agvActionDone（param）方法，param 为机器人动作完成后向本方法传入的参数，期望机器人动作完成后能够自动触发该方法。
3.自定义 agvActionDone（）方法的实际内容，在本案例中，希望机器人动作完成后能够获取机器人 Id 和库位，因此首先将 param 转为 JSON 格式，再从 param 中获取对应的参数，具体见下文的代码示例。
### 4.在 boot（）方法中，使用 jj.registerTaskEventFunction 方法，将 agvActionDone（）方法注册为事件方法。
5.点击【启动】按钮，令 RDS 系统重新加载脚本，执行 boot（） 方法，RDS 脚本引擎加载注册的事件方法，当对应的机器人完成动作后，点击【日志】按钮，观察输出信息。
当对应的机器人完成动作后，点击【日志】按钮，观察输出信息。

## function boot() {
        // 注册事件方法，机器人动作完成后，rds触发事件执行该方法
    jj.registerTaskEventFunction("agvActionDone")
## }

function agvActionDone(param){
## //事件数据
    var paramJson = JSON.parse(param)
## //机器人id
    var agvId = paramJson["agvId"]
## //站点名称
    var siteId = paramJson["workSite"]
## //打印日志
    jj.scriptLog("INFO", "agvActionDone", "agv action done,agvId=" + agvId)

## }

### 3.3.1 注册按钮

## 1. 打开脚本操作页面，新建脚本 boot.js，将如下代码写入脚本，并点击【保存】。

## function boot() {
### //注册按钮，当点击按钮时，执行绑定事件
    jj.registerButton("button", "按钮", "buttonEvent", "red");
## }

## // 按钮事件
### function buttonEvent(){
## // 注：该方法没有参数，没有返回值
    jj.scriptLog("INFO","buttonEvent","按钮事件执行了");
## }

点击【启动】按钮， 令 RDS 系统重新加载脚本，执行 boot（） 方法，RDS 脚本引擎加载注册的事件方法，b 此时脚本页面会出现注册的按钮，如果点击按钮，则执行对应方法。 点击【日志】按钮，打开日志输出栏，可查看方法运行过程中的日志
1694587059255-38dcb9cc-4874-4214-b036-f7f4a851bc67.png

### 3.4 查询当前机器人信息

### 3.4.1 脚本调用示例

var robotInfoStr = jj.getRobotsStatus();

### 3.4.2 响应示例

https://books.seer-group.com/public/rdscore/master/zh/api/http/robot/robotsStatus.html

### 3.4.3 脚本解析示例

判断 是否有空闲机器人 ，需要使用 dispatchable 与 procBusiness 这两个字段共同判断，当 dispatchable == true && procBusiness == false 时，该机器人空闲。
## 使用示例：
 结合 3.1 节注册接口的内容，现在希望创建一个判断当前是否有空闲机器人的方法， 并能通过接口访问。
## 1.进入在线脚本编辑界面，创建 boot（）方法。
2.创建 existIdleRobot（）方法用于获取机器人否存在空闲，在 boot（）方法中，使 jj.registerHandler 方法，将 existIdleRobot（）方法注册为接口的处理方法。
### 3.定义 existIdleRobot（）方法的实际内容，具体内容见下文的代码示例。
4.点击【启动】按钮，令 RDS 系统重新加载脚本，执行 boot（） 方法，RDS 脚本引擎加载注册的事件方法，当对应的机器人完成动作后，点击【日志】按钮，观察输出信息。

## function boot() {
        jj.registerHandler("POST", "/script-api/existIdleRobot", "existIdleRobot", false);
## }

### function existIdleRobot() {
    var robotInfoStr = jj.getRobotsStatus();// 获取机器人信息
    var robotInfo = JSON.parse(robotInfoStr); //将获得的信息转为能够读取的JSON格式
    var report = robotInfo["report"]; //获取名为"report"的属性，这个属性包含了机器人的各类信息
    var availbleRobot = false;// 定义一个布尔值变量 表示是否存在空闲机器人 true:存在 false:不存在  默认为false
    for (var key in report) {//遍历"report"的每一个属性
        var dispatchable = report[key]["dispatchable"];// 获取机器人可接单的属性
        var procBusiness = report[key]["procBusiness"];// 获取是否在执行业务相关的运单的属性
        availbleRobot = dispatchable && !procBusiness;//判断是否空闲
### return availbleRobot;//返回最终结果
## }
## }

### 3.5 HTTP 请求
### 3.5.1 发起 HTTP 请求
## POST 请求
## 发起 POST 请求到以下路径：
POST http://127.0.0.1:8080/test/siteStatus
## 请求正文如下：
## {
## "type":"ALL"
## }
## 响应如下：
## [
## {
           "filled": true,
## "id": "A1"
       },
## {
           "filled": false,
## "id": "A2"
       },
## {
           "filled": true,
## "id": "C1"
       },
## {
           "filled": true,
## "id": "C2"
## }
## ]
1685957883028-7d1b86bf-c981-45c2-b7e1-e270307e4c11.png

## 脚本中添加以下代码：
## // 目标url地址
var url = "http://127.0.0.1:8080/test/siteStatus";
## function boot() {
## // 定时轮询获取目标url的数据
    jj.defineScheduledFunctions(true, 10000, 5000, "postRequest", []);
## }
## // 向目标地址发起POST请求
### function postRequest() {
## // 请求参数
## let param = {
## "type": "ALL"
    };
### // 需要将请求参数转成JSON格式进行传输
    let response = jj.requestPost(url, JSON.stringify(param));
    var projectResponse = JSON.parse(response);
    // 判断目标接口返回的http code码是否为200，一般情况下200的code码代表返回正常。
    if(projectResponse["code"] == 200) {
        // resBodyJson为接口返回的业务数据，JSON格式。后续开发者可根据业务自行解析处理
        let resBodyJson = projectResponse["body"];
        jj.scriptLog("INFO", "postRequest", resBodyJson);
## } else {
        jj.scriptLog("ERROR", "postRequest", "接口请求异常！");
## }
## }

以上脚本代码，通过发起定时轮询，向目标 URL 发起 POST 请求获取数据，返回数据进行日志打印。
1657160104773-598ad2bc-8b85-4c6a-83db-e055b3def634.png

## GET 请求
## 发起 GET 请求
## 脚本中添加以下代码：
## // 目标url地址
var url = "http://127.0.0.1:8080/test/siteStatus2";
## function boot() {
## // 定时轮询获取目标url的数据
    jj.defineScheduledFunctions(true, 10000, 5000, "getRequest", []);
## }
## // 向目标地址发起GET请求
### function getRequest() {
## // 向url发起请求
    let responseJson = jj.requestGet(url);
    jj.scriptLog("INFO", "getRequest", responseJson);  
## }

以上脚本代码，通过发起定时轮询，向目标 URL 进行 GET 请求获取数据，并打印返回的数据。
1657160123174-8aa9ed0a-f69a-4d9d-b527-64be138578f9.png

## PUT 请求
## 发起 PUT 请求到以下路径：

PUT http://127.0.0.1:8080/test/siteStatus3

## 脚本中添加以下代码：

## // 目标url地址
var url = "http://127.0.0.1:8080/test/siteStatus3";
## function boot() {
## // 定时轮询获取目标url的数据
    putRequest();
## }
## // 向目标地址发起PUT请求
### function putRequest() {
## // 请求参数
## let param = {
### "groupNames": ["1", "2", "3"]
    };
### // 需要将请求参数转成JSON格式进行传输
    let response = jj.requestPutJson(url, JSON.stringify(param));
    var projectResponse = JSON.parse(response);
    // 判断目标接口返回的http code码是否为200，一般情况下200的code码代表返回正常。
    if (projectResponse["code"] == 200) {
        // resBodyJson为接口返回的业务数据，JSON格式。后续开发者可根据业务自行解析处理
        let resBodyJson = projectResponse["body"];
        jj.scriptLog("INFO", "postRequest", resBodyJson);
## } else {
        jj.scriptLog("ERROR", "postRequest", "接口请求异常！");
## }
## }

以上脚本代码，通过发起定时轮询，向目标 URL 进行 PUT 请求获取数据，并打印返回的数据。

### ./image/PUT-putParam.png
### 3.5.2 接收上位机请求
使用 POST 在 boot 方法中注册一个接口，请求参数、响应参数等可自定义，并在下文自定义实现该方法。
## function boot() {
    // 注册一个接口，并关联到 httpDemo 方法，且不需要登录权限。
    // 前缀必须是 /script-api/，才能保证不需要登录就可以访问此接口。
    jj.registerHandler("POST", "/script-api/httpDemo", "httpDemo", false);
## }
//此方法是接口/script-api/httpDemo的具体实现
### function httpDemo(req) {
    // 解析请求参数param，请求的内容均是通过 param 字段传入。
    var requestParam = JSON.parse(req);

     // 展示请求参数。
    jj.scriptLog("info", "requestParam: ", requestParam)

    //自定义请求参数。 
### var respData="Success"

    //封装请求参数并返回 其中"body"为固定字段。
### var body={"body":respData}
## return body
## }
使用 Postman 访问该接口调用该方法 其中请求参数和响应数据可自定义 下图为请求参数和响应示例
1691401764518-210b8879-c698-44ab-87ed-b8a29a281962.png

## 在日志显示请求参数
1691401828232-6e7c14cc-8de0-4fe7-8c19-3972683658a2.png

使用 GET 方式注册一个接口，接收外部的调用请求，获取接口地址 URL 中附带的动态参数。打开【在线脚本】页面，新建脚本 boot.js，将如下代码写入脚本，并点击【保存】。
## function boot() {
## //使用GET请求调用接口
  jj.registerHandler("GET", "/script-api/{id}/EasyTransferTask", "EasyTransferTask", false); 
## }

## //获取接口里面的动态参数(id)
function EasyTransferTask(params,data){
  var paramObject = JSON.parse(data);
  let id = paramObject["id"];
### //打印我们获取到的请求路径里面动态参数的值
  jj.scriptLog("id = " + id,"","");

## }
以上脚本代码，通过 postman 测试调用脚本接口地址 http://127.0.0.1:8080/script-api/10086/EasyTransferTask ，通过 GET 请求获取数据，并打印返回的数据。
1692261692554-dd596b2a-d760-46b7-9218-d566672d98b7.png

日志打印 id 的值代表调用成功，其中 id 值为 10086，与脚本接口地址 http://127.0.0.1:8080/script-api/10086/EasyTransferTask 中的 10086 对应。
### 3.6 OPC 通信
 点击 设置 界面，添加以下代码，保存后重启 RDS 服务。
## opc:
## enable: true
## # 是否匿名连接
### isAnonymousConnect: true
## # 用户名（非匿名连接）
### serverUsername: testUser
## # 密码（非匿名连接）
### serverPassword: 123456
## #opc客户端连接服务的地址
  opcuaEndpointUrl: opc.tcp://127.0.0.1:49320
### #订阅PLC节点数据，数据变化后发布时间间隔ms
  opcuaEndpointSubInterval: 2000
## #读写重试次数，-1表示无限制
## retry: -1
## #读写重试时间间隔，ms
## retryInterval: 1000

RDS 作为客户端进行 OPC 通信， OPC 配置中主要需修改以下参数：
## opc:
## # 启用opc
## enable: true
     #opc客户端连接服务的地址，需根据实际情况修改opc服务地址
     opcuaEndpointUrl: opc.tcp://127.0.0.1:49320
## OPC 读
脚本中添加以下代码，定时轮询 OPC 通信，读取目标 PLC 的值，并打印结果值。
## function boot() {
## // 定时轮询获取opc的数据
    jj.defineScheduledFunctions(true, 10000, 5000, "readOpc", []);
## }
### function readOpc() {
    // 读取opc,第一个参数simu.test.k1是目标PLC设备的OPC标识（Identifier）
    let value = jj.readOpcValue("simu.test.k1");
    jj.scriptLog("INFO", "readOpc", "opcua读取结果值：" + value);  
## }

## 打印结果如下：
1657160142227-6356feaa-7a93-4667-968e-ab17cfb94346.png

## OPC 写
脚本中添加以下代码，定时写入 OPC ，并打印写入结果。
## function boot() {
## // 定时写入opc数据
    jj.defineScheduledFunctions(true, 10000, 5000, "writeOpc", []);
## }
### function writeOpc() {
    // 写入opc,第一个参数simu.test.k1是目标PLC设备的OPC标识（Identifier）
    // 第三个参数是代表写入的数据类型，0:String,1:Boolean,2: Word, 3:Short, 4:Long, 5: DWord
    // 此处第三个参数值3代表，写入OPC的值是一个Short类型的值
    let result = jj.writeOpcValueByType("simu.test.k1", "21", 3);
    jj.scriptLog("INFO", "writeOpc", "opcua写入结果：" + result);  
## }

## 打印结果如下：
1657160153842-e9c03a17-868a-43cf-846d-90673e638e8b.png 

方法返回 true 代表写入成功。

### 3.7 Modbus TCP 通用方法

### 3.7.1 读取 Modbus Slave 的内容

打开脚本操作页面，新建脚本 boot.js ，将如下代码写入脚本，并点击【保存】。

var modbusIp = "127.0.0.1"         //modbus ip 
var modbusPort = 502                                 //modbus 端口
var modbusSlaveId = 3                         //modbus 主/从服务slaveId
var modbusOffset = 0                                 //信号读取地址
var modbusAddrType = "4x"         //信号数据类型 0x:读线圈 1x:读离散量输入 3x:读输入寄存器 4x:读保持寄存器

## function boot() {
## // 定时读取Modbus数据
    jj.defineScheduledFunctions(true, 10000, 5000, "readSingleModbusValue", []);
## }
function readSingleModbusValue() {
### // 定时读取modbus指定地址位的值
    var result = jj.readSingleModbusValue(modbusIp, modbusPort, modbusSlaveId, modbusAddrType, modbusOffset)
    jj.scriptLog("INFO", "readSingleModbusValue", "readSingleModbusValue 读取值：" + result);  
## }

### 3.7.2 向 Modbus Slave 写入

打开脚本操作页面，新建脚本 boot.js ，将如下代码写入脚本，并点击【保存】。

var modbusIp = "127.0.0.1"         //modbus ip 
var modbusPort = 502                                 //modbus 端口
var modbusSlaveId = 3                         //modbus 主/从服务slaveId
var modbusOffset = 0                                 //信号写入地址
var modbusAddrType = "4x"         //信号数据类型 0x:写线圈  4x:写保持寄存器
## function boot() {
## // 定时写入Modbus数据
    jj.defineScheduledFunctions(true, 10000, 5000, "writeSingleModbusValue", []);
## }
function writeSingleModbusValue() {
### // 定时写入modbus指定地址位的值为1
    var result = jj.writeSingleModbusValue(modbusIp, modbusPort, modbusSlaveId, modbusAddrType, modbusOffset, 1)
    jj.scriptLog("INFO", "writeSingleModbusValue", "writeSingleModbusValue 写入成功标识：" + result);  
## }

### 3.8 发送邮件

 点击 设置 界面，发送邮件配置如下（保存成功，重启 RDS 生效） ：
## emailConfig:
  host: "smtp.qq.com"                                                // 邮件服务器
  port: 465                                                                                        // 端口号
  username: "your username"                        // 发件人邮箱
  password: "your password"                        // 密码
  protocol: "smtp"                                                        // 协议，默认 smtp

在脚本文件 boot.js 中，将如下代码写入脚本，并点击【保存】。
## function boot() {
### // 方式一：启动boot时调用一次调用
## sendMail()

    // 方式二：注册接口发送邮件。关于 jj 提供方法的详细说明，请参考《脚本方法字典》。
    jj.registerHandler("POST", "/script-api/sendMail", "sendMail", false);
## }

## //无参发送邮件，适用于固定邮件内容
### function sendMail() {
### const messageJson = {
        to: "someone@qq.com",
        subject: "Daily Meeting",
### text: "Meeting Content"
## }

    jj.sendMail(JSON.stringify(messageJson));
## }

//有参发送邮件，适用于动态邮件内容，发送内容可以通过接口指定。
### function sendMail(param) {

    // 解析 param。包括请求的内容，均是通过 param 字段传入,param 结构见下面的 body 格式。
    var paramObject = JSON.parse(param);
## var messageJson = {
        to: paramObject["to"],
        subject: paramObject["subject"],
### text: paramObject["text"]
## }

    jj.sendMail(JSON.stringify(messageJson));
## }

### 通过方式二， 通过接口发邮件时，请求体 body 格式为：
## {
  "to": "someone@qq.com",
  "subject": "Daily Meeting",
### "text": "Meeting Content"
## }
### 3.9 TCP 使用案例
### 3.9.1 发送 TCP 数据
脚本中添加以下代码，定时发送 TCP 数据，并打印发送结果。

## function boot() {
### //轮询调用sendTcpMessage方法
    jj.defineScheduledFunctions(true, 10000, 5000, "sendTcpMessage", []);
## }
## //发送消息测试方法
function sendTcpMessage(param) {
## //模拟data字符串
### var data = "mock-string"
    //调用jj.sendTcpMessage，传入三个参数，地址，端口，需要发送给对方的数据。
    var result = jj.sendTcpMessage("127.0.0.1",8888,data);
    jj.scriptLog("INFO", "sendTcpMessage", "tcp发送消息结果：" + result);
## }

## 打印结果如下
1692261748195-684c137d-dc22-4790-a56a-6d814b5a32a0.png

### 3.9.2 接收 TCP 数据
脚本 boot 方法中添加以下代码，设置端口，客户通过 RDS 的 IP 地址 和下面设置的端口 8888 可以发送信息到 RDS。 listenTcpData 方法是用来接收到客户通过 TCP 传输的数据，并可以打印出结果值。
## function boot() {
    //需要监听的端口Port，例如：8888。
    jj.registerTcpPort(8888);
    //注册监听tcp数据事件。
    jj.registerTaskEventFunction('listenTcpData');
## }
//在此方法中获取监听到的消息。每收到一次消息，调用一次该方法。
function listenTcpData(param){
    //取出发送过来的字符串。
    var data = JSON.parse(param).data
    //打印data信息，观察控制台是否收到该信息。
    jj.scriptLog("INFO","","data = " + data);
## }
本节与前一节相结合，就可以将 RDS 同时仿真成服务器和客户端。日志如下所示：
1692262117695-53f40ace-60d0-4ec0-930b-adbf5e017957.png

### 3.10 任务终止事件响应案例

在任务被手动终止时，你可以在终止后执行你想要的操作，以下在脚本中实现任务终止时，复位 modbus 的信号。

## function boot() {
  // jj.registerTaskEventFunction("taskStopped"); 是固定写法，请勿修改。
  // 通过这个方法可以监听任务被终止。
  // 关于 jj 提供方法的详细说明，请参考手册中的《脚本方法》。
  jj.registerTaskEventFunction("taskStopped");
## }

### function taskStopped(param){
### // 将param字符串转化为对象，不用修改
  var paramJson = JSON.parse(param);
  // 获取taskLabel参数，通过taskLabel参数来确定是哪一个天风任务被终止
  // taskLabel 就是在"天风任务"界面上显示的名称字段。
  var taskLabel = paramJson["taskLabel"];
  // if判断是什么taskLabel，从而执行不同的逻辑。
### if(taskLabel == "取空托盘"){
## // 这里创建一个PLC对象
    var emptyPlc = new EmptyPlatePlcConfig();
### // 对PLC进行复位，jj 方法可以在《脚本方法》中查看
    jj.writeCoilStatus(emptyPlc.host,emptyPlc.port,1,emptyPlc.arrived,false);
    jj.writeCoilStatus(emptyPlc.host,emptyPlc.port,1,emptyPlc.operationFinished,false);
## }
## }

## // 定义PLC参数
var EmptyPlatePlcConfig = (function () {
  function EmptyPlatePlcConfig() {
    this.host = "127.0.0.1";
    this.port = 501;
    this.arrived = 101;
    this.canOperation = 102;
    this.operationFinished = 103;
    this.canLeave = 104;
## }
  return EmptyPlatePlcConfig;
}());

### taskStopped 任务终止事件返回参数示例：
## {
  "status": "1001",
  "taskId": "89cbfba9-d724-46d9-a8a8-71b5a025442d",
  "taskLabel": "test",
## "taskRecord": {
    "createdOn": 1664508083000,
    "defId": "89cbfba9-d724-46d9-a8a8-71b5a025442d",
    "defLabel": "test",
    "defVersion": 6,
    "id": "45110942-95b1-466f-b405-8ceb42f699c3",
    "inputParams": "[{\"defaultValue\":\"DEFAULT MESSAGE\",\"name\":\"message\"
      ",\"label\":\"消息\",\"type\":\"String\",\"required\":false}]",
    "isDel": 0,
    "orderId": {},
    "priority": 1,
    "rootTaskRecordId": "45110942-95b1-466f-b405-8ceb42f699c3",
    "status": 1001,
    "taskDefDetail": "{\"inputParams\":[{\"name\":\"message\",\"type\":\"String\",\"label\":\"消息\",\"required\":false,\"defaultValue\":\"DEFAULT MESSAGE\"}],\"outputParams\":[],\"rootBlock\":{\"id\":-1,\"name\":\"-1\",\"blockType\":\"RootBp\",\"children\":{\"default\":[{\"id\":2,\"name\":\"b2\",\"blockType\":\"DelayBp\",\"children\":{},\"inputParams\":{\"timeMillis\":{\"type\":\"Simple\",\"value\":\"20000\"}},\"refTaskDefId\":\"\",\"selected\":false},{\"id\":1,\"name\":\"b1\",\"blockType\":\"SetSiteLockedBp\",\"children\":{},\"inputParams\":{\"siteId\":{\"type\":\"Simple\",\"value\":\"\\t 50-10-03B-01-1\"},\"ifFair\":{\"type\":\"Simple\",\"value\":true}},\"refTaskDefId\":\"\",\"selected\":false}]},\"selected\":false,\"refTaskDefId\":\"\",\"inputParams\":{}}}"
  },
## "type": 0
## }
## 常用字段说明：

## Name

## Description

## status

任务状态：1000（运行中）、1001（终止）、1002（暂停）、1003（结束）、1004（异常结束）、1005（重启异常）、1006（异常中断）、1007（手动结束）、1008（已分配）

## taskId

## 任务实例 ID

## taskLabel

## 任务名称

## createOn

## 任务创建时间

## inputParams

## 任务输入参数

## priority

## 任务优先级
### 3.11 三菱 MELSEC 通信
!!! 注意： RDS 三菱 PLC 通讯，采用 Qna 兼容 3E 帧协议实现（兼容 SLMP 的报文格式），需要先在 PLC 侧的以太网模块配置为二进制通讯 。
点击 RDS 左侧菜单"在线脚本"，默认会打开名为 "boot.js" 的脚本文件，没有则需要新建脚本 "boot.js"，将如下代码写入脚本，并点击【保存】。下述 "jj"开头的方法，可以从《脚本方法》手册中查看，这里只作完整示例的演示。
// 三菱PLC的数据主要由两类数据组成，位数据和字数据，在位数据中，例如X,Y,M,L都是位数据，字数据例如D,W。
var ip = "192.168.1.101";
var port = 3210;

## function boot() {
## // 定时读取 Melsec 的值
    jj.defineScheduledFunctions(true, 10000, 5000, "readValue", []);
## // 定时写入 Melsec 的值
    jj.defineScheduledFunctions(true, 10000, 5000, "writeValue", []);
## }

## // 读取
### function readValue() {
## // 读取 Boolean 值
    var boolValue = jj.readMelsecBoolean(ip, port, "X0");
## // 读取 Number 值
    var numValue = jj.readMelsecNumber(ip, port, "D13");
## // 读取 string 值
    var strValue = jj.readMelsecString(ip, port, "D14", 1);
    jj.scriptLog("INFO", "readValue", "读取 Boolean 值：" + boolValue);  
    jj.scriptLog("INFO", "readValue", "读取 Number 值：" + numValue);  
    jj.scriptLog("INFO", "readValue", "读取 string 值：" + strValue);  
## }

## // 写入
### function writeValue() {
## // 写入 Boolean 值
    var wirteBool = jj.writeMelsecBoolean(ip, port, "X0", true);
## // 写入 Number 值
    var wirteNumber = jj.writeMelsecNumber(ip, port, "D13", 123);
## // 写入 string 值
    var wirteString = jj.writeMelsecString(ip, port, "D14", "aa");
    jj.scriptLog("INFO", "writeValue", "写入 Boolean 值：" + wirteBool);  
    jj.scriptLog("INFO", "writeValue", "写入 Number 值：" + wirteNumber);  
    jj.scriptLog("INFO", "writeValue", "写入 string 值：" + wirteString);  
## }

## 打印结果如下：
1672051080862-e4d432dc-0fd4-4352-baff-d65f1b3285ea.png

### 3.12 通过 Websocket 协议与客户端交互
如果需要 RDS 通过 Websocket 和第三方其他设备交互，需要在 application.yml 配置文件中增加以下字段。
//在设置界面，修改该字段 控制 Websocket 服务的开启和关闭
## websocket:
## rds-endpoint:
## enabled: true

客户第三方设备需要通过 ws://rds-ip:8080/websocket/{name} 形式的请求发送信息到 RDS ，其中 {name} 表示客户可以自定义的名称。
客户发送信息后，RDS 可以在脚本中接收客户端发送过来的消息：
## function boot() {
## //注册为系统事件
    jj.registerTaskEventFunction("onWebsocketMsg")
## }

function onWebsocketMsg(params){

    // jj 方法的详细介绍可查看《脚本方法》手册。

### //打印客户端请求的参数，params 就是发送过来的消息
    jj.scriptLog("INFO","params",params)
## //获取所有客户端ip
        var ips=jj.getWebsocketClientIp()
        jj.scriptLog("INFO","ips",ips)
## //获取所有客户端注册名
        var names=jj.getWebsocketClientName()
        jj.scriptLog("INFO","names",names)
### //根据ip，按照需求，向客户端发送响应
        jj.sendMsgToWscByClientIp("success connect", "127.0.0.1")
## //根据客户端名称，向客户端发送响应
        jj.sendMsgToWscByClientName("success connect", "clientName")
## }
## 打印结果如下：
1692262019453-6b1b0ba7-8edd-4b5d-8fd6-f87fb5f8cb1d.png

### 3.13 通过监听 西门子 S7 PLC 信号创建任务实例

打开脚本操作页面，新建脚本 boot.js，将如下代码写入脚本，并点击【保存】。
var PLCType = "S1500" // PLC 类型，可选值（S1200,S300,S400,S1500,S200Smart,S200）
var ip = "192.168.8.65" // PLC IP
## function boot() {
    // 注册运单方法，rds服务启动3秒后，每隔2秒执行一次运单方法
    jj.defineScheduledFunctions(true, 3000, 2000, "postOrder", [])
## }

### function postOrder(){
### // 读取 地址位：DB1.12.5 的信号值
    var signalValue = jj.readS7Boolean(PLCType, ip, "DB1.12.5");

## // 信号是发起任务的值
### if(signalValue == true){
## // 构造任务输入参数
## let inputParams = {
### destination:"AP1"  //机器人目标站点
## }

## // 构造任务参数
## let orderReq = {
            taskLabel:"DemoWindTask", //任务名
            inputParams:JSON.stringify(inputParams)
## }

## // 发起任务
        jj.createWindTask(JSON.stringify(orderReq))

## // 打印日志
        jj.scriptLog("INFO", "postOrder", "post order to wind engine of rds")
## }
## }

点击【启动】按钮， 令 RDS 系统重新加载脚本，执行 boot（） 方法，RDS 脚本引擎将加载注册的定时方法，按照配置的时间间隔执行 postOrder（） 方法。 点击【日志】按钮，打开日志输出栏，可查看方法运行过程中的日志。
编写天风任务。

1692870111878-d76b56bb-97a3-4fcb-84c6-92cd7ac31363.png

## 四，手持操作端开发实战案例

### 4.1 手持端配置说明

## operator:
### # 是否允许动态修改手持端首页菜单属性，默认为不允许
### runtimeMenuPropsUpdate: false
## # 岗位数组
## workTypes:
## # 岗位id
## - id: COMMON-TYPE
## # 岗位名
## label: 通用岗位
## # 工位数组
## workStations:
## # 工位id
### - id: COMMON-STATION
## # 工位名
## label: 通用工位
## # 绑定岗位，未绑定时则不显示
## type: COMMON-TYPE
## # 需求单配置
## demandTask:
    # 是否启用需求单功能，true:启用，false:禁用，默认为禁用
## enable: true
## # 手持端需求单标签页的名称
## tabName: 需求单
    # 是否显示需求单中的删除按钮，true:显示，false:隐藏，默认为隐藏
### showDeleteButton: false
## # 需求单响应方法配置
## processFuncList:
## # 需求单数据的名称
### - defLabel: chooseSite
### # 需求单详情页点击"下单"后，对应的脚本响应方法名
### funcName: findSiteByGroupName
## # 数据单
## tableShow:
    # 是否启用数据单，true:启用，false:禁用，默认为禁用
## enable: true
## # 数据单自定义名称
## tabName: 数据单
## showSql:
## # 数据单唯一id
## - id:  ts
              # 执行的sql语句 AND LIKE HAVING WHERE 必须是大写 
        sql: select site_id,locked,locked_by from t_worksite WHERE site_id = {site_id}
## # 查询语句自定名称
## label: 库位展示
## # sql语句注册的查询条件参数
## params: [site_id]
        # 查询参数的类型，目前只支持string、number两种。和params先后顺序一一对应
### paramsType: [string]
## # 查询参数显示的信息
### paramsTipName: [库位id]
## # 手持端表格显示字段
## tableHead:
          - value: site_id      # 需要显示的查询字段
            label: 库位id        # 手持端页面表头显示名称
          - value: locked       # 需要显示的查询字段
            label: 是否锁定       # 手持端页面表头显示名称
## # 字段值转成需要展示的文字
## status:
            # 当locked这个字段值为0时，页面对应的要显示成未锁定，当值为1时，页面对应的要显示成锁定
## - value: 0
## label: 未锁定
## - value: 1
## label: 锁定
          - value: locked_by   # 需要显示的查询字段
            label: 锁定者       # 手持端页面表头显示名称
## # 任务菜单数组
## orders:
## # 菜单id
### - menuId: moveTaskChain
## # 菜单名
## label: 运输任务链
## # 下单的响应方法(POST)路由
### route: moveTaskChainFunc
## # 菜单背景颜色
### menuItemBackground: '#ffff33'
## # 菜单文字颜色
### menuItemTextColor: '#aff2dd'
### # 菜单关联的任务定义，用于脚本进行方法（GET）路由
### robotTaskDef: moveTaskChain
      # 是否禁用菜单，true为禁用，默认为启用。禁用菜单后，菜单将变成灰色，不能点击下单。
## disabled: false
## # 可查看该菜单的岗位数组
### workTypes: [ COMMON-TYPE ]
## # 可查看该菜单的工位数组
      workStations: [ COMMON-STATION ]
      # 下单时是否隐藏查询按钮，true：隐藏，false：显示，默认为隐藏。
## canSendTask: true
## # 下单界面首部的提示
## tip: 运输任务链
## # 点击下单按钮后，弹出的提示性文本
### confirmMessage: 确定发起运输任务链吗？
## # 菜单组件列表
## params:
## # 菜单组件名
## - name: fromSite
## # 菜单组件类型如下:
          # text：单行文本，textarea：多行文本，select：单选下拉菜单，list：多选下拉菜单
## input: text
## # 菜单组件提示文字
## label: 物料缓存库位
## - name: matSite
## input: select
## label: 放置物料的库位
          # 静态下拉菜单，该下拉菜单的所有选项需事先在配置文件中配置完成
## options:
## # 下拉菜单选项值
## - value: buf-1
## # 下拉菜单选项文字
## label: 物料缓存位1
## - value: buf-2
## label: 物料缓存位2
## - name: traySite
## input: select
## label: 选择空托盘的库位
          # 动态下拉菜单。它的数据来源自脚本的getForkSiteList方法，即在脚本中要定义名为
### # getForkSiteList的数据接口
          dynamicOptionsSource: getForkSiteList
          # 下拉菜单是否进行懒加载。true：进行懒加载，false：放弃懒加载。
          # 下拉菜单懒加载意味着用户只有点击下拉菜单时，下拉菜单才会渲染其中的选项数据，
          # 它的数据不会提前加载。
### lazyGetSource: false
## - name: toSite
## input: select
## label: 放置空托盘的库位
## options:
## # 下拉菜单选项值
## - value: tray-1
## # 下拉菜单选项文字
## label: 托盘缓存位1
## # 静态多级联动菜单配置
## checkParent:
                  # matSite为该下拉菜单选项绑定的同级菜单组件名，一旦用户在名
                                  # 为matSite的下拉菜单中变更了选项，该下拉菜单的选项将跟着变化
## - parent: matSite
                  # 用户在名为matSite的下拉菜单中选择了值为buf-1的选项，
                  # value: tray-1的下拉菜单选项就会动态显示出来供用户选择
## value: [ buf-1 ]
## - value: tray-2
## label: 托盘缓存位2
## checkParent:
## - parent: matSite
## value: [ buf-2 ]
### - name: actionRecord
## # 多行文本
## input: textarea
## # 多行文本框的行数
## multiple: 3
## # 菜单组件提示文字
## label: 操作记录
## - name: componentId
## input: text
## label: 组件id
          # hidden 属性代表名为 componentId 的菜单组件在界面上隐藏
## hidden: true
          # 组件隐藏时，默认填充的值（适用于 text 和 textarea 类型的组件）
## defaultValue: "id1"

### 4.2 创建基本运输任务
### 4.2.1 用配置文件创建基本运输任务
## 1.创建天风任务，如下图所示：
1657160239656-7485d322-328d-4331-8522-ede0397d74b2.png

 将任务命名为 EasyTransferTask 。为其设置两个输入参数 from ， to ，字符串类型。将第一个参数 from 填入【关键路径】内。
并将这两个参数，分别填入两个【机器人通用动作】块的【目标站点名】内。

1657160260694-17a37ca6-2064-42f9-812d-409dd99cb3e1.png

### 2.切换到【设置】界面，按以下方式配置：
## rdscore:
  baseUrl: http://127.0.0.1:8088

## operator:
## orders:
### - menuId: moveTaskChain
### label: 简单运输任务 # 手持端选项的提示说明
      menuItemBackground: '#32CD32' # 手持端选项的背景颜色
      menuItemTextColor: black  # 手持端字体的颜色
      route: EasyTransferTask # boot.js 脚本里面，对手持端下单的响应方法(POST)路由
## tip: 简单运输任务
      confirmMessage: 确定下单吗？  # 点击下单按钮之后的弹窗提示
## params:
## - name: from
          input: text   # 采用输入或扫码的方式录入参数
## label: 请选择起点库位
## - name: to
          input: text   # 采用输入或扫码的方式录入参数
## label: 请选择终点库位

### 3.打开【在线脚本】页面，将如下代码写入脚本，并点击【保存】 。
注意：后面有//------更改 的代表着根据需求可能需要修改。没有的话代表无需更改。

## function boot() {
## //按手持端配置里面写的路由进行注册
    //下例为：手持端配置中 route:EasyTransferTask 。故此处写/EasyTransferTask，后面"EasyTransferTask"，引号的内容也需要与route里面配置的一致
    //若手持端配置中 route:task。则应写成  jj.registerHandler("POST", "/script-api/task", "task", false);
    jj.registerHandler("POST", "/script-api/EasyTransferTask", "EasyTransferTask", false); //------更改

## }

//注册完成后，需要通过下方函数实现，函数名根据自己配置的路由，在此例中路由配置为route:EasyTransferTask
### //则实现的函数也为EasyTransferTask
function EasyTransferTask(param){//------更改

    //注意！！！！！！！后面有//------更改 的代表着可能需要修改。没有的话代表无需更改

### //1.对手持端参数进行处理，转换格式(无需更改)
    jj.getLogger().info("下单接口参数：" + param);
    var paramJson = JSON.parse(param);
    var params = paramJson["params"];

### /*2.读取手持端传来的参数，(依据实际情况进行更改)
    若传入参数个数等于1：保留let from = params[0]["value"];，将let to = params[1]["value"];删除。
    若传入参数等于2：保持原状即可。
    若传入参数大于2；则按规则增加代码，例如：传入参数为3个，则增加 let site = params[2]["value"];。

### 在读取手持端传来的参数时，在一下几点需要注意：
    1.等号左边参数的命名，对应于天风任务中[任务输入参数]-->[变量名]，在本例中，天风任务写的传入变量名分别为 from , to.故在此脚本中，也命名为 from , to。
### 2.params[]中的数值0对应的是第一个参数，1对应的是第二个参数，以此类推第三个参数则为params[2]。
## */
    let from = params[0]["value"];//------更改
    let to = params[1]["value"];//------更改

    /*3.判断输入参数为空，假如参数有任一参数为空，则在手持端进行提示。
## 1.if (from == "" || to == "") 这一行，需要根据实际的参数数量增删，以及根据实际参数的命名进行变更。
### 2. ||  此符号表达的意思是 或运算 ，符号两边只要有任意一个条件成立，则执行下述代码
### 3.如果有三个或以上参数需要判断，则表达式为 ：if (... || ... || ... || ...)
### 4.operatorResponse.msg = "请输入起点库位或终点库位"; 根据实际需要的弹窗信息进行更改

## */
    if (from == "" || from == null || to == "" || to == null) { //------更改
        jj.getLogger().info("参数为空！！！！！1");
        var res = { code:400, msg: "请输入起点库位或终点库位" }
        return { code: 400, body: JSON.stringify(res) };
## }

## try {

        //4.将前面代码获取到的 from，to 塞入inputParams里面,需要注意的点如下
## /*
## 1. :号左右两边，命名严格一致，都对应于天风任务中[任务输入参数]-->[变量名]的命名
### 2. 如果需要增加，例如天风任务中[任务输入参数]-->[变量名]，除了有 from，to 之外还有 site ，则增加一下代码site: site,
### 3. 如果需要删除，直接删除对应行即可

## */
## let inputParams = {
### from: from, //------更改
## to: to, //------更改
        };

## //5.生成天风任务
        //需要将 taskLabel: 后面的内容改成对应的天风任务的任务名，此例为 EasyTransferTask。

## let taskParam = {
            taskLabel: "EasyTransferTask",//------更改
            inputParams: JSON.stringify(inputParams),
        };
        jj.createWindTask(JSON.stringify(taskParam));

        var res = { code:200, msg: "下单成功" }
        return { code: 200, body: JSON.stringify(res) };
## } catch (error) {
        jj.getLogger().error("create tss task error", error);
        var res = { code:400, msg: "fail" }
        return { code: 400, body: JSON.stringify(res) };
## }

## }
1657160277908-8223aa6d-8179-4d69-b73b-ccb3397bc565.png

### 4.打开手持端界面进行下单。
1653452439693-1db56827-a745-4b83-b99d-d7e7d5fc71ef.png

### 4.2.2 用功能配置创建基本运输任务
## 1.创建天风任务，如下图所示：
1692265391332-0d8a5456-78d9-4902-a8ad-0006a60a1cc0.png

将任务命名为 EasyTransferTask 。为其设置两个输入参数 from ， to ，字符串类型。将第一个参数 from 填入【关键路径】内。
并将这两个参数，分别填入两个【机器人通用动作】块的【目标站点名】内。
1692265546819-7a548aa3-8afc-44e5-a6b5-084d63012fdd.png

### 2.打开【设置】界面，点击功能配置按钮，进入功能配置页面。
1692265620376-639dc395-0fe1-4512-a55c-c051e7d3613b.png

点击加号，新建一个菜单按钮，命名为简单运输任务。
1692265657661-a2fd8189-3d7e-4252-b1f5-715d560fa5c7.png

### 3.展开任务菜单，点击简单运输任务，进入任务菜单编辑界面。
1692265717205-13009002-3f10-4528-a40c-b0aad4a65d6c.png

### 4.点击屏幕中间预览页的简单运输任务按钮，右侧会弹出其属性。
1692265746025-029b8c61-00db-41df-9216-ceac566ae01d.png

### 5.展开简单运输任务按钮，进入详情页，编辑详情页的显示信息。
1692265774027-96ade9df-d762-4379-bd0f-bf858da113bf.png

我们可以根据需要将组件拖入详情页中，然后配置其属性。
1692265798094-1a070ef0-3ab9-436e-b4d0-2f7bbcb2176f.png

这里我们拖入两个单行输入组件，然后点击单行输入组件，在右侧编辑属性。
1692265822193-bf28a178-a654-44c4-b686-f05372665fd6.png

1692265848714-d597e8b0-080c-40f1-aecd-390f1ac67491.png

### 6.点击【下单】按钮，配置其属性：路由就是下单按钮的响应方法（POST）路由，该方法在脚本中写好了，二次确认提示文本是下单后的提示信息，点击【保存】按钮。
1692265876853-c685f6ba-93d8-4537-8e01-fb065045a5ae.png

### 7.打开【在线脚本】页面，将如下代码写入脚本，并点击【保存】。
 注意：后面有//------更改 的代表着根据需求可能需要修改。没有的话代表无需更改。
## function boot() {
## //按手持端配置里面写的路由进行注册
    //下例为：手持端配置中 route:EasyTransferTask 。故此处写/EasyTransferTask，后面"EasyTransferTask"，引号的内容也需要与route里面配置的一致
    //若手持端配置中 route:task。则应写成  jj.registerHandler("POST", "/script-api/task", "task", false);
    jj.registerHandler("POST", "/script-api/EasyTransferTask", "EasyTransferTask", false); //------更改

## }

//注册完成后，需要通过下方函数实现，函数名根据自己配置的路由，在此例中路由配置为route:EasyTransferTask
### //则实现的函数也为EasyTransferTask
function EasyTransferTask(param){//------更改

    //注意！！！！！！！后面有//------更改 的代表着可能需要修改。没有的话代表无需更改

### //1.对手持端参数进行处理，转换格式(无需更改)
    jj.getLogger().info("下单接口参数：" + param);
    var paramJson = JSON.parse(param);
    var params = paramJson["params"];

### /*2.读取手持端传来的参数，(依据实际情况进行更改)
    若传入参数个数等于1：保留let from = params[0]["value"];将let to = params[1]["value"];删除。
    若传入参数等于2：保持原状即可。
    若传入参数大于2；则按规则增加代码，例如：传入参数为3个，则增加let site = params[2]["value"];。

### 在读取手持端传来的参数时，在一下几点需要注意：
    1.等号左边参数的命名，对应于天风任务中[任务输入参数]-->[变量名]，在本例中，天风任务写的传入变量名分别为 from , to.故在此脚本中，也命名为 from , to。
### 2.params[]中的数值0对应的是第一个参数，1对应的是第二个参数，以此类推第三个参数则为params[2]。
## */
    let from = params[0]["value"];//------更改
    let to = params[1]["value"];//------更改

    /*3.判断输入参数为空，假如参数有任一参数为空，则在手持端进行提示。
## 1.if (from == "" || to == "") 这一行，需要根据实际的参数数量增删，以及根据实际参数的命名进行变更。
### 2. ||  此符号表达的意思是 或运算 ，符号两边只要有任意一个条件成立，则执行下述代码
### 3.如果有三个或以上参数需要判断，则表达式为 ：if (... || ... || ... || ...)
### 4.operatorResponse.msg = "请输入起点库位或终点库位"; 根据实际需要的弹窗信息进行更改

## */
    if (from == "" || from == null || to == "" || to == null) { //------更改
        jj.getLogger().info("参数为空！！！！！1");
        var res = { code:400, msg: "请输入起点库位或终点库位" }
        return { code: 400, body: JSON.stringify(res) };
## }

## try {

        //4.将前面代码获取到的 from，to 塞入inputParams里面,需要注意的点如下
## /*
## 1. :号左右两边，命名严格一致，都对应于天风任务中[任务输入参数]-->[变量名]的命名
### 2. 如果需要增加，例如天风任务中[任务输入参数]-->[变量名]，除了有 from，to 之外还有 site ，则增加一下代码site: site,
### 3. 如果需要删除，直接删除对应行即可

## */

## let inputParams = {
### from: from, //------更改
## to: to, //------更改
        };

## //5.生成天风任务
        //需要将 taskLabel: 后面的内容改成对应的天风任务的任务名，此例为 EasyTransferTask。

## let taskParam = {
            taskLabel: "EasyTransferTask",//------更改
            inputParams: JSON.stringify(inputParams),
        };
        jj.createWindTask(JSON.stringify(taskParam));

        var res = { code:200, msg: "下单成功" }
        return { code: 200, body: JSON.stringify(res) };
## } catch (error) {
        jj.getLogger().error("create tss task error", error);
        var res = { code:400, msg: "fail" }
        return { code: 400, body: JSON.stringify(res) };
## }

## }
1692265984176-11aac5e5-cd6a-4595-9353-ea77abe81731.png

### 8.打开手持端界面进行下单。
1692266027387-3905ae3d-ec40-4635-b43f-95cbdf24eb8a.png

### 4.3 下单时通知任务发起者

注意：1。以下描述的两例，全都包含在创建基本任务的脚本中，具体见【创建基本运输任务】。

### 2.后面有//------更改 的代表着根据需求可能需要修改，没有的话代表无需更改。

## 1.任务发起成功通知任务发起者。

    operatorResponse.code = 200;
    operatorResponse.msg = "下单成功";
    response.body = JSON.stringify(operatorResponse);
    return response;

### 2.任务发起失败时通知任务发起者。

---由于有输入参数为空导致任务发起失败。

    let from = params[0]["value"];//------更改
    let to = params[1]["value"];//------更改

    /*3.判断输入参数为空，假如参数有任一参数为空，则在手持端进行提示。
## 1.if (from == "" || to == "") 这一行，需要根据实际的参数数量增删，以及根据实际参数的命名进行变更。
### 2. ||  此符号表达的意思是 或运算 ，符号两边只要有任意一个条件成立，则执行下述代码
### 3.如果有三个或以上参数需要判断，则表达式为 ：if (... || ... || ... || ...)
### 4.operatorResponse.msg = "请输入起点库位或终点库位"; 根据实际需要的弹窗信息进行更改

## */
    if (from == "" || to == "") { //------更改
        jj.getLogger().info("参数为空！！！！！1");
        operatorResponse.code = 400; 
        operatorResponse.msg = "请输入起点库位或终点库位";//------更改
        response.body = JSON.stringify(operatorResponse);
        return response;
## }

### 4.4 设置一个静态下拉菜单参数

## 1.【设置】界面如下：
## operator:
## orders:
### - menuId: EasyTransferTask
### label: 简单运输任务 # 手持端选项的提示说明
      menuItemBackground: '#32CD32' # 手持端选项的背景颜色
      menuItemTextColor: black  # 手持端字体的颜色
      route: EasyTransferTask # 该选项绑定的路由
## tip: 简单运输任务
      confirmMessage: 确定下单吗？  # 点击下单按钮之后的弹窗提示
## params:
### - name: from        # 代表起点
### input: select   # 下拉框
## label: 请选择起点库位
## options:
### - value: A-from # 下拉框代表的值
### label: A号起点 # 下拉框的描述
## - value: B-from
## label: B号起点

### - name: to           # 代表终点
### input: select   # 下拉框
## label: 请选择起点库位
## options:
## - value: C-to
## label: C号终点
## - value: D-to
## label: D号终点
### 2.其他部分详见【创建基本运输任务】，将此配置文件替换 【创建基本运输任务】的配置文件即可。
### 3.手持端界面显示：
1653452452318-da9c6d12-21d4-49f3-b7fa-9e52d70f24f9.png

起点下拉框固定显示：（配置文件中配置了起点为：A 号起点和 B 号起点两个）。
1653452462140-16cfd639-60db-4c9d-879f-96b4f1622e9d.png

终点下拉框固定显示：（配置文件中配置了终点为：C 号终点和 D 号终点两个）。
1653452469740-415544bc-3c80-4454-838c-82b5ce49ba0e.png

### 4.5 设置一个静态联动的下拉菜单参数
## 1.【设置】页面填入如下内容：
## operator:
## orders:
### - menuId: EasyTransferTask
### label: 简单运输任务 # 手持端选项的提示说明
      menuItemBackground: '#32CD32' # 手持端选项的背景颜色
      menuItemTextColor: black  # 手持端字体的颜色
      route: EasyTransferTask # 该选项绑定的路由
## tip: 简单运输任务
      confirmMessage: 确定下单吗？  # 点击下单按钮之后的弹窗提示
## params:
## - name: from
### input: select   # 下拉框
## label: 请选择起点库位
## options:
### - value: A-from # 下拉框代表的值
### label: A号起点 # 下拉框的描述
## - value: B-from
## label: B号起点

                # 终点下拉框通过checkParent来实现静态联动(后一个下拉框显示的内容和前一个下拉框内容有关)
## - name: to
### input: select   # 下拉框
## label: 请选择起点库位
## options:
## - value: C-to
## label: C号终点
### checkParent: # checkParent
                - parent: from # 与哪一个下拉框绑定，在本例中与 name 参数为 from 的下拉框绑定
                  value: [A-from] # 当 from 下拉框选择 A-from 时，本下拉框 C-to 可以显示
## - value: D-to
## label: D号终点
## checkParent:
## - parent: from
                  value: [A-from] # 当 from 下拉框选择 A-from 时，本下拉框 D-to 可以显示
## - value: E-to
## label: E号终点
## checkParent:
## - parent: from
                  value: [B-from] # 当 from 下拉框选择 B-from 时，本下拉框 E-to 可以显示
### 2.其他部分详见【创建基本运输任务】，将此配置文件替换 【创建基本运输任务】的配置文件即可。
### 3.手持端界面显示：
1653452482865-962cd321-0930-4266-bd56-1f1f1d2fd35a.png

当起点库位选择 A 号起点，即 A-from 时，终点下拉框显示为：
1653452492007-dd064a56-4af2-4142-aae5-27a33266153f.png

当起点库位选择 B 号起点，即 B-from 时，终点下拉框显示为：
1653452499754-15f36c45-f678-495e-b0cc-06a11e5825ad.png

### 4.6 创建修改库位状态任务

## 1、在 设置 界面中添加

## operator:
## orders:
### - menuId: updateSite
## label: 库位设置
### menuItemBackground: red
### menuItemTextColor: white
## # 下单的响应方法(POST)路由
## route: updateSite
## disabled: false
## canSendTask: true
## tip: 库位设置
### confirmMessage: 确定要执行吗？
## params:
## - name: siteId
## input: select
## label: 库位Id
## options:
## - value: Loc-01
## label: 库位1
## - value: Loc-02
## label: 库位2
## - name: content
## input: select
## label: 库位内容
## options:
## - value: ""
## label: 清空
## - value: EmptyTray
## label: 空托盘
## - value: 满料架
## label: 满料架

### 2、打开脚本操作页面，新建脚本 boot.js，将如下代码写入脚本，并点击【保存】。

## function boot() {
    // updateSite和①中的route保持一致，"/script-api/"为固定字符串，后面的可以自定义保持和route一直
    jj.registerHandler("POST", "/script-api/updateSite", "updateSite", false);
## }
### function updateSite(params) {
## try {
## // 选择修改的库位id
        var siteId = JSON.parse(params)["params"][0]["value"];
## // 设置库位内容
        var content = JSON.parse(params)["params"][1]["value"];
## var inputParams = {
            "siteId": siteId,
## "content": content
        };
## //创建任务
## var taskParam = {
## // 天风任务的名称
            "taskLabel": "updateSite",
## // 天风任务生成的需要的入参
            "inputParams": JSON.stringify(inputParams)
        };
        jj.createWindTask(JSON.stringify(taskParam));
        var res = { code: 200, msg: "设置成功" };
        return { code: 200, body: JSON.stringify(res) };
## }
## catch (error) {
        var res = { code: 400, msg: "设置失败" };
        return { code: 400, body: JSON.stringify(res) };
## }
## }

### 3、 新建天风任务
## 设置任务的输入参数
1700193903967-aa34e052-b199-431e-bec9-638e78d88af0.png

1700193744402-d4de53ee-5378-4f70-89a8-d24085aac5f7.png

上图中 ① ② 处任务输入参数和 2 中的 inputParams 参数名保持一致

### 4.7 设置一个动态下拉菜单参数

## 1、在 设置 界面如下配置

## operator:
## orders:
## - menuId: dynamic
## label: 动态下拉菜单
### menuItemBackground: red
### menuItemTextColor: white
## disabled: false
## # 下单的路由
## route: updateSite
## tip: 动态
### confirmMessage: 确定要执行吗？
## params:
## - name: siteId
## input: select
## label: 库位
## # 绑定脚本的路由post
          dynamicOptionsSource: getForkSiteList
## lazyGetSource: true

### 2、打开脚本操作页面，新建脚本 boot.js，将如下代码写入脚本，并点击【保存】。

## function boot() {
         // /script-api/l2-operator/ 为固定字符串，getForkSiteList与上文的1中dynamicOptionsSource参数的值保持一致
    jj.registerHandler("POST", "/script-api/l2-operator/getForkSiteList", "getForkSiteList", false);
## }

function getForkSiteList(params) {
   // 动态下拉菜单的返回值数据结构一定为数组对象，如下案例'list'
## var list = [{
            value: "option1",
## label: "选项一"
        },
## {
            value: "option2",
## label: "选项二"
        },
## {
            value: "option3",
## label: "选项三"
        },
## {
            value: "option4",
## label: "选项四"
        },
## {
            value: "option5",
## label: "选项五"
        }];
    return { code: 200, body: JSON.stringify(list) };
## }

1660715653336-79d514f6-8468-4164-a20f-b8ef956fd93f.png

### 4.7.1 在下拉菜单中显示库位信息

## 1.在 设置 界面配置如下

## operator:
## orders:
### - menuId: setStockNumber
## label: 配置备货完成数量
### menuItemBackground: #00CCCC
### menuItemTextColor: black
### robotTaskDef: setStockNumber
## disabled: false
### workTypes: [ COMMON-TYPE ]
### workStations: [store ]
## tip: 下单
### confirmMessage: 确定下单
## params:
## -  name: workSites
### # 需将input的类型设置为worksite
## input: worksite
## label: 库位情况
## # 读取库区库位信息,可设置多个库区
## groupNames:
## - ProductSite

## 打开手持端显示库位信息：

1660114013565-8a5e5f2a-0054-4ac1-afe1-c75ac3115ad6.png 

### 4.7.2 为下拉菜单的选项设置背景色

## 1.在 设置 界面的 options 选项中添加 color 字段设置颜色

## operator:
## orders:
### - menuId: setStockNumber
## label: 配置备货完成数量
### menuItemBackground: #00CCCC
### menuItemTextColor: black
### robotTaskDef: setStockNumber
## disabled: false
### workTypes: [ COMMON-TYPE ]
### workStations: [store ]
## tip: 下单
### confirmMessage: 确定下单
## params:
## - name: site
## input: select
## label: 库位信息
### lazyGetSource: false
## options:
## - label: 库位1
## value: location1
## # 颜色格式为十六进制
## color: #9999CC
## - label: 库位2
## value: location2
## color: #32CD32
## defaultOption: true
## - label: 库位3
## value: location3
## color: #3CB371

### 2.打开手持端显示下拉选项对应的颜色

1660114114354-2a89dd96-c026-4522-ba49-21fa66405836.png 

### 4.8 在任务中通知某岗位/工位/用户

## 1、通知用户在天风任务中使用'通知手持端的用户'块

1657160331861-c8333159-bcdc-4952-88ab-3966010ad464.png

### 2、通知工位/岗位

## 1、在 设置 界面添加岗位、工位配置

## operator:
## # 岗位数组
## workTypes:
## # 岗位id
## - id: WS
## # 岗位名
## label: WS
## # 工位数组
## workStations:
## # 工位id
## - id: WT
## # 工位名
## label: WT

### 2、手持端先绑定工位/岗位
1653452846647-431666b2-84b3-4c4e-bc1d-cce7740b474d.png

### 3、天风任务使用 '通知手持端' 块，推送岗位、工位、岗位&&工位

1657160341614-27294228-7c9e-4408-b238-a3c71f650b17.png

### 4.9 将任务关联到岗位/工位/用户
## 1. 天风任务使用 '设置工位岗位' 块
1663919770528-69058fd5-09e3-4272-9b27-37863c14ca74.png

## 设置手持端工位岗位
1663920080722-94d317fe-c208-48a3-b433-ef1205317853.png

## 任务界面查看我的任务

1663919800371-b3c7f9c3-5a51-4603-9ceb-49f75ff64fa9.png

### 4.10 创建需求单

## 案例需求描述
 线边工位发起叫料需求，并选择一个线边的库位作为叫料点；仓库管理员收到需求后，选择仓库的一个库位作为物料发货点，然后发起从仓库到线边的物料搬运任务。
该需求可以使用需求单的功能实现。
## 设置 界面开启需求单功能
## operator:
## workTypes:
## - id: COMMON-TYPE
## label: 通用岗位
## workStations:
### - id: COMMON-STATION
## label: 通用工位
## # 需求单配置
## demandTask:
## # 启用需求单
## enable: true
## # 手持端需求单标签页的名称
## tabName: 需求单
## processFuncList:
## # 需求单页面中工单的名称
### - defLabel: callMatFromBuf
### # 需求单该工单下单后，对应的脚本响应方法名
## funcName: dealOrder
        # 需求单扩展属性，配置后使用脚本方法添加需求单时，可设置扩展属性
## extensionFields:
### - id: 1 # id 字段, 必须唯一值，不可重复
### attributeName: 扩展字段1
## - id: 2
### attributeName: 扩展字段2

以上 设置 界面配置参数中，“enable: true” 启用需求单功能，开启后手持端将出现需求单标签页面。tabName 字段可自定义需求单标签页的名称。
1653452928008-79138e8c-dcd3-4350-a39a-56c7d8dcfee1.png

 设置 界面中增加用于线边工位叫料的菜单。
## operator:
## workTypes:
## - id: COMMON-TYPE
## label: 通用岗位
## workStations:
### - id: COMMON-STATION
## label: 通用工位
## # 需求单配置
## demandTask:
## # 启用需求单
## enable: true
## # 手持端需求单标签页的名称
## tabName: 需求单
## processFuncList:
## # 需求单页面中工单的名称
### - defLabel: callMatFromBuf
### # 需求单该工单下单后，对应的脚本响应方法名
## funcName: dealOrder
        # 需求单扩展属性，配置后使用脚本方法添加需求单时，可设置扩展属性
## extensionFields:
## - id: 1
### attributeName: 扩展字段1
## - id: 2
### attributeName: 扩展字段2
## orders:
### - menuId: callMatFromBuf
## label: 仓库出货
### robotTaskDef: callMatFromBuf
### route: addDemandData
### menuItemBackground: #ffff33
### workTypes: [ COMMON-TYPE ]
      workStations: [ COMMON-STATION ]
## tip: 仓库出货
### confirmMessage: 确定从仓库出货吗？
## params:
## - name: targetSite
## input: select
## label: 叫料库位
## options:
## - value: PS-1
## label: 线边库位1
## - value: PS-2
## label: 线边库位2

 以上配置参数中，“menuId: callMatFromBuf” 开始的部分正是新增的叫料菜单参数。此时刷新手持端首页，可以看到多了一个名叫“仓库出货”的菜单，点击菜单后可以选择一个叫料点库位。 
1653453145942-2b67c6dd-b768-431c-a46d-6070b78343ef.png 

1653453155137-ab6e6128-0d1c-4168-a94b-27266d5f1309.png

脚本中增加处理线边叫料的响应方法。
## function boot() {
### // "仓库出货"菜单的下单按钮的响应方法进行注册
  jj.registerHandler("POST", "/script-api/addDemandData", "addDemandData", false);
## }

function addDemandData(param) {
  var paramObject = JSON.parse(param);
  let response = new ScriptRepsonseEntity();
  let operatorResponse = new OperatorRepsonseEntity();
## // 创建需求单
  let robotDefLabel = paramObject["robotTaskDef"];
  let menuId = paramObject["menuId"];
## let addParam = {
## // 需求单数据名称
    defLabel: robotDefLabel,
    description: "仓库出货",
### // 需求单补充内容对应的菜单id，需在yml配置中存在
    menuId: "chooseMatSite",
## // 需求单数据可见的岗位
    workTypes: "COMMON-TYPE",
## // 需求单数据可见的工位
    workStations: "COMMON-STATION",
    createdBy: menuId,
## // 需求单内容对象
    content: paramObject,
### // 需求单扩展字段，需要在配置文件中配置
        attrList: [{"attributeName": "扩展字段1","attributeValue": "123"},{"attributeName": "扩展字段2","attributeValue": "123"}]
  };
## // 创建需求单数据
  jj.addDemand(JSON.stringify(addParam));

  response.body = JSON.stringify(operatorResponse);
  return response;
## }
### class OperatorRequestEntity {
## constructor() {
    this.robotTaskDef = "";
    this.station = "";
    this.params = [];
## }
## }
class OperatorRequestParamsEntity {
## constructor() {
    this.key = "";
    this.value = "";
## }
## }
class OperatorRepsonseEntity {
## constructor() {
    this.code = 200;
    this.msg = "OK";
## }
## }
### class ScriptRepsonseEntity {
## constructor() {
    this.code = 200;
    this.body = "OK";
## }
## }
### class SelectOption {
## constructor() {
    this.value = "";
    this.label = "";
## }
## }

 以上代码中，通过 workTypes 和  workStations 参数可指定创建的需求单对于某些岗位/工位可见。menuId: "chooseMatSite" 参数指定仓库管理员收到需求单后，用于选择仓库发货库位的配置菜单 id。
然后在 设置 界面中增加 menuId 为 chooseMatSite 的菜单。
## operator:
## workTypes:
## - id: COMMON-TYPE
## label: 通用岗位
## workStations:
### - id: COMMON-STATION
## label: 通用工位
## # 需求单配置
## demandTask:
## # 启用需求单
## enable: true
## # 手持端需求单标签页的名称
## tabName: 需求单
## processFuncList:
## # 需求单页面中工单的名称
### - defLabel: callMatFromBuf
### # 需求单该工单下单后，对应的脚本响应方法名
## funcName: dealOrder
        # 需求单扩展属性，配置后使用脚本方法添加需求单时，可设置扩展属性
## extensionFields:
## - id: 1
### attributeName: 扩展字段1
## - id: 2
### attributeName: 扩展字段2
## orders:
### - menuId: callMatFromBuf
## label: 仓库出货
### robotTaskDef: callMatFromBuf
### route: addDemandData
### menuItemBackground: #32CD32
### workTypes: [ COMMON-TYPE ]
      workStations: [ COMMON-STATION ]
## tip: 仓库出货
### confirmMessage: 确定从仓库出货吗？
## params:
## - name: targetSite
## input: select
## label: 叫料库位
## options:
## - value: PS-1
## label: 线边库位1
## - value: PS-2
## label: 线边库位2

### - menuId: chooseMatSite
## label: 选择仓库库位
### route: chooseMatSite
### menuItemBackground: #32CD32
## disabled: true
### workTypes: [ COMMON-TYPE ]
      workStations: [ COMMON-STATION ]
## params:
## - name: bufSite
## input: select
## label: 仓库库位
## options:
## - value: buf-1
## label: 仓库库位1
## - value: buf-2
## label: 仓库库位2

 menuId 为 chooseMatSite 的菜单的 disabled 设置为 true，即菜单是禁用点击的，这是因为这个菜单是集成在需求单详情中，不能单独给用户使用。这个禁用的菜单颜色呈灰色，没有点击效果。 
1653453171486-be2b8667-1fc6-4a57-94a5-9714e08c5cd7.png

这时，可尝试点击手持端的“仓库出货”菜单，选择一个叫料点库位，下单发起叫料。 
1653453198909-9f16bc57-e094-4339-8aa7-49b1f81ab374.png

点开需求单标签页，可看到出现一条需求单数据。 
1653453216558-d2a787f3-0733-48d4-bfb8-acfaa42f6159.png 

 点击这条数据，查看详情，此时，也意味着这个需求单被当前仓库管理员锁定，其他手持端用户将看不到这条需求单数据。

1653453228909-33a4c3d4-e322-4a8a-a27e-34ddadaaf116.png 

上图红框的部分即是集成在需求单中的  menuId 为 chooseMatSite 的菜单，用于选择发货库位。这时，选择一个仓库库位，点击“确定”按钮，理论上可以正式发起从仓库到产线的物料搬运任务。
但是，点击“确定”按钮后，发起物料搬运任务的业务逻辑还未完成，需要在脚本上补充。
## function boot() {
### // "仓库出货"菜单的下单按钮的响应方法进行注册
  jj.registerHandler("POST", "/script-api/addDemandData", "addDemandData", false);
## }
function addDemandData(param) {
  var paramObject = JSON.parse(param);
  let response = new ScriptRepsonseEntity();
  let operatorResponse = new OperatorRepsonseEntity();
## // 创建需求单
  let robotDefLabel = paramObject["robotTaskDef"];
  let menuId = paramObject["menuId"];
## let addParam = {
## // 需求单数据名称
    defLabel: robotDefLabel,
    description: "仓库出货",
### // 需求单补充内容对应的菜单id，需在yml配置中存在
    menuId: "chooseMatSite",
## // 需求单数据可见的岗位
    workTypes: "COMMON-TYPE",
## // 需求单数据可见的工位
    workStations: "COMMON-STATION",
    createdBy: menuId,
## // 需求单内容对象
    content: paramObject,
### // 需求单扩展字段，需要在配置文件中配置
        attrList: [{"attributeName": "扩展字段1","attributeValue": "123"},{"attributeName": "扩展字段2","attributeValue": "123"}]
  };
## // 创建需求单数据
  jj.addDemand(JSON.stringify(addParam));

  response.body = JSON.stringify(operatorResponse);
  return response;
## }
// 需求单详情页点击下单的响应方法，此方法用于根据叫料点的起点和终点生成搬运任务
### function dealOrder(param) {
  let demandInfo = JSON.parse(param);
## // 需求单的内容
  let contentObject = demandInfo["content"];
  let contentParam = contentObject["params"];
## // 选择的叫料点库位id
  let toSiteId = contentParam[0]["value"];
## // 需求单的补充内容
  let supplementContentObject = demandInfo["suplementContent"];
  let suppleParam = supplementContentObject["params"];
  let fromSiteId = suppleParam[0]["value"];
  // 此处判断如果仓库库位操作者选择了 buf-1 库位，则返回手持端该库位不可用的信息
### if(fromSiteId == "buf-1") {
      let ex = new ScriptException()
## ex.code = 400
### ex.message = "该库位不可用!"
## throw ex
## }
## let inputParams = {
    fromSiteId: fromSiteId,
## toSiteId: toSiteId
  };
## let taskParam = {
## // 搬运物料的天风任务名
    taskLabel: "moveMat",
    inputParams: JSON.stringify(inputParams)
  };
## // 运行搬运物料的天风任务
  jj.createWindTask(JSON.stringify(taskParam));
## }
### class OperatorRequestEntity {
## constructor() {
    this.robotTaskDef = "";
    this.station = "";
    this.params = [];
## }
## }
class OperatorRequestParamsEntity {
## constructor() {
    this.key = "";
    this.value = "";
## }
## }
class OperatorRepsonseEntity {
## constructor() {
    this.code = 200;
    this.msg = "OK";
## }
## }
### class ScriptRepsonseEntity {
## constructor() {
    this.code = 200;
    this.body = "OK";
## }
## }
### class SelectOption {
## constructor() {
    this.value = "";
    this.label = "";
## }
## }
### class ScriptException {
## constructor() {
        this.code = 0;
        this.message = "OK";
## }
## }

 上方代码中，dealOrder 方法是发起物料搬运任务的响应方法，该方法中调用名为 moveMat 的天风任务，将发货点库位 id 和叫料点库位 id 作为起点（fromSiteId）和终点（toSiteId）参数传入。
创建名为 moveMat 的天风任务，以发货点库位 id 和叫料点库位 id 作为入参，将物料从发货点运到线边叫料点。
1657160372927-b9dd153d-ae20-4ef2-94fa-83126bfacf41.png 

1657160382279-7fc517ee-c254-4096-a637-75ca01983061.png

至此，用需求单功能实现线边叫料、仓库出货的业务逻辑开发完毕。此时在需求单详情中点击“确定”按钮，将正式发起物料搬运任务。
1653453334051-181da2a8-90c1-48c0-8e17-d78a5512c549.png 

1657160400161-0da7ccae-66da-498e-a6ff-7786d9479749.png 

1657160407131-b10fdeaf-9aa4-4831-9e1c-d35c9f08a405.png

如果仓库出库时点击“确定”按钮发起发料时，业务上需先判断库位是否可用，不可用就返回手持端错误信息，这个需求可以通过修改脚本实现。
## function boot() {
### // "仓库出货"菜单的下单按钮的响应方法进行注册
    jj.registerHandler("POST", "/script-api/addDemandData", "addDemandData", false);
## }
function addDemandData(param) {
    var paramObject = JSON.parse(param);
    let response = new ScriptRepsonseEntity();
    let operatorResponse = new OperatorRepsonseEntity();
## // 创建需求单
    let robotDefLabel = paramObject["robotTaskDef"];
    let menuId = paramObject["menuId"];
## let addParam = {
            // 需求单数据名称,可自定义，需和yml需求单配置参数中的defLabel字段值保持统一
        defLabel: robotDefLabel,
        description: "仓库出货",
### // 需求单补充内容对应的菜单id，需在yml配置中存在
        menuId: "chooseMatSite",
### // 需求单数据可见的岗位，多个岗位用英文逗号相隔
        workTypes: "COMMON-TYPE",
### // 需求单数据可见的工位，多个工位用英文逗号相隔
        workStations: "COMMON-STATION",
        createdBy: menuId,
## // 需求单内容对象
        content: paramObject,
### // 需求单扩展字段，需要在配置文件中配置
        attrList: [{"attributeName": "扩展字段1","attributeValue": "123"},{"attributeName": "扩展字段2","attributeValue": "123"}]
    };
## // 创建需求单数据
    jj.addDemand(JSON.stringify(addParam));

    response.body = JSON.stringify(operatorResponse);
    return response;
## }
// 需求单详情页点击下单的响应方法，此方法用于根据叫料点的起点和终点生成搬运任务
### function dealOrder(param) {
    let demandInfo = JSON.parse(param);
## // 需求单的内容
    let contentObject = demandInfo["content"];
    let contentParam = contentObject["params"];
## // 选择的叫料点库位id
    let toSiteId = contentParam[0]["value"];

## // 需求单的补充内容
    let supplementContentObject = demandInfo["suplementContent"];
    let suppleParam = supplementContentObject["params"];
    let fromSiteId = suppleParam[0]["value"];

    // 此处判断如果仓库库位操作者选择了 buf-1 库位，则返回手持端该库位不可用的信息
### if(fromSiteId == "buf-1") {
### let ex = new ScritpExcetion()
## ex.code = 400
### ex.message = "该库位不可用!"
## throw ex
## }

## let inputParams = {
        fromSiteId: fromSiteId,
## toSiteId: toSiteId
    };

## let taskParam = {
## // 搬运物料的天风任务名
        taskLabel: "moveMat",
        inputParams: JSON.stringify(inputParams)
    };
## // 运行搬运物料的天风任务
    jj.createWindTask(JSON.stringify(taskParam));
## }
### class OperatorRequestEntity {
## constructor() {
        this.robotTaskDef = "";
        this.station = "";
        this.params = [];
## }
## }
class OperatorRequestParamsEntity {
## constructor() {
        this.key = "";
        this.value = "";
## }
## }
class OperatorRepsonseEntity {
## constructor() {
        this.code = 200;
        this.msg = "OK";
## }
## }
### class ScriptRepsonseEntity {
## constructor() {
        this.code = 200;
        this.body = "OK";
## }
## }
### class SelectOption {
## constructor() {
        this.value = "";
        this.label = "";
## }
## }
### class ScritpExcetion {
## constructor() {
        this.code = 0;
        this.message = "OK";
## }
## }
如下图，手持端会提示“该库位不可用！”的信息，因为有错误信息，该需求单将保持待处理的状态。
1663906214027-a6424a4f-ced8-4d0f-a317-53460ba12b7b.png

### 4.11 创建数据单

## 1.案例需求描述

### 自定义查询数据表，并且在手持端以表格形式展示

### 2. 设置 界面开启数据单功能

## operator:
## # 数据单
## tableShow:
    # 是否启用数据单，true:启用，false:禁用，默认为禁用
## enable: true
## # 数据单自定义名称
## tabName: 数据单
## showSql:
## # 数据单唯一id
## - id:  ts
              # 执行的sql语句:{site_id}表示可筛选，{site_id}表示这个字段占位需要根据手持端输入的查询条件替换
        sql: select site_id,locked,locked_by from t_worksite WHERE site_id LIKE {site_id}
## # 查询语句自定名称
## label: 库位展示
        # sql语句注册的查询条件参数声明数组里面的值和占位括弧里面的值保持一致
## params: [site_id]
        # 查询参数的类型，目前只支持string、number两种。和params先后顺序一一对应
### paramsType: [string]
## # 查询参数显示的信息
### paramsTipName: [库位id]
## # 手持端表格显示字段
## tableHead:
          - value: site_id      # 需要显示的查询字段
            label: 库位id        # 手持端页面表头显示名称
          - value: locked       # 需要显示的查询字段
            label: 是否锁定       # 手持端页面表头显示名称
## # 字段值转成需要展示的文字
## status:
            # 当locked这个字段值为0时，页面对应的要显示成未锁定，当值为1时，页面对应的要显示成锁定
## - value: 0
## label: 未锁定
## - value: 1
## label: 锁定
          - value: locked_by   # 需要显示的查询字段
            label: 锁定者       # 手持端页面表头显示名称
### 说明：AND、WHERE、HAVING、LIKE必须大写

1660114273585-f7d0c19e-fc61-4a7c-9e5e-e5696ddf19cc.png

### 4.12 查询按钮使用案例

## 1. 案例需求描述

手持端查询成功，能够返回查询信息，并且只有查询成功后才能点击下单。

### 2. 在 设置 界面中添加以下代码

## rdscore:
  baseUrl: http://127.0.0.1:8088

## operator:
## workTypes:
## - id: COMMON-TYPE
## label: 通用岗位
## workStations:
### - id: COMMON-STATION
## label: 通用工位

## orders:
## - menuId: queryDemo
## label: 查询按钮案例
### menuItemBackground: #32CD32
### menuItemTextColor: black
      canSendTask: false        # 开启查询按钮为false，关闭为true，默认值为true。
      showQueryCallback: true        # 是否弹框显示查询返回字段。
## disabled: false
### workTypes: [ COMMON-TYPE ]
      workStations: [ COMMON-STATION ]
## tip: 查询按钮案例
### confirmMessage: 确定下单吗？
## params:
## - name: cardNo
## input: text
## label: 请扫料盒码

### 3.脚本中处理查询逻辑，脚本中添加以下代码。

## function boot() {
## //注册查询方法
    jj.registerHandler("POST", "/script-api/can-send-task", "handleQueryParams", false)
## }

function handleQueryParams(param){
    //自定义查询方法，案例场景员工扫描料盒码。查询是否有空闲库位，能够放入料盒码。
## try {
        jj.getLogger().info("查询参数：" + param);
        var paramJson = JSON.parse(param);
        var params = paramJson["params"];
        var response = new ScriptRepsonseEntity();
        var operatorResponse = new OperatorRepsonseEntity();

        var cardNo = params[0].value;
        jj.getLogger().info("cardNo" + cardNo);
## //根据库区名查询一个空闲库位
       var siteId = jj.getSiteByGroupNameRetry("liuchengka",false)

### if (null == siteId) {
            operatorResponse.code = 400;
            operatorResponse.msg = "查询失败，没有空闲库位分配";
            response.body = JSON.stringify(operatorResponse);
            return response;
## }

        operatorResponse.code = 200;
        operatorResponse.msg = "料盒放在"+siteId;
        response.body = JSON.stringify(operatorResponse);    
        return response;
## } catch (error) {
        jj.getLogger().info(`query error ${error}`)
## throw error
## }
## }

## // 手持端响应实体类
class OperatorRepsonseEntity {
## constructor() {
        this.code = 200;
        this.msg = "OK";
## }
## }
## // 服务端响应封装类
### class ScriptRepsonseEntity {
## constructor() {
        this.code = 200;
        this.body = "OK";
## }
## }

function successResponse(message = "OK") {
    return { code: 200, body: JSON.stringify({ code: 200, msg: message }) };
## }

## 添加以上代码后，按以下操作

## 打开手持端

1660114338228-55515a56-ccac-48db-8db9-14c459608bd3.png

## 点击查询按钮案例

1660114398245-205d69cf-d333-43b9-9277-956787eb0724.png

### 扫描料盒码点击查询，查询成功。并且下单按钮能够使用

1660114437263-c0f2cdb7-0ffe-41ba-9ceb-86cba110f652.png

### 扫描料盒码点击查询，查询失败。下单按钮任然不能使用

1660114486687-6f24812b-11fd-4fde-9bbe-3251b3ddd94f.png

### 4.13 创建首页卡片

## 1.在 设置 界面中，添加以下配置

## operator:
## cards:
## enable: true
## card:
### - menuId: EasyTransferTask1
        label: 表格                          # 主题
#        workTypes: []                                                # 工位岗位配置，根据需要开启
#        workStations: []                                        # 岗位配置，根据需要开启
        menuItemBackground: #32CD32       # 手持端选项的背景颜色
        menuItemTextColor: #000000        # 手持端字体的颜色
        menuType: table                      # text 文字，table表格
        route: showDate                     # 手持端路由
### - menuId: EasyTransferTask1
        label: 文字                          # 主题
### #        workTypes: []
### #        workStations: []
        menuItemBackground: #32CD32       # 手持端选项的背景颜色
        menuItemTextColor: #000000        # 手持端字体的颜色
        menuType: text                      # text 文字，table表格
        route: showText                     # 手持端路由

### 2.在线脚本中添加以下代码

## function boot() {
## //首页卡片表格数据注册的路由
        jj.registerHandler("POST", "/script-api/showDate", "showDate", false);
## //首页卡片表格文字注册的路由
        jj.registerHandler("POST", "/script-api/showText", "showText", false);
## }
### function showText(params){
    var res = { code: 200, msg: "成功" ,data:"这是显示手持端卡片的文字信息" };
    return { code: 200, body: JSON.stringify(res) };
## }

### function showDate(params){
## let condition = {
### "area": "new", //区域名
## }
    let result = jj.findAvailableSitesByCondition(JSON.stringify(condition),"DESC")
    let resultJson = JSON.parse(result)
## var data = [
### {"head": ["库位id","占用","锁定"]}
## ]

    for(let i=0; i<resultJson.length; i++){
        let locked = resultJson[i]["locked"] == 0 ? "未锁定":"锁定"
        let filled = resultJson[i]["filled"] == 0 ? "未占用":"占用"
        let siteId = resultJson[i]["siteId"]
        let row = {"row" : [siteId,filled,locked]}
## data.push(row)
## }
    var resultMsg = {"code": 200,msg:"查询成功","data": data}
    return {"code":200, "body":JSON.stringify(resultMsg)}
## }

说明：text需要返回的数据格式为： { code: 200, body: JSON.stringify({ code: 200, msg: "成功" ,data:"文本展示文字" }) };table返回的数据格式为：{ code: 200, body: JSON.stringify({ code: 200, msg: "成功" ,data:[
                    {"head":["表头1","表头2","表头3"]},
                    {"row":["行1","行2","行3"]},
                    {"row":["行21","行22","行23"]},
                    {"row":["行31","行32","行33"]},
                    {"row":["行41","行42","行43"]});
    button返回数据格式: { code: 200, msg: "成功" , JSON.stringify(data:[
       {"text":"1","bgColor":"#4B0082","textColor":"#000000"},
       {"text":"2","bgColor":"#4B0082","textColor":"#000000"},
       {"text":"3","bgColor":"#FF00FF","textColor":"#000000"},
       {"text":"4","bgColor":"#483D8B","textColor":"#000000"},
       {"text":"5","bgColor":"#191970","textColor":"#FF00FF"},
       {"text":"6","bgColor":"#5F9EA0","textColor":"#FF00FF"},
   ]});

## 手持端展示如下：

1660114520587-dfa3aa10-51a0-4b98-9da8-bb11222b0645.png

1660114548254-55a45a91-1aec-4d35-873b-6f672ccaf7d7.png

### 4.14 快捷下单
背景说明：可通过按钮直接生成任务，任务执行过程中不可再次下单，并且下单按钮会变成不可下单的颜色，直到任务结束或者任务进行到某一个阶段才可以继续下单，这时候按钮变成可下单的颜色。
## 快捷下单参数说明
## operator:
## easyOrders:
enable: true                          # 开启快捷下单
## easyOrder:
### - menuId: EasyTransferTask1
label: 简单运输任务1                  # 按钮文字
menuItemBackground: '#32CD32'         # 按钮背景色
menuItemTextColor: '#000000'          # 按钮文字的颜色
menuItemRunBackground: '#FF0000'      # 不可下单时的颜色
layout: left                        # 按钮布局left right nright
taskLabel: test                     # 天风任务名
## orderExecute:
route: createTask                 # 下单时执行的路由
params: BDB1                      # 下单时执行的路由时入参
## callBackExecute:
route: backTask                    # 执行回调的路由
params: CK1                       # 回调函数调用路由传参
### 4.14.1 快捷下单布局
layout 布局在同一行最多显示两个按钮， left  表示当前按钮从上而下显示在左边， right 表示当前按钮从上而下布局显示在右边， nright  表示当前按钮换行显示在右边，可不配置按默认展示。
## operator:
## easyOrders:
    enable: true                          # 开启快捷下单
## easyOrder:
### - menuId: EasyTransferTask1
        label: 简单运输任务1                  # 按钮文字
        menuItemBackground: '#32CD32'         # 按钮背景色
        menuItemTextColor: '#000000'          # 按钮文字的颜色
        menuItemRunBackground: '#FF0000'      # 不可下单时的颜色
        layout: left                        # 按钮布局left right nright
        taskLabel: test                     # 天风任务名
## orderExecute:
          route: createTask                 # 下单时执行的路由
          params: BDB1                      # 下单时执行的路由时入参
## callBackExecute:
          route: backTask                    # 执行回调的路由
          params: CK1                       # 回调函数调用路由传参
### - menuId: EasyTransferTask1
       label: 简单运输任务1                  # 按钮文字
       menuItemBackground: '#32CD32'         # 按钮背景色
       menuItemTextColor: '#000000'          # 按钮文字的颜色
        menuItemRunBackground: '#FF0000'      # 不可下单时的颜色
        layout: left                        # 按钮布局left right nright
        taskLabel: test                     # 天风任务名
## orderExecute:
          route: createTask                 # 下单时执行的路由
          params: BDB1                      # 下单时执行的路由时入参
## callBackExecute:
          route: backTask                    # 执行回调的路由
          params: CK1                       # 回调函数调用路由传参
1667878913314-e7ebdb88-2c25-4f6b-b191-c37ca8495117.png

### 4.14.2 快捷任务下单
### taskLabel  下单、回调配置案例
## operator:
## easyOrders:
       enable: true                          # 开启快捷下单
## easyOrder:
### - menuId: EasyTransferTask1
           label: 简单运输任务1                  # 按钮文字
           menuItemBackground: #32CD32         # 按钮背景色
           menuItemTextColor: #000000          # 按钮文字的颜色
           menuItemRunBackground: #FF0000      # 不可下单时的颜色
           layout: left                        # 按钮布局left right nright
           taskLabel: test                     # 天风任务名
发起任务页面，点击此按钮，可发起名为 test 的天风任务（必须要有天风任务名位 test 的天风任务），任务在运行中（任务未到达终态）此按钮会变成不可下单的颜色，直至此任务到达终态，此按钮才可以继续下单
### 2. orderExecute 下单， taskLabel 回调配置案例
## 设置 界面 配置
## operator:
## easyOrders:
       enable: true                            # 开启快捷下单
## easyOrder:
### - menuId: EasyTransferTask1
           label: 简单运输任务1                    # 按钮文字
           menuItemBackground: '#32CD32'         # 按钮背景色
           menuItemTextColor: '#000000'          # 按钮文字的颜色
           menuItemRunBackground: '#FF0000'      # 不可下单时的颜色
           taskLabel: test                       # 天风任务名
           orderExecute:                         # 下单参数
             route: createTest                   # 下单时执行的脚本方法名称
## 脚本代码
## function boot(){
## //下单时执行的脚本方法注册
  jj.registerHandler("POST", "/script-api/createTest", "createTest", false);
## }

### function createTest(params){
  jj.scriptLog("info", "createTest",params)
## let taskParam = {
### taskLabel: "test",//------更改
  };
  let result = JSON.parse(jj.createWindTask(JSON.stringify(taskParam)));
  jj.scriptLog("info", "createTest",JSON.stringify(result))
### if(result["code"] != -1){
    let res = { code:200, msg: "成功" }
    return { code: 200, body: JSON.stringify(res) };
## }
  let res = { code:400, msg: "失败" }
  return { code: 400, body: JSON.stringify(res) };

## }
说明：手持端的 taskLabel 配置和执行脚本下单方法里生成的天风任务的任务名必须一致（必须要有天风任务名位 test 的天风任务）。发起任务页面，点击此按钮，执行脚本方法生成天风任务名为 test 的天风任务，任务在运行中（任务未到达终态）此按钮会变成不可下单的颜色，直至此任务到达终态，此按钮才可以继续下单
### 3.orderExecute 下单， callBackExecute 回调配置案例
### 任务执行到某个时刻快捷任务就需要回复下单
## 设置 界面 配置
## operator:
## easyOrders:
       enable: true                            # 开启快捷下单
## easyOrder:
### - menuId: EasyTransferTask1
           label: 简单运输任务1                    # 按钮文字
           menuItemBackground: '#32CD32'           # 按钮背景色
           menuItemTextColor: '#000000'            # 按钮文字的颜色
           menuItemRunBackground: '#FF0000'        # 不可下单时的颜色
           orderExecute:                         # 下单参数
             route: createTest                   # 下单时执行的脚本方法名称
           callBackExecute:                       # 下单时自定的回调方法
             route: createTestBack               # 回调的路由
## 脚本代码
## function boot(){
## //下单时执行的脚本方法注册
  jj.registerHandler("POST", "/script-api/createTest", "createTest", false);
## //下单时执行的回调方法注册
  jj.registerHandler("POST", "/script-api/createTestBack", "createTestBack", false);
## }
### function createTest(params){
  jj.scriptLog("info", "createTest",params)
  //获取快捷任务的唯一标识，作为全局缓存的唯一变量的状态位，传到任务参数中，在任务某个时刻修改它
  let menuId= JSON.parse(params)["menuId"]
## let inputParams = {
## "menuId": menuId
  };
## let taskParam = {
    "taskLabel": "test",
    "inputParams": JSON.stringify(inputParams),
  };

  let result = JSON.parse(jj.createWindTask(JSON.stringify(taskParam)));
  jj.scriptLog("info", "createTest",JSON.stringify(result))
### if(result["code"] != -1){
    let res = { code:200, msg: "成功" }
    jj.putCacheParam(menuId, false+"")
    return { code: 200, body: JSON.stringify(res) };
## }
  let res = { code:400, msg: "失败" }
  return { code: 400, body: JSON.stringify(res) };

## }
function createTestBack(params){
  jj.scriptLog("info", "createTestBack",params)
  let menuId= JSON.parse(params)["menuId"]
### //回调执行方法取出唯一变量状态位判断是否可恢复下单
  let flag = jj.getCacheParam(menuId)
  if(flag == "true" || flag == null){
    //满足条件可恢复下单      canSendTask 为true时标识可继续下单，false表示不可继续下单
    return { "code": 200, "body": JSON.stringify({ "code": 200, "msg": "成功", "canSendTask": true }) }
## }
  return { "code": 200, "body": JSON.stringify({ "code": 200, "msg": "成功", "canSendTask": false }) }
## }
## 天风任务
1667879197404-ae0afb6f-fadf-48c7-9447-e6ac98d99214.png

1667879214863-722113c0-cd45-40c3-87a5-7f842d4e4cee.png

说明：发起任务页面，点击此按钮，执行脚本方法生成天风在执行下单的路由方法时存入全局变量作为状态位，在运行天风任务中修改这个变量，之后再回调函数中判断这个变量是否满足继续下单的要求。（ 提示：任务终止、异常结束需要再系统时间中复位这个变量 ）
### 4.taskLabel 下单，callBackExecute 回调配置案例
## 设置 界面 配置
## operator:
## easyOrders:
       enable: true                            # 开启快捷下单
## easyOrder:
### - menuId: EasyTransferTask1
           label: 简单运输任务1                    # 按钮文字
           menuItemBackground: '#32CD32'           # 按钮背景色
           menuItemTextColor: '#000000'            # 按钮文字的颜色
           menuItemRunBackground: '#FF0000'        # 不可下单时的颜色
           taskLabel: test                       # 天风任务名
           callBackExecute:                      # 下单时自定的回调方法
             route: createTestBack               # 回调的路由
## 脚本代码
## function boot(){
## //下单时执行的回调方法注册
       jj.registerHandler("POST", "/script-api/createTestBack", "createTestBack", false);
## }
   function createTestBack(params){
### //根据业务需求返回需要对应canSendTask的值
         //canSendTask 为true时标识可继续下单，false表示不可继续下单
        return { "code": 200, "body": JSON.stringify({ "code": 200, "msg": "成功", "canSendTask": true }) }
        //return { "code": 200, "body": JSON.stringify({ "code": 200, "msg": "成功", "canSendTask": false }) }
## }
提示：方式 3 如果回调函数返回值 canSendTask 都是 true，此用法可以当个发起任务按钮使用

## 五， 定时任务使用案例
### 5.1 案例说明
天风任务分为普通任务和定时任务两类，其中的定时任务可实现周期性执行某个任务流程的功能。此案例通过配置天风任务的定时任务，定时读取 modbus 信号值，并打印结果。
### 5.2 案例教程

## 1、在天风任务界面创建定时任务。
1673262555676-691f129c-3333-4922-9bee-98e3392d7022.png

### 2、编辑定时任务，增加读取 Modbus 值的块，后续再接打印块。
1673262581164-e9c5a993-0c1b-4a49-86a0-a5ddd5f4130f.png

### 3、 设置定时任务的运行周期（点击【任务基本设置】按钮，默认运行周期：3 s/次）

1673262606132-64a0ef23-d5fb-43b3-a705-72f30bf003fd.png

### 4、 点击【启动】按钮，启动定时任务。
1673262628615-98c8d7c8-6d0b-4c09-8daf-658fd5739ee9.png

### 5、 在任务监控页面查询定时任务实例。在定时任务筛选下拉菜单中，选择“是”，表示筛选天风任务中的定时任务类型。

1673262643625-14829fb1-1692-4e88-b8f1-3090d8cdf894.png

## 六， 接口配置使用案例

### 6.1 案例说明

 RDS 中的【接口配置】功能可以通过拖拽组件的方式自定义一个对外的公共接口，包括自定义接口参数和返回值。
本案例通过【接口配置】进行自定义接口，该接口实现向指定库位中设置指定的货物内容的功能，接口参数中将传入目标库位和对应的库位内容，RDS 接收到后，通过调用设置库位内容的天风任务，实现该功能。

### 6.2 案例教程
1692266687634-1877ff49-57a4-45df-a836-faca472f7b52.png

RDS 的【接口配置】支持两种类型的接口，手持端接口和对外公共接口，支持 GET 和 POST 两种请求方式，接口数据传输格式使用 JSON。此案例将增加一个 POST 请求方式的公共接口。
## 假设需要自定义的接口如下：
POST http://127.0.0.1:8080/handle/register/interfaceTask
## 接口请求参数：
## {
## "data":{
    "taskRecordId": "306311f2-06c3-4af3-b7dd-d99a6da1a46b",     
    "targetSiteId": "Loc-05",
### "content": "Empty Tray"
## }
## }
其中的 taskRecordId 参数可用于设置 RDS 系统的生成的任务实例 Id 使用上位机指定的 Id。
## 接口返回值：
## {
## "result": "success"
## }
### 6.2.1 接口任务

 【接口配置】添加一个接口（接口设置的 URL 必须以"/handle/register/"开头），比如此案例的接口 URL 为 /handle/register/interfaceTask，不需要填写 IP 或者域名。
1673262913540-2d37a705-1515-4a02-bbb8-378690cf6aa8.png

## 接口编辑
 接口输入参数，参数类型选择 JSON 对象，表示输入参数为 JSON 类型。
1673263036093-abf5fe7b-a184-4871-8cac-7dd5ffdc0b91.png

 以下是该接口的具体配置，配置中首先检查接口传入的任务 Id 是否重复，如果重复，即向发送接口的上位机反馈任务实例 id 重复的错误信息。接下来正式调用设置库位内容的天风任务，库位设置的天风任务完成后，返回调用接口成功的信息。

1673262973792-53e26c05-e82f-4896-a9f0-8f0f38b8ebd8.png

接下来，配置上一步中用于设置库位内容的天风任务。首先新建一个名为"接口任务"的天风任务。
1692267055765-c1e3621a-668a-421f-ae0c-eed6d06f8358.png

设置天风任务的输入参数。其中的 taskRecordId 不需要人为在输入参数中定义，RDS 系统默认会提供该参数。
1692267093683-37ca22af-8694-417a-b8b5-399a3466e0d4.png

配置天风任务，使用【设置库位货物】块，根据输入参数设置库位货物内容。
1692267135183-4858711f-e6f1-4ccc-a63e-9cdf94f2b529.png

至此，全部的接口配置完成。

## 3 .测试接口
1673600514674-bad3ee56-171b-4025-b43b-3606c0266064.png

可以在任务管理页面看到运行的天风任务，实例 ID 为接口参数中赋予的任务实例 id。
1673403436714-7766f10a-35c6-432a-9a66-7ad2f070bef8.png

### 6.2.2 PDA 接口
该案例将添加一个手持端使用的任务下发接口，实现的功能和 6.2.1 章节类似，不过任务下发终端是手持端，通过定义手持端菜单以及下单接口，在手持端菜单上选择指定库位和改库位的货物内容，达到将 RDS 系统中该库位的货物内容设置为手持端需要的值。手持端菜单样式如下，可通过【设置】界面配置参数得到该菜单。
1692267273022-e6fa508e-5800-4a84-b79b-544bab72332a.png

## 1. 【接口配置】添加一个手持端接口， 注：接口设置的 url 必须以"/handle/register/"开头
1692267338956-c650993c-0c77-4fd8-b169-4238c788589a.png

 【设置】界面配置以下参数，配出手持端名为 interfaceTaskPDA 的菜单。
## rdscore:
  baseUrl: http://127.0.0.1:8088
## operator:
## workTypes:
## - id: COMMON-TYPE
## label: 通用岗位
## workStations:
### - id: COMMON-STATION
## label: 通用工位
## orders:
### - menuId: PDAInterfaceTask
### label: interfaceTaskPDA
### menuItemBackground: null
### menuItemTextColor: black
### route: interfaceTaskPDA
      ifEnableInterfaceManager: true
## disabled: false
## tip: SetSiteContent
### confirmMessage: 确认下单吗？
## params:
### - name: targetSiteId
## input: select
## label: 请选择库位id
## options:
## - label: Loc-05
## value: Loc-05
## - label: Loc-03
## value: Loc-03
## - name: content
## input: select
## label: 请选择货物内容
## options:
## - label: Empty Tray
## value: Empty Tray
## - label: Full Tray
## value: Full Tray
以上 yaml 格式的配置参数中，ifEnableInterfaceManager 参数 true，表示 menuId 为 PDAInterfaceTask 的手持端菜单的下单接口是通过【接口配置】功能配置所得，其中的 route 参数表示接口路由，必须和【接口配置】中定义的接口的 URL 参数匹配，比如以上示例中 route: interfaceTaskPDA，【接口配置】中的 URL 则为 ”/handle/register/interfaceTaskPDA“。
### route: interfaceTaskPDA
ifEnableInterfaceManager: true  
## 手持端接口编辑
接口添加输入参数，其中 targetSiteId 和 content 是对应图中的选择库位下拉菜单和库位内容下拉菜单两组件的 name 属性值。
1692267577993-a97fc823-e051-42e0-8bd1-fc995fe03bb8.png

1692267602135-66133756-7ef0-47a8-957b-c3a2f57e151d.png

配置具体接口逻辑，调用设置库位的天风任务，之后返回手持端成功完成的响应。其中名为”接口任务”的天风任务的配置方式和 6.2.1 章节相同。
1673263147624-90c700a7-0961-4f46-bb01-f4ff097d56f9.png

以上配置完成后，通过手持端测试接口，点击下单按钮，可以发现【库位管理】中的 Loc-03 库位的货物内容变为 Empty Tray 。

1673403550024-71f729b0-1491-4599-a01f-d33d32e6f504.png

1673403559795-fd82ac40-4ac6-43ad-8255-df18260e85f8.png

## 七，脚本 debug 开启案例

### 7.1 脚本 debug 开启案例

## 1. 在【设置】界面，添加以下配置，开启脚本 debug 功能，ifDebug 为 true 代表开启调试。

## debug:
## ifDebug: true
## port: 9090

### 2.进入在线脚本页面，点击调试。

1660114751344-50676f6e-34a4-44e1-b8f2-59338f96f152.png

### 会出现如下的弹窗，浏览器输入框内已填充网址

1660114773925-82ba819e-ab16-43ae-a7a5-b4f4edc36945.png

### 去掉网址前面的 chrome- 并按回车

1660114793422-f5cb10dd-10de-4a6d-a6bd-6e38989ca638.png

即可跳转到 debug 页面，对于第一次使用浏览器打开 debug 页面的，需要执行以下操作

1660114816137-40a66672-19d3-40ff-9bab-0a92b9cc2eae.png

## 点击打开文件之后，选择 rds0.js

1660114837867-e160ff6b-58d8-46ef-b849-650290f0d245.png

## 即可打开脚本 debug 页面

1660114857487-1f3ddc13-b382-4eb0-ac0b-7875368b7242.png

## 八，免脚本项目开发案例

### 8.1 仿真项目开发

整场项目的售前方案阶段，需要进行系统仿真，对路线规划和运输节拍进行模拟测算。仿真的步骤包括：

在 Roboshop 软件上，建立仿真场景。
在 RDS 天风任务里，定义运输路线。
运行任务，启动仿真，录屏测算。在仿真不达预期时，重复前面两个步骤，不断修正场景和业务设计。

下文主要描述步骤 2 的操作方法 。

按下图方法，建立一条运输任务 “SingleTask1”。 
1657160463320-3dc8ab5e-5bf5-4347-a89b-6448d02a32e8.png 

## 其中：
【随机生成工作站】块，可按指定前缀和编号规则，随机生成一个站点/库位/工作站的名称。如需要在 PS-04 ～ PS-06 之间，随机选一个库位，则【前缀】填”PS-“；【后缀起始值】填 ”4“；【区间】也就是数量范围，由于共有 04、05、06 三个库位，此处填 ”3“；【后缀长度】指后缀中包含几个数字，如需 ”PS-004“ 这样的名称，则填 ”3“。文中命名规则只含两个数字，如 ”PS-04“，此处填 ”2“。
【选择执行机器人】块，【关键路径】填任务起点库位名称，Core 将尽量选取离起点近的机器人来执行任务。
【机器人通用动作】块，若【目标站点名】中填入的是站点，脚本名填 “Wait” 即可。若【目标站点名】中填入的是库位，脚本名填 “binTask”。
可使用【延迟】块来模拟机器人进行操作的耗时，单位是毫秒。
作为仿真任务，此任务不需要定义输入、输出参数。
按下图方法，创建批量任务 “MultiTask1”。 
1657160477037-e270af12-fbce-4775-8347-1a7701812842.png 

## 其中：
【并发执行 N 次】块，可将子块中的任务，同时创建 N 个。
运行一次该任务，可以同时创建 3 条 “SingleTask1” 任务。
如果需要同时仿真多条运输线上的任务，可按如下方法编辑 “MultiTask1”： 
1657160489481-b9eccf71-1abd-479c-9632-f261667722b1.png 

## 其中：
【并行执行】块，可并发执行其中的各个子块。
运行一次该任务，可以同时创建 3 条 “SingleTask1” 任务，以及 3 条 “sub” 任务。

### 8.2 拼合单

### 方式1.在一个运输任务使用一个拼合单块，如下：

1657160505870-c993dc9c-6783-4669-bbb7-9dfb4b8887ae.png

### 2.在一个运输任务使用多个拼合单块，结合并发块使用，如果使用需要拼合单的输出参数需要结合串行块，如下：

1657160517106-d3bf0a89-073c-42d4-9105-d284a1f646c1.png

1657160526520-763385f0-cc22-4f88-a6e0-e79c7d79f92b.png

### 方式2.多背篓车，使用普通单 代替拼合单，如下：

多个背签的机器人可以使用普通拼单方式,可达到按照自定义动作顺序(和机台、上位对接):
必填项:取货辅助动作load,放货辅助动作unload,绑定相同的料箱号(单个任务可以使用任务实例id),设置放货
### 机器人为前一个取货机器人名(进行同一个机器人取放货设置)
## image.png

多个任务指定起点、终点、可在调度安排下 就近取放货的动作顺序，提高整个流程的效率

## 附件

## 📎RDS 脚本开发实战教程。pdf

## 附录

## 业务参数

## 设置 界面配置文件参数：

### # 配置rdscore的访问地址(默认无需修改)
## rdscore:
  baseUrl: http://127.0.0.1:8088

## # 是否开启重启恢复任务功能
### restartRecoverConfig:
## # true为开启，false为不开启
### restartRecover: false
  # 默认 -1 重启恢复所有任务，当大于 -1 时，只恢复指定天数以内的任务
## keepDays: -1

## # 是否记录modbus读写日志
## modbusLogConfig:
## saveLog: false

## # 超时参数设置
## requestTimeOut:
## # modbus连接超时
## modbusTimeOut: 4000
## # http连接超时
### httpConnectTimeout: 4000
## # http读超时
### httpReadTimeout: 4000
## # http写超时
### httpWriteTimeout: 4000

## # 是否开启脚本debug调试功能
## debug:
## # true为开启，false为不开启
## ifDebug: false
## # 设置访问的端口
## port: 9090

# 是否开启机器人故障时，向目标邮箱发送邮件报警的功能。(需要配合下面的开启邮件配置一起使用)
## errorReportEmail:
## # true为开启，false为不开启
## ifEnabled: false
## # 目标邮箱的地址
  toAddresses: lijk@seer-group.com
## # 发件人
## Sender: ljk

## # 开启邮件配置
## emailConfig:
## # 邮件服务地址
## host: smtp.qq.com
## # 端口号
## port: 465
## # 发件人邮箱
### username: 1234@163.com
## # 发件人邮箱授权码
### password: dlalrfbaelxgbagd
  # 支持哪一种协议，可填的有"SMTP IMAP POP3"
## protocol: smtp
## # 是否开启加密
## sslEnable: true

## # 手持端配置
## operator:
### # 是否允许动态修改手持端首页菜单属性，默认为不允许
### runtimeMenuPropsUpdate: false
## # 岗位数组
## workTypes:
## # 岗位id
## - id: COMMON-TYPE
## # 岗位名
## label: 通用岗位
## # 工位数组
## workStations:
## # 工位id
### - id: COMMON-STATION
## # 工位名
## label: 通用工位
## # 需求单配置
## demandTask:
    # 是否启用需求单功能，true:启用，false:禁用，默认为禁用
## enable: false
## # 手持端需求单标签页的名称
## tabName: 需求单
    # 是否显示需求单中的删除按钮，true:显示，false:隐藏，默认为隐藏
### showDeleteButton: false
## # 需求单响应方法配置
## processFuncList:
## # 需求单数据的名称
### - defLabel: chooseSite
### # 需求单详情页点击"下单"后，对应的脚本响应方法名
### funcName: findSiteByGroupName
## # 数据单
## tableShow:
    # 是否启用数据单，true:启用，false:禁用，默认为禁用
## enable: false
## # 数据单自定义名称
## tabName: 数据单
## showSql:
## # 数据单唯一id
## - id:  ts
        # 执行的sql语句 AND LIKE HAVING WHERE 必须是大写 
        sql: select site_id,locked,locked_by from t_worksite WHERE site_id = {site_id}
## # 查询语句自定名称
## label: 库位展示
## # sql语句注册的查询条件参数
## params: [site_id]
        # 查询参数的类型，目前只支持string、number两种。和params先后顺序一一对应
### paramsType: [string]
## # 查询参数显示的信息
### paramsTipName: [库位id]
## # 手持端表格显示字段
## tableHead:
          - value: site_id      # 需要显示的查询字段
            label: 库位id        # 手持端页面表头显示名称
          - value: locked       # 需要显示的查询字段
            label: 是否锁定       # 手持端页面表头显示名称
## # 字段值转成需要展示的文字
## status:
            # 当locked这个字段值为0时，页面对应的要显示成未锁定，当值为1时，页面对应的要显示成锁定
## - value: 0
## label: 未锁定
## - value: 1
## label: 锁定
          - value: locked_by   # 需要显示的查询字段
            label: 锁定者       # 手持端页面表头显示名称
## # 任务菜单数组
## orders:
## # 菜单id
### - menuId: moveTaskChain
## # 菜单名
## label: 运输任务链
## # 下单的响应方法(POST)路由
### route: moveTaskChainFunc
## # 菜单背景颜色
### menuItemBackground: '#ffff33'
## # 菜单文字颜色
### menuItemTextColor: '#aff2dd'
### # 菜单关联的任务定义，用于脚本进行方法（GET）路由
### robotTaskDef: moveTaskChain
      # 是否禁用菜单，true为禁用，默认为启用。禁用菜单后，菜单将变成灰色，不能点击下单。
## disabled: false
## # 可查看该菜单的岗位数组
### workTypes: [ COMMON-TYPE ]
## # 可查看该菜单的工位数组
      workStations: [ COMMON-STATION ]
      # 下单时是否隐藏查询按钮，true：隐藏，false：显示，默认为隐藏。
## canSendTask: true
## # 下单界面首部的提示
## tip: 运输任务链
## # 点击下单按钮后，弹出的提示性文本
### confirmMessage: 确定发起运输任务链吗？
## # 菜单组件列表
## params:
## # 菜单组件名
## - name: fromSite
## # 菜单组件类型如下:
          # text：单行文本，textarea：多行文本，select：单选下拉菜单，list：多选下拉菜单
## input: text
## # 菜单组件提示文字
## label: 物料缓存库位
## - name: matSite
## input: select
## label: 放置物料的库位
          # 静态下拉菜单，该下拉菜单的所有选项需事先在配置文件中配置完成
## options:
## # 下拉菜单选项值
## - value: buf-1
## # 下拉菜单选项文字
## label: 物料缓存位1
## - value: buf-2
## label: 物料缓存位2
## - name: traySite
## input: select
## label: 选择空托盘的库位
          # 动态下拉菜单。它的数据来源自脚本的getForkSiteList方法，即在脚本中要定义名为
### # getForkSiteList的数据接口
          dynamicOptionsSource: getForkSiteList
          # 下拉菜单是否进行懒加载。true：进行懒加载，false：放弃懒加载。
          # 下拉菜单懒加载意味着用户只有点击下拉菜单时，下拉菜单才会渲染其中的选项数据，
          # 它的数据不会提前加载。
### lazyGetSource: false
## - name: toSite
## input: select
## label: 放置空托盘的库位
## options:
## # 下拉菜单选项值
## - value: tray-1
## # 下拉菜单选项文字
## label: 托盘缓存位1
## # 静态多级联动菜单配置
## checkParent:
                  # matSite为该下拉菜单选项绑定的同级菜单组件名，一旦用户在名
                  # 为matSite的下拉菜单中变更了选项，该下拉菜单的选项将跟着变化
## - parent: matSite
                  # 用户在名为matSite的下拉菜单中选择了值为buf-1的选项，
                  # value: tray-1的下拉菜单选项就会动态显示出来供用户选择
## value: [ buf-1 ]
## - value: tray-2
## label: 托盘缓存位2
## checkParent:
## - parent: matSite
## value: [ buf-2 ]
### - name: actionRecord
## # 多行文本
## input: textarea
## # 多行文本框的行数
## multiple: 3
## # 菜单组件提示文字
## label: 操作记录
## - name: componentId
## input: text
## label: 组件id
          # hidden 属性代表名为 componentId 的菜单组件在界面上隐藏
## hidden: true
          # 组件隐藏时，默认填充的值（适用于 text 和 textarea 类型的组件）
## defaultValue: "id1"
