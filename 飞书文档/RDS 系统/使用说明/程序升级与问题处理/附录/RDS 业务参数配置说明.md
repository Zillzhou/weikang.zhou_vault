# RDS 业务参数配置说明

## RDS 业务参数配置说明
下列配置是配置在application-biz.yml 文件中：
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
        # 需求单扩展属性，配置后使用脚本方法添加需求单时，可设置扩展属性
## extensionFields:
## - id: 1
### attributeName: 扩展字段1
## - id: 2
### attributeName: 扩展字段2
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
          - value: site_id      #需要显示的查询字段
            label: 库位id        #手持端页面表头显示名称
          - value: locked       #需要显示的查询字段
            label: 是否锁定       #手持端页面表头显示名称
## # 字段值转成需要展示的文字
## status:
            # 当locked这个字段值为0时，页面对应的要显示成未锁定，当值为1时，页面对应的要显示成锁定
## - value: 0
## label: 未锁定
## - value: 1
## label: 锁定
          - value: locked_by   #需要显示的查询字段
### label: 锁定者       #手持端页面表头显示名称
## # 任务菜单数组
## orders:
## # 菜单id
### - menuId: moveTaskChain
## # 菜单名
## label: 运输任务链
## # 下单的响应方法(POST)路由
### route: moveTaskChainFunc
## # 菜单背景颜色
### menuItemBackground: #ffff33
## # 菜单文字颜色
### menuItemTextColor: #aff2dd
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

## # 是否开启MQTT协议功能
## mqttConfigView:
## enable: false
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
