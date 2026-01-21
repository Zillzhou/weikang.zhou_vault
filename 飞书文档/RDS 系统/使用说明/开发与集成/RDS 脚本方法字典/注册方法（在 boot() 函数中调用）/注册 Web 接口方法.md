# 注册 Web 接口方法

## 注册 Web 接口方法
## 方法说明
在脚本中注册接口方法。
function registerHandler(method: string, path: string, callback: string, auth: boolean): void

## 输入参数
method：HTTP 请求方式，支持 POST，GET。
path：接口的 URL 路径。
callback：处理接口的脚本方法名。
auth：调用此接口是否需要登录。
## 输出参数
## 无
## 异常
本方法会抛出异常。
