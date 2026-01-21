# 发送 POST 请求，参数为 XML 格式

### 发送 POST 请求，参数为 XML 格式
## 方法说明
本方法是阻塞方法，发送 POST 请求，参数为 XML格式。在本方法中会自动将传入的 JSON 格式字符串转化为 XML 格式
function requestPostXml(url: string, param: string): string

## 输入参数
url，string 类型，请求的 URL。
param，string 类型，JSON 字符串，请求的参数。
## 输出参数
若请求成功：将响应的 XML 格式 字符串 转化为 JSON 格式并返回。
若请求失败：null。
## 异常
本方法会不抛出异常。
