---
sidebar_position: 2
slug: /api-dynamic-customer
description: 开放平台客户记录 API：查询客户列表，请求示例、响应格式与错误码说明。
---

# 代理客户

---
## 1. 获取代理的客户记录

**GET** `/open/customer/list`

**功能描述：** 获取代理的客户记录，支持分页查询和条件过滤

#### 请求参数

| 参数名   | 类型    | 必填 | 说明     | 默认值 |
| -------- | ------- | ---- | -------- | ------ |
| pageNum  | Integer | 否   | 页码     | 1      |
| pageSize | Integer | 否   | 每页条数 | 10     |
| userName | String  | 否   | 客户账号 | null   |

#### 响应数据格式

| 字段名                   | 类型                  | 说明                     |
| ------------------------ | --------------------- | ------------------------ |
| code                     | Integer               | 响应状态码，200 表示成功 |
| msg                      | String                | 响应消息                 |
| data                     | Object                | 响应数据对象             |
| data.total               | Long                  | 总记录数                 |
| data.rows                | `` `Array<Object>` `` | 数据列表                 |
| data.rows[].trafficCount | BigDecimal            | 充值流量数（单位 MB）    |
| data.rows[].residualFlow | BigDecimal            | 剩余流量（单位 MB）      |
| data.rows[].customerId   | Long                  | customerId               |
| data.rows[].userName     | String                | 用户账号（登录名称）     |

#### 请求示例

```bash
curl -X GET "http://user.ipweb.cc/prod-api/open/customer/list?pageNum=1&pageSize=10&userName=" -H "token: your-access-token"
```

#### 响应示例

**成功响应：**

```json
{
  "msg": "操作成功",
  "code": 200,
  "data": {
    "total": 1,
    "rows": [
      {
        "trafficCount": 512000000.0,
        "residualFlow": 205587097.6,
        "customerId": 39,
        "userName": "bazhuayu@gmaill.com"
      }
    ],
    "code": 200,
    "msg": "查询成功",
    "other": null
  }
}
```

**认证失败响应：**

```json
{
  "code": 200,
  "msg": "无效的认证令牌",
  "data": null
}
```

## 2.  创建账号

**GET** `/api/agent/addUser?userName=lisi@q.com`

**功能描述:** 创建用户账号

### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
userName |  是 |  String |  创建账号昵称(邮箱格式)  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，201表示名称存在,是该渠道的客户，202表示名称存在,不是该渠道的客户 其他表示失败  
msg |  String |  响应描述  
customerId |  Integer |  客户id  
Token |  String |  用户Token  
password |  String |  用户随机验证密码  

### 返回示例：
```text
{
    "msg": "注册失败，该用户名已存在,不是该渠道的客户",
    "code": 202
}

{
    "msg": "操作成功",
    "password": "abcdefg",
    "code": 200,
    "customerId": 888,
    "Token": "AAEYX6KEXMJUZ86NVZRASRZF50A"
}
```
## 3. 修改验证(代理)密码

**GET** `/api/agent/updatePwd?customerId=888&password=123456`



### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
customerId |  是 |  String |  被修改的客户id  
password |  是 |  String |  客户新密码  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，201表示名称存在,是该渠道的客户，其他表示失败  
msg |  String |  响应描述  

### 返回示例：
```text
{
    "msg": "成功",
    "code": 200
}
```
## 4. 客户关停

**GET** `/api/agent/changeStatus?customerId=888&customerState=1`

 

### 请求参数：

参数名 |  是否必传 |  类型 |  说明  
---|---|---|---  
customerId |  是 |  String |  被修改的客户id  
customerState |  是 |  Integer |  关停状态 （0正常 1停用）  

### 返回结果：

参数名 |  类型 |  说明  
---|---|---  
code |  Integer |  200表示成功，201表示名称存在,是该渠道的客户，其他表示失败  
msg |  String |  响应描述  

### 返回示例：
```text
{
    "msg": "成功",
    "code": 200
}
```

---
© 2025 代理客户查询 API 文档 - 版本 1.0.0

最后更新时间：2025 年 12 月 26 日
