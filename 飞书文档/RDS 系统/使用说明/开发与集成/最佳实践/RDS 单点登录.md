# RDS 单点登录

## RDS 单点登录
RDS 目前支持 OAuth2 方式的单点登录，使用时在 RDS 系统配置文件 application.yml 中开启即可。
## 基本信息
### RDS 单点登录基于 OAuth2 实现
授权模式使用 Authorization Code 授权码模式
如需要使用 HTTPS 方式访问，RDS 默认证书格式为 JKS （可自定义其他类型配置）
单点登录交互过程中 HTTP 接口通讯使用 Basic 认证方式
在开启单点登录前，确认 RDS 服务器可以访问外网，可以访问客户的授权中心。
以下介绍配置文件中各个字段的含义。
## oauth:
## enable: false
  authorizationUri: http(s)://xxx/xxx
### tokenUri: http(s)://xxx/xxx
  userInfoUri: http(s)://xxx/xxx
### userAttributeName: xx
## clientId: xx
## clientSecret: xx
### scope: profile openid
### grantType: authorization_code
## resType: code
  redirectBaseUri: http(s)://rds-ip:port
  redirectEndpoint: /oath/authorize
## admins: 张三
### homeUrl: /index.html#/view
## pdaHomeUrl: /pda/
## logoutUrl: xxx
### enableLoginPage: false
### ssoButtonText: SSO Login
## 释义：
enable：默认 false，不开启单点登录。如需要开启单点登录功能，请修改为true.
authorizationUri：RDS 用来访问的授权中心地址，由 客户提供 ，用来登录用户授权界面。
tokenUri：RDS 用来获取用户 token 的地址，由 客户提供 。
userInfoUri：获取用户基本信息的地址，由 客户提供 。通过改地址可以得到类似以下信息。
## {
  "username": "张三",
### "email": "123@456.com"
## }
userAttributeName：RDS 要保存的用户名信息。需要基于第 4 步配置，如上所示，根据用户信息结构，则本字段就填写 userAttributeName：username ，由 username 就拿到了"张三"，则保存用户名到 RDS 中，该用户信息 JSON 结构由 客户提供 。
clientId：单点登录的客户端ID， 客户提供 。
clientSecret：单点登录的客户端密钥， 客户提供 。
scope：默认，不需修改，用于访问用户信息。
grantType：授权模式，请勿修改。
resType: 授权模式，请勿修改。
redirectBaseUri：填写 RDS 部署的服务器 IP 和端口，如果有域名则填写域名和端口即可。
redirectEndpoint：请勿修改。
admins：默认管理员账户名，需要录入一个/多个(用逗号隔开)客户的实际存在的用户名，作为管理员。
homeUrl：RDS 主页，请勿修改。
pdaHomeUrl：pda 的网页端地址，请勿修改。
logoutUrl：单点登录的登出地址，由 客户提供 （如果有的话，没有可以不填）。
enableLoginPage: 是否开启 RDS 登录界面，false 隐藏登录页，单点登录直接跳转至第三方授权；true 则在登录页的普通登录按钮下方额外增加一个单点登录按钮。
ssoButtonText: 此设置表示开启 enableLoginPage 后，单点登录按钮上的显示内容。

## 更新版本
## Robod 版本 < 4.4.9
如果 RDS 需要升级版本，升级之后配置文件 application.yml 会被覆盖，请在升级之前务必备份好此配置文件，升级之后将该文件重新替换进去。
## linux 系统：
application.yml 配置文件所在目录：/opt/data/rds/app。
## windows 系统：
application.yml 配置文件默认所在目录：C:/RDS/data/rds/app，如果安装时选择了D盘作为安装盘，则对        应目录为 D:/RDS/data/rds/app。
找到 application.yml 配置文件后，在升级 RDS 版本前，将其备份，备份完成后进行 RDS 版本升级，升级完成后，再将备份的原 application.yml 替换回原目录，重启RDS系统，完成。

## Robod 版本 ≥ 4.4.9
RDS 升级版本，不会出现 application.yml 配置文件被覆盖的情况，可以不考虑额外的备份替换。
