# 设备通信代理（proxy)

## 设备通信代理（proxy)
## @杨达
## 解决的问题

ModbusTCP的一些实现有连接数和访问速度限制，不能每个设备维护一个连接
## 多个设备公用的通信需要多次配置

## 解决方式

对于每个配置的ModbusTCP类型的通信代理，只维护一个连接
在设备信息中配置通信代理的名称即可，具体的通信配置，如ip，端口等在通信代理中配置

## 配置方式
## 目前支持三种设备通信代理：
### 0.1.8.221115 增加了ModbusTCPSpecificIP

## 种类名称

## 通信方式

## 备注

## ModbusTCP

## TCP

ModbusTCP通信协议，core作为master(客户端)

## ModbusTCPSpecificIP

## TCP

ModbusTCP通信协议，core作为master(客户端)，支持绑定客户端IP

## HTTP

## HTTP

## core作为客户端

## ModbusTCP类型代理的配置项：

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## ip

### 模型文件-proxy-ModbusTCP

## string

### 127.0.0.1

## -

## IP地址

## port

### 模型文件-proxy-ModbusTCP

## uint16_t

## 502

## -

## 端口号

## slaveId

### 模型文件-proxy-ModbusTCP

## uint16_t

## 1

## -

## Modbus的slave号

## timeout

### 模型文件-proxy-ModbusTCP

## s

## 120

## -

## 超时时间

## 例子：
ip为127.0.0.1，端口为502，slave id为1，超时为120秒的ModbusTCP通信代理配置
## image.png

### ModbusTCPSpecificIP 类型代理的配置项：

## 参数名称

## 参数位置

## 单位

## 默认值

## 最小值

## 最大值

## serverIp

### 模型文件-proxy-ModbusTCPSpecifiIP

## string

### 127.0.0.1

## -

## 服务器IP地址

## port

### 模型文件-proxy-ModbusTCPSpecifiIP

## uint16_t

## 502

## -

## 端口号

## slaveId

### 模型文件-proxy-ModbusTCPSpecifiIP

## uint16_t

## 1

## -

## Modbus的slave号

## clientIp

### 模型文件-proxy-ModbusTCPSpecifiIP

## string

### 127.0.0.1

## -

## 客户端IP

## 例子：
下图中为 客户端 IP，即本机IP为 192.168.12.3 ； 服务器 IP，即设备IP为 192.168.1.21 的设备通信代理。
1672118870950-6235113c-a634-400b-b85e-5ec1fa6e742a.png

## HTTP类型代理的配置项：

## 参数名称

## 参数位置

## 类型

## 默认值

## 最小值

## 最大值

## baseurl

## 模型文件-proxy-HTTP

## string

## http://example.com

## -

HTTP代理的地址前缀，只能是“scheme://host:port”形式。即，不支持 “http://192.168.21.34:9999/api”，支持“http://192.168.21.34:9999”

## 例子：
baseurl为 http://example.com 的HTTP通信代理
## image.png

## 注意：
url的拼接方式为：baseurl+/controlDoor 格式，使用路由区分API，而不是路径参数
