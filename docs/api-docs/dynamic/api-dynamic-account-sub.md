---
sidebar_position: 4
slug: /api-dynamic-account-sub
description: 动态代理子账号API, 用于获取动态代理子账号信息、创建动态代理子账号、修改动态代理子账号信息
---
# 代理子账号
---
请求地址（baseUrl）： `http://api.ipweb.cc:8004`

## 1. 创建子账号

**POST** `{baseUrl}/api/agent/childAccount/add`


### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
username |  是 |  String |  子账号（字母和数字2到12位）  
pwd |  是  |  String |  子账号密码（字母和数字 4到12位）  
limitFlow |  否 |  Integer |  流量限制M 小于0为不限制）默认-1  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，其他表示失败  
msg |  String |  描述  

### 返回示例：
```text
{
    "code": 200,
    "msg": "查询成功"
}
```
## 2. 修改子账号密码

**POST** `{baseUrl}/api/agent/childAccount/updatePwd`

### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
username |  是 |  String |  子账号  
pwd |  是  |  String |  子账号密码（字母和数字 4到12位）  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，其他表示失败  
msg |  String |  描述  

### 返回示例：
```text
{

  "code": 200,

  "msg": "操作成功"

}
```
## 3. 修改子账号流量限制

**POST** `{baseUrl}/api/agent/childAccount/updateLimitFlow`


### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
username |  是 |  String |  子账号  
limitFlow |  是  |  Integer |  流量限制M（小于0为不限制）默认-1  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，其他表示失败  
msg |  String |  描述  

### 返回示例：
```text
{

  "code": 200,

  "msg": "操作成功"

}
```
## 4. 修改子账号状态

**POST** `{baseUrl}/api/agent/childAccount/updateStatus`


### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
username |  是 |  String |  子账号  
status |  是  |  Integer |  状态（0停用 1正常）  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，其他表示失败  
msg |  String |  描述  

### 返回示例：
```text
{

  "code": 200,

  "msg": "操作成功"

}
```
## 5. 获取子账号列表

**POST** `{baseUrl}/api/agent/childAccount/users`


### 返回结果：

参数名 |  |  类型 |  说明  
---|---|---|---  
code |  |  Integer |  200表示成功，其他表示失败  
data |  |  Array |
|  download_flow |  Float |  下载流量M  
|  upload_flow |  Float |  上传流量M  
|  limit_flow |  Integer |  流量限制M（小于0为不限制）  
|  pwd |  String |  密码  
|  username |  String |  用户名  
|  status |  Integer |  状态（0停用 1正常）  

### 返回示例：
```text
{
    "msg": "操作成功",
    "code": 200,
    "data": [
        {
            "download_flow": 33.52092,
            "upload_flow": 5.37097,
            "limit_flow": -2,
            "id": 1,
            "pwd": "password22",
            "username": "username11",
            "status": 1
        }
    ]
}
```
## 6. 获取用户

**GET** `{baseUrl}/api/agent/account?country=us&time=5`

### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
country |  是 |  String |  国家/地区 英文简称  
times |  是  |  Integer |  ip有效时长  
repeat |  否 |  Integer |  1重复 0去重 默认1  

### 返回结果：

参数名 |  |  类型 |  说明  
---|---|---|---  
code |  |  Integer |  200表示成功，其他表示失败  
data |  |  Json |
|  userName |  String |  用户名  
|  upload_flow |  String |  密码  

### 返回示例：
```text
{
    "msg": "操作成功",
    "code": 200,
    "data": {
        "passWord": "987654321a123456789",
        "userName": "100951637280-ABCDEFG"
    }
}
```
## 7. 批量获取用户

**GET** `{baseUrl}/api/agent/account2?country=us&time=5&limit=3`


### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
country |  是 |  String |  国家/地区 英文简称  
times |  是  |  Integer |  ip有效时长  
limit |  是 |  Integer |  获取账号数量  
repeat |  否 |  Integer |  1重复 0去重 默认1  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，其他表示失败  
data |  Array |  格式: 用户名:密码  

### 返回示例：
```text
{
    "msg": "操作成功",
    "code": 200,
    "data": [
        "100951637280-AvKUhzlR:123456789a987654321",
        "100951637280-wAfsZkSi:123456789a987654321",
        "100951637280-nOO1SJQu:123456789a987654321"
    ]
}
```
## 8. 子账号格式定义

### 1、粘贴方式  
```text
账号（12位数字与字母)_custom_zone_us_st_[areaId]_city_[cityId]_sid_(8位数字)_time_(时长小于90分钟)


全球随机：
账号（12位数字与字母)_custom_zone_global_sid_(8位数字)_time_(时长小于90分钟)
洲随机：
账号（12位数字与字母)_custom_zone_asia_sid_(8位数字)_time_(时长小于90分钟)
账号（12位数字与字母)_custom_zone_europe_sid_(8位数字)_time_(时长小于90分钟)
账号（12位数字与字母)_custom_zone_africa_sid_(8位数字)_time_(时长小于90分钟)
账号（12位数字与字母)_custom_zone_america_sid_(8位数字)_time_(时长小于90分钟)        
```

### 2、轮转

```text
账号（12位数字与字母)_custom_zone_us_st_[areaId]_city_[cityId]
全球随机：
账号（12位数字与字母)_custom_zone_global
洲随机：
账号（12位数字与字母)_custom_zone_asia
账号（12位数字与字母)_custom_zone_europe
账号（12位数字与字母)_custom_zone_africa
账号（12位数字与字母)_custom_zone_america
```