# BasicAuth认证说明

## BasicAuth认证说明
## 原理和实现
BasicAuth 是一种常见的系统接口认证方式，它会检查 API 接口请求头中的 Authorization 字段，从中解析出加密后的 Username 和 Password，然后将结果与服务器保存的进行对比，如果一致则通过；否则，返回认证失败的信息，认证失败表示请求客户端请求 API 接口时没有携带或携带了错误的认证信息。
RDS 在脚本中自定义的对外接口（url 以 /script-api 为前缀） 和通过【接口配置】功能配置的对外接口（url 以 /handle/register 为前缀）都支持 BasicAuth 接口认证方式。
## 使用方法
## 1.修改 RDS 配置 参数
首先打开 RDS【设置】界面，点击“图形配置”按钮，出现下图参数：
enable：勾选则代表启动 BasicAuth 认证，不勾选代表关闭 BasicAuth 认证。
basicAuthUsername：表示认证用的用户名，默认为 authUser，可进行更改。
basicAuthPassword：表示认证用的密码 ，默认为 rds，可进行更改。
更改后，点击保存，需要重启 RDS 方可生效。
1687318665048-98a749ba-df2a-41c9-a591-49a8ffbd36a3.png 

## 图 1
重启成功后，RDS 自定义的接口将开启 BasicAuth 认证，以下通过 Postman 测试 BasicAuth 认证的接口如何使用。
### 2.接口测试
以 Postman 为例，选择 Authorization 标签，再点击 Basic Auth，就会看到 Username 和 Password 字段，这两个字段分别代表用户名和密码。
1687319181110-4daad016-1366-4309-af37-9ec03b45f1f2.png

## 图 2
1687326933913-2239d59b-5382-4906-b9df-86ace2accf46.png

## 图 3
Username 文本框 输入图1 basicAuthUsername参数配置的值 authUser， Password 输入图1的 basicAuthPassword 参数配置的值 rds，此时点击 Postman 中的 Header 标签页，如图 4 可以看到 Headers 内出现了 Authorization 字段，其Value是以Basic字符串开头，Basic 后面附加的 YXV0aFVzZXI6cmRz 字符串是根据Username和Password 通过 Base64 算法加密后生成。
此时通过Postman调用 RDS 接口，RDS 系统将校验请求头中的 Authorization 字段进行认证。
1687328938433-e21f9411-1925-41a9-bab9-42ebc37bb2c4.png

## 图4
注：Value: Basic YXV0aFVzZXI6cmRz
是通过 Basic $(base64_encode( Username : Password ))生成，生成方式：
## 1.将 Username 和 Password 对应的值 以英文冒号拼接，比如 authUser:rds。
### 2.对 authUser:rds 进行Base64加密，得到加密后的字符串：YXV0aFVzZXI6cmRz
3.将Basic 和 YXV0aFVzZXI6cmRz 两字符串以空格间隔拼接，就可得到完整的 Authorization 请求头Value： Basic YXV0aFVzZXI6cmRz 。
### 3.发送请求
### 3.1 Postman 填写与 RDS 配置相同的 用户名密码，Postman 发送接口可返回正确的接口响应。
1687323736992-acf331b7-9376-4def-90d3-0c9911fb5432.png

3.2 填写与 RDS 配置不匹配的 用户名密码，或者不填写用户名密码，响应码 code 9051，msg为API Authentication Failed，表示接口认证失败。
1687323798298-6e39920e-c308-494a-bebd-a70713e97afc.png
