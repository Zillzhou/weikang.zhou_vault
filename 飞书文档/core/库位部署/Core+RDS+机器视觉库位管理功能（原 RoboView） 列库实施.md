# Core+RDS+机器视觉库位管理功能（原 RoboView） 列库实施

Core+RDS+机器视觉库位管理功能（原 RoboView） 列库实施
## 基本信息

## core版本

### 0.1.9.231026+

## 时间

### 2023.10.27

## 测试范围

## 列库核心任务流

## 软件

### roboshop， rds， core， roboview

## 相关软件均可从制品库中下载
## 背景与目标
## 1.背景
RoboView进行库位检测需要core，rds进行联调，结合叉车库位运货的业务逻辑进行综合测试，更全面的模拟现场生产场景，找出不足之处
整体任务测试覆盖率80%，主要检查在核心流程是否可能存在问题。通过可用性测试找到找到此业务的核心痛点，并解决该痛点，从而提升产品体验。
## 前期分析与设定
## 1. 场景任务设计
库位识别需结合列库场景，需要场景中包含列库，并和相关人员反复沟通确认，持续修改直到能满足目标设定。
## (1)创建库位
库位来自AP点，所有AP点位串联一起，为一列，配置库位后可成列库。
①选中所有点位→②点击库位设置→③创建库位，勾选仅AP站点→④编辑命名规则， 命名
1698806188375-89ad127c-6db7-4be5-a9ad-07df0920fe10.png

## (2)编辑库位动作块
### ①点击属性→②追加模式→③添加动作块→④编辑键→⑤编辑值
## 💡 Tips：
## i. 调度根据传入键来选择库位动作块
i.下方示意图只展示了取货库位动作， 还需配置放货库位动作，⑤中"recfile" 需要根据实际的识别的栈板型号选择。
ii.库位动作块配置详见 任务指令 。
1698807091836-3a7ab1b9-30ca-420b-9bcd-d83d2d929dfc.png

## (3)将库位绑定到库区
一个库位只能属于一个库区，每个库区需绑定一个前置点。
1698805130138-9f67866c-9ba2-4af8-a246-032af3a52124.png

1698805814746-74fb37d2-2a9b-4c5a-a7d8-acf7e181829c.png

### 2. robwiew库位部署
RoboView需要实时监控库位状态， 因此库位需要被相机识别， 不能被除机器人外的其它物品遮挡， 详细配置参考链接 库位部署 。
## (1)添加相机
1699263456390-21c8d90a-a0b3-4cab-860c-4342482c30f0.png

## (2)绘制库位
1699263472524-94dc536b-81ac-402b-93e2-d0ef45045632.png

## 过程和方法
## 1. core下发从库区到库区的运单
post  http://{host}:8088/setOrder
## # json数据
## {
    "id":"test_00113",
## "fromBinAreas": [
## "binArea-02"
    ],
## "toBinAreas": [
## "binArea-01"
## ]
## }
本文档示例的两个库区为栈式列库， 运单告诉叉车从哪个库区取货， 从哪个库区放货， 在选择取货库位时， 机器人去往库区前置点， 根据roboview检测的库位有无货物情况， 选择离前置点最近的有货的库位取货， 放货时， 选择离库区前置点最远的无货库位放货。详见 库区到库区的运单
## 通过相机监测框实时查看库位状态
1699236562799-64d6ea68-e89c-4ac3-92b3-5f3c4fa162fb.png

## 11月9日.mp4
### 2. rds下发从库区到库区的运单
### 天风任务及RDS的使用参考链接： RDS 多机器人调度系统
## (1)编辑天风任务
启动rds并登录后, 选择天风任务, 定义新的任务, 输入任务名称
1699259550020-fe6fe607-0f5c-4f2e-9eb0-6ac1bf37b6e7.png

## 创建好后选择编辑
1699259706514-6f9913b9-4e6f-4b6e-bf6e-ddb9129dbf8c.png

## 天风任务逻辑
1699262647506-fe6b506e-d1da-481d-93aa-ddd19ea98b13.jpeg

## 取货任务
1699262876070-842a92c1-34e7-499c-a8f4-4443fd91a44b.png

## 放货任务
1699262995186-b0d355fe-434b-4728-bfa9-ecf19de8ce8f.png

## 放货任务boot.js
## function boot()
## {
    jj.registerHandler("POST", "/script-api/httpDemo", "httpDemo", false);
## }
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
### function findSiteDesc() {
## var condition = {
        "groupNames": ["binArea-01"],
## "filled": false
## }
    var targetSiteIds = "";
    var siteList = jj.findSitesByCondition(JSON.stringify(condition),"ASC");
### if(siteList != "null") {

        targetSiteIds = JSON.parse(siteList)[0].siteId;
        jj.scriptLog("info", "site:", targetSiteIds)
## }
    jj.scriptLog("info", "log", targetSiteIds)
## let result1 = {
### targetSiteId: targetSiteIds
## }
    return {result: JSON.stringify(result1)}
## }
### (2)运行天风任务①导入天风任务→②运行
1698810081366-affdb1e1-d47d-4cc9-89f2-4788388a455e.png

## (3) 在rds检查运单和库位状态
## 运单状态
1699263182068-6c9f2c2f-df48-4206-9580-ea2c249aa4dd.png

## 库位状态
1704161373057-94be3b6b-d1df-4e2f-af32-04ba339fa7d8.png

## ⚠️ 需要注意的点
如果是机器人是地牛类型， 货叉只有升降， 没有指定高度，高度大于0即为货叉抬升， 为0时为货叉下降
流程中会用到其它功能如栈板识别， 可能影响任务效率， 可根据实际要求取舍
