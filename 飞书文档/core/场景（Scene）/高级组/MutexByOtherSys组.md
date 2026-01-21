# MutexByOtherSys组

## MutexByOtherSys组
## @杨达
1675941037845-283bce9d-ff2a-44a2-9f27-014f51d837a1.png

## 功能
外部系统管理互斥组状态，core进入时申请，离开时释放。

### 0.1.9.241204对此功能进行了重构，具体区别请看下表：

## 功能点

### 0.1.9.241204之前

### 0.1.9.241204之后

## 可以包含的元素种类

### 只能是MutexRegion，不支持点、线

## 支持MutexRegion、点、线

## 可以进入的机器人数

## 只能有一辆Core控制的机器人进入

## 可以有多辆Core控制的机器人进入

## 请求外部系统时机

### 将要进入时会频繁申请释放，进入后不会再释放，离开后会释放

将要进入时，申请直到成功，组内所有Core控制的机器人都离开才释放

## 支持的系统间互斥组数量

## 最多十个。数量多时会导致调度卡顿

## 不限，不会导致调度卡顿
## 表中没有提到的功能保持不变

## 对外部系统的要求
提供http服务器，实现下列申请、释放互斥组的API，维护互斥组占用状态
## 参数配置
将UseOtherSysBlockGroup项改为true，启用该功能，
### 如果为false，此互斥组被Core忽略，等于没有设置
### 之前为true，改为false时，会释放占用的互斥组

## 参数名称

## 取值范围

## 支持版本

### UseOtherSysBlockGroup

## true/false

## >0.1.8.230211
### 需要控制请求频率时，修改下列参数，单位为毫秒

## 参数名称

## 取值范围

## 支持版本

BlockGroupOtherSysRequestInterval

## 0-10000

## ~latest
## 申请互斥组
## core请求外部系统
### POST "http://host:port/path"
## {
  "robot_name":"AMB-1",
### "block_group_id":"BG-1"
## }
## 外部系统响应
## {
  "code":200,
## "message":"ok"
## }
## {
  "code":400,
  "message":"failed, owned by KC-1"
## }
## 释放互斥组
## core请求外部系统
### POST "http://host:port/path"
## {
  "robot_name":"AMB-1",
### "block_group_id":"BG-1"
## }
## 外部系统响应
## {
  "code":200,
## "message":"ok"
## }
## {
  "code":400,
  "message":"failed, owned by KC-1"
## }
## 新配置

### 0.1.9.240309及之后
## 支持配置申请、释放互斥组的请求内容
## 支持配置申请成功对应的报文
## image.png

## 配置请求/释放报文
acquireTemplate ， releaseTemplate 属性为json字符串或者URL请求字符串。按照项目需要配置。支持两个变量，注意，变量要在双引号内：
RobotName 表示机器人。（0.1.9.241204之后，全部为CORE不再区分机器人）
## BlockGroup 表示互斥组
上图中的例子， acquireTemplate 中的变量被替换后，如下：

### 0.1.9.1204之前：
{"SystemName":"AMB-01", "Devices":"2"}

### 0.1.9.241204之后：
{"SystemName":"CORE", "Devices":"2"}
机器人AMB-01申请互斥组2时，就会向服务器发送上述请求报文。
## POST请求
### acquireRequestMethod 选择POST
acquireTemplate 和 releaseTemplate 为json字符串
## GET请求
### acquireRequestMethod 选择GET
acquireTemplate和releaseTemplate 为URL参数字符串，如下图：
## image.png

## 配置响应成功时的状态码
acquireResponseCode releaseResponseCode 为字符串。按照项目需求配置。默认为200。改为非数字值，表示不检查响应码。如没有特殊需求，建议配置为200。
## 配置响应成功时的报文
acquireResponseBody releaseResponseBody 为json字符串。按照项目需求配置。默认为
## {
  "message":"ok",
## "code":200
## }
匹配规则 ：配置中出现的字段，在实际报文中都出现，且值和配置值一致时，认为请求成功。
例如，图中配置的 acquireResponseBody 为 {"result":"true"} ，服务器响应如下：
## {
  "result":"true",
### "timestamp":"2024-03-01"
## }
响应中包含 配置中的 result 字段，且值为true，和配置一致，认为请求成功。实际响应中有，但是配置中没有的字段，不影响请求是否成功的判断。
## 下面几个响应表示请求失败：
## {
  "result":"false",
## "msg":"ok"
## }
## {
## "Result":200
## "msg":"OK"
## }
