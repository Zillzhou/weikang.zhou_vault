# 设置库位被 Core 占用

## 设置库位被 Core 占用
## 接口介绍
### 此 API 可以设置库位的占用者为Core
## 请求
## 功能：设置库位的占用者为Core
## 方法：POST
接口说明： /api/work-sites/holdWorksiteByCore 
## 请求示例的URL
POST http://host:8080/api/work-sites/holdWorksiteByCore
## 请求参数

## Name

## Type

## Description

## Required

## siteId

## String

## 库位Id

## 是
## 请求示例
## {
## "siteId": "Loc-02"
## }
## 响应

## Name

## Type

## Description

## code

## int

## API 错误码，详情见 API 错误码

## msg

## String

## API错误码信息

## data

## Object

## 返回数据对象
## 响应示例
## Responses Code 200
## 请求成功，响应正文格式如下：
## {
    "code": 200,
    "msg": "Success",
## "data": null
## }
