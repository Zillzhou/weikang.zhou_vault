# MQTT 发布信息（使用配置文件的topic）

### MQTT 发布信息（使用配置文件的topic）
## MQTT 发布信息

## 方法说明
向配置的 topic 发布信息 。
function MQTTPublish(message: string): void;

## 输入参数
### message，string 类型，需要发布的信息
## 输出参数
无。
## 异常
本方法不会抛出异常。 
## 使用示例
## 1. 在 application-biz.yml 里面开启MQTT，并完成相应的配置，如下：
## # 是否开启 MQTT 服务
## mqttConfigView:
## enable: true
## pubConfig:
## # 服务器端点url
    broker: tcp://broker.emqx.io:1883
    # 订阅的主题，MQTT允许使用通配符订阅主题，不允许使用通配符pub发布消息
## topics:
## - Examples/1/123
    # 设置message的服务质量（0：消息最多传递一次（零次或一次）1：至少传递一次（一次或多次）。2：只传递一次）
## qos: 2
## # 客户端唯一标识
## clientId: RDS-Pub
## # 连接的用户名，不需要就填 null
## username: null
## # 连接的密码,不需要就填 null
## password: null
    # 设置是否清空session,false表示服务器会保留客户端的连接记录，true每次都以新的身份连接服务器
## cleanSession: false
## # 超时时间(seconds)
### connectionTimeout: 30
## # 设置会话心跳时间(seconds)
### keepAliveInterval: 60
## # 设置断开后重新连接
### automaticReconnect: true
    # 表示发送的消息需要一直持久保存（不受服务器重启影响），不但要发送给当前的订阅者，并且以后新来的订阅了此Topic name的订阅者会马上得到推送。
## retained: false
    # 作为publish的遗嘱消息，会存到服务器，在publish端非正常断连的情况下，发送给所有订阅的客户端
## # 不需要就填 null
## willMsg: null
## # 遗嘱消息发布的topic
### willTopic: Examples1
## subConfig:
## # 服务器端点url
    broker: tcp://broker.emqx.io:1883
    # MQTT允许使用通配符sub订阅主题，但不允许使用通配符pub发布消息
## topics:
## - Examples/1/123
    # 设置message的服务质量（0：消息最多传递一次（零次或一次）1：至少传递一次（一次或多次）。2：只传递一次）
## qos: 1
## # 客户端唯一标识
## clientId: RDS-Sub
## # 连接的用户名，不需要就填 null
## username: null
## # 连接的密码,不需要就填 null
## password: null
    # 设置是否清空session,false表示服务器会保留客户端的连接记录，true每次都以新的身份连接服务器
## cleanSession: false
## # 超时时间(seconds)
### connectionTimeout: 30
## # 设置会话心跳时间(seconds)
### keepAliveInterval: 60
## # 设置断开后重新连接
### automaticReconnect: true
    # 表示发送的消息需要一直持久保存（不受服务器重启影响），不但要发送给当前的订阅者，并且以后新来的订阅了此Topic name的订阅者会马上得到推送。
## retained: false
### 2. 编写脚本方法
  jj.MQTTPublish("RDS-Script sub message.");
