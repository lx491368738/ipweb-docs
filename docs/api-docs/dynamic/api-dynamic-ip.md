---
sidebar_position: 1
slug: /api-dynamic-ip
description: 动态流量 API：查询动态IP信息
---
# 动态IP
---
## 1.   获取城市ip量分布

**GET** `/api/agent/cityCount/{countryId}/{areaId}`

 

### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
countryId |  是 |  String |  国家/地区id  
areaId |  是  |  String |  州id  

### 返回结果：

参数名 |  |  类型 |  说明  
---|---|---|---  
code |  |  Integer |  200表示成功，其他表示失败  
row |  |  Array |
|  cityId |  String |  城市id  
|  cityName |  String |  城市名称  
|  label  |  String |  ip量级，[少，较多，多]  

### 返回示例：
```text
{
    "rows": [
        {
            "cityName": "Cupertino",
            "cityId": "30113",
            "label": "少"
        },
        {
            "cityName": "Cypress",
            "cityId": "16025",
            "label": "较多"
        },
        {
            "cityName": "Rancho Santa Margarita",
            "cityId": "13083",
            "label": "多"
        }
    ],
    "code": 200,
    "msg": "查询成功"
}
```
## 2. 获取分组

**GET** `/api/agent/account?country=us&times=5&repeat=1`


### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
country |  是 |  String |  国家/地区英文  
times |  是  |  Integer |  有效时长  
repeat |  是 |  Integer |  1重复  0去重  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，其他表示失败  
data |  String |  分组号  

### 返回示例：
```text
{
    "msg": "成功",
    "code": 200,
    "data": "100004509283"
}
```
## 3. 批量获取分组

**GET** `/api/agent/account?countrys=us&times=5&repeat=1`


### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
country |  是 |  String |  国家/地区英文  
times |  是  |  Integer |  有效时长  
repeat |  是 |  Integer |  1重复  0去重  

### 返回结果：

参数名 |  |  类型 |  说明  
---|---|---|---  
code |  |  Integer |  200表示成功，其他表示失败  
data |  |  Array |
|  country |  String |  国家  
|  groupId |  String |  分组号  

### 返回示例：
```text
{
    "msg": "成功",
    "code": 200,
    "data": [
        {
            "country": "US",
            "groupId": "100004509283"
        },
        {
            "country": "AE",
            "groupId": "101626509765"
        }
    ]
}
```
## 4.  获取州ip量分布

**GET** `/api/agent/countArea/{countryId}`


### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
countryId |  是 |  String |  国家/地区id  

### 返回结果：

参数名 |  |  类型 |  说明  
---|---|---|---  
code |  |  Integer |  200表示成功，其他表示失败  
row |  |  Array |
|  areaId |  String |  州id  
|  areaName |  String |  州名称  
|  label  |  String |  ip量级，[少，较多，多]  

### 返回示例：
```text
{
    "rows": [
        {
            "areaId": "1583",
            "areaName": "California",
            "label": "少"
        },
        {
            "areaId": "2703",
            "areaName": "North Dakota",
            "label": "较多"
        },
        {
            "areaId": "2713",
            "areaName": "Alaska",
            "label": "多"
        }
    ],
    "code": 200,
    "msg": "查询成功"
}
```
---


© 2025 动态流量 API 接口文档 - 版本 1.0.0

最后更新时间：2025 年 12 月 26 日