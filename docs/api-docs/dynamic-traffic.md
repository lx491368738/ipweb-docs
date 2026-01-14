---
sidebar_position: 2
slug: /dynamic-traffic
description: 动态流量 API：查询充值与消耗记录、代理客户流量报表，包含认证方式与示例。
---

# 动态流量

## 目录

- [动态流量](#动态流量)
  - [目录](#目录)
  - [1. 接口概述](#1-接口概述)
  - [2. 认证方式](#2-认证方式)
    - [请求头格式](#请求头格式)
    - [示例](#示例)
    - [Token 格式说明](#token-格式说明)
  - [3. API 接口列表](#3-api-接口列表)
    - [3.1 获取我的流量充值记录](#31-获取我的流量充值记录)
      - [请求参数](#请求参数)
      - [响应数据格式](#响应数据格式)
      - [请求示例](#请求示例)
      - [响应示例](#响应示例)
    - [3.2 获取给我的流量充值记录](#32-获取给我的流量充值记录)
      - [请求参数](#请求参数-1)
      - [响应数据格式](#响应数据格式-1)
      - [请求示例](#请求示例-1)
      - [响应示例](#响应示例-1)
    - [3.3 获取代理客户的流量使用记录](#33-获取代理客户的流量使用记录)
      - [请求参数](#请求参数-2)
      - [响应数据格式](#响应数据格式-2)
      - [请求示例](#请求示例-2)
      - [响应示例](#响应示例-2)
  - [4. 响应格式说明](#4-响应格式说明)
  - [5. 错误码说明](#5-错误码说明)

---

## 1. 接口概述

动态流量 API 提供了完整的流量充值历史查询功能，包括查询为客户充值的记录和查询自己的充值记录。

**基础 URL：**

- **HTTP：** `http://user.ipweb.cc/prod-api/open/dynamic`
- **HTTPS：** `https://user.ipweb.cc/prod-api/open/dynamic`

**支持格式：** JSON

**字符编码：** UTF-8

**协议支持：** 同时支持 HTTP 和 HTTPS 协议，推荐使用 HTTPS 以确保数据传输安全

---

## 2. 认证方式

**认证方式：** Token 认证

所有 API 请求都需要在请求头中包含有效的认证令牌。系统会验证令牌的有效性，无效令牌将返回认证失败错误。

### 请求头格式

在所有 API 请求中，需要在 HTTP 请求头中添加以下认证信息：

```
Token: your_access_token_here
```

### 示例

```
# 使用curl命令示例
curl -X GET "http://user.ipweb.cc/prod-api/open/dynamic/myRechargeFlowList" \
     -H "Token: abc123def456ghi789jkl012mno345pqr678stu901vwx234yz"

# 使用JavaScript fetch示例
fetch("http://user.ipweb.cc/prod-api/open/dynamic/myRechargeFlowList", {
  method: 'GET',
  headers: {
    'Token': 'abc123def456ghi789jkl012mno345pqr678stu901vwx234yz',
    'Content-Type': 'application/json'
  }
})
```

### Token 格式说明

- **请求头名称：** Token
- **Token 值：** 由系统分配的访问令牌字符串
- **Token 长度：** 通常为 32-64 位字符的字母数字组合
- **有效期：** Token 具有有效期限制，过期后需要重新获取
- **安全性：** 请妥善保管 Token，避免泄露给第三方

---

## 3. API 接口列表

### 3.1 获取我的流量充值记录

**GET** `/myRechargeFlowList`

**功能描述：** 获取我的流量充值记录，支持分页查询和条件过滤

#### 请求参数

| 参数名    | 类型    | 必填 | 说明                       | 默认值 |
| --------- | ------- | ---- | -------------------------- | ------ |
| pageNum   | Integer | 否   | 页码                       | 1      |
| pageSize  | Integer | 否   | 每页条数                   | 10     |
| type      | Integer | 否   | 类型：0 充值 1 消耗        | 0      |
| account   | String  | 否   | 被充值账号                 | null   |
| startDate | String  | 否   | 开始日期，格式：yyyy-MM-dd | null   |
| endDate   | String  | 否   | 结束日期，格式：yyyy-MM-dd | null   |

#### 响应数据格式

| 字段名                 | 类型                  | 说明                                |
| ---------------------- | --------------------- | ----------------------------------- |
| code                   | Integer               | 响应状态码，200 表示成功            |
| msg                    | String                | 响应消息                            |
| data                   | Object                | 响应数据对象                        |
| data.total             | Long                  | 总记录数                            |
| data.rows              | `` `Array<Object>` `` | 数据列表                            |
| data.rows[].customerId | Long                  | 客户 ID                             |
| data.rows[].account    | String                | 账号                                |
| data.rows[].flowCount  | BigDecimal            | 流量数(M)                           |
| data.rows[].typeDesc   | String                | 类型描述                            |
| data.rows[].createId   | Long                  | 操作人 ID                           |
| data.rows[].createBy   | String                | 操作人账号                          |
| data.rows[].createTime | String                | 创建时间，格式：yyyy-MM-dd HH:mm:ss |

#### 请求示例

```bash
GET http://user.ipweb.cc/prod-api/open/dynamic/myRechargeFlowList?pageNum=1&pageSize=10&type=0&account=test@cc.cc&startDate=2025-04-20&endDate=2025-09-20
```

#### 响应示例

**成功响应：**

```json
{
  "code": 200,
  "msg": "查询成功",
  "data": {
    "total": 2,
    "rows": [
      {
        "customerId": 12345,
        "account": "customer1",
        "flowCount": 100.0,
        "typeDesc": "充值",
        "createId": 98765,
        "createBy": "admin",
        "createTime": "2025-11-17 10:30:00"
      },
      {
        "customerId": 12346,
        "account": "customer2",
        "flowCount": -20.0,
        "typeDesc": "消耗",
        "createId": 98765,
        "createBy": "admin",
        "createTime": "2025-11-17 11:15:00"
      }
    ],
    "code": 200,
    "msg": "查询成功"
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

---

### 3.2 获取给我的流量充值记录

**GET** `/rechargeFlowListForMe`

**功能描述：** 获取给我的流量充值记录，支持分页查询和条件过滤

#### 请求参数

| 参数名    | 类型    | 必填 | 说明                       | 默认值 |
| --------- | ------- | ---- | -------------------------- | ------ |
| pageNum   | Integer | 否   | 页码                       | 1      |
| pageSize  | Integer | 否   | 每页条数                   | 10     |
| account   | String  | 否   | 充值账号                   | null   |
| startDate | String  | 否   | 开始日期，格式：yyyy-MM-dd | null   |
| endDate   | String  | 否   | 结束日期，格式：yyyy-MM-dd | null   |

#### 响应数据格式

响应数据格式与[获取客户充值历史列表](#31-获取我的流量充值记录)相同

#### 请求示例

```bash
GET http://user.ipweb.cc/prod-api/open/dynamic/rechargeFlowListForMe?pageNum=1&pageSize=10&type=0&account=admin@cc.cc&startDate=2025-04-20&endDate=2025-09-20
```

#### 响应示例

**成功响应：**

```json
{
  "code": 200,
  "msg": "查询成功",
  "data": {
    "total": 1,
    "rows": [
      {
        "customerId": 12347,
        "account": "customer3",
        "flowCount": 50.0,
        "typeDesc": "充值",
        "createId": 54321,
        "createBy": "myself",
        "createTime": "2025-11-17 09:15:00"
      }
    ],
    "code": 200,
    "msg": "查询成功"
  }
}
```

---

### 3.3 获取代理客户的流量使用记录

**GET** `/open/dynamic/flowReport`

**功能描述：** 获取代理客户的流量使用记录，支持分页查询和条件过滤

#### 请求参数

| 参数名     | 类型    | 必填 | 说明                                                          | 默认值 |
| ---------- | ------- | ---- | ------------------------------------------------------------- | ------ |
| pageNum    | Integer | 否   | 页码                                                          | 1      |
| pageSize   | Integer | 否   | 每页条数，默认值 100 最大值为 2000                            | 100    |
| customerId | Long    | 否   | 代理的 customerId，如果不传则获取代理账号下的所有客户使用记录 | null   |
| startDate  | String  | 否   | 开始日期，格式：yyyy-MM-dd                                    | null   |
| endDate    | String  | 否   | 结束日期，格式：yyyy-MM-dd                                    | null   |

#### 响应数据格式

| 字段名                   | 类型    | 说明                       |
| ------------------------ | ------- | -------------------------- |
| code                     | Integer | 响应状态码，200 表示成功   |
| msg                      | String  | 响应消息                   |
| data                     | Object  | 响应数据对象               |
| data.rows[].reportDate   | String  | 统计日期，格式：yyyy-MM-dd |
| data.rows[].customerId   | Integer | 客户 id                    |
| data.rows[].userName     | String  | 用户账号（登录名称）       |
| data.rows[].uploadFlow   | Double  | 上传流量（单位 MB）        |
| data.rows[].downloadFlow | Double  | 下载流量（单位 MB）        |

#### 请求示例

```bash
curl -X GET "http://user.ipweb.cc/prod-api/open/dynamic/flowReport?customerId=39&pageSize=10&pageNum=1&startDate=2021-12-26&endDate=2025-12-26" -H "token: your-access-token"
```

#### 响应示例

**成功响应：**

```json
{
  "msg": "操作成功",
  "code": 200,
  "data": {
    "total": 1049,
    "rows": [
      {
        "reportDate": "2025-12-25",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 596.9,
        "downloadFlow": 57500.89
      },
      {
        "reportDate": "2025-12-24",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 9.96,
        "downloadFlow": 971.02
      },
      {
        "reportDate": "2025-12-23",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 1386.96,
        "downloadFlow": 127666.61
      },
      {
        "reportDate": "2025-12-22",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 3154.19,
        "downloadFlow": 281663.12
      },
      {
        "reportDate": "2025-12-21",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 1773.4,
        "downloadFlow": 160514.67
      },
      {
        "reportDate": "2025-12-20",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 2271.29,
        "downloadFlow": 206284.81
      },
      {
        "reportDate": "2025-12-19",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 2007.01,
        "downloadFlow": 184863.78
      },
      {
        "reportDate": "2025-12-18",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 1351.99,
        "downloadFlow": 126838.57
      },
      {
        "reportDate": "2025-12-17",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 541.49,
        "downloadFlow": 49832.48
      },
      {
        "reportDate": "2025-12-16",
        "customerId": 39,
        "userName": "xxxx@gmaill.com",
        "uploadFlow": 1458.5,
        "downloadFlow": 137031.02
      }
    ],
    "code": 200,
    "msg": "查询成功",
    "other": null
  }
}
```

---

## 4. 响应格式说明

所有 API 接口都返回统一的 JSON 格式响应：

| 字段名 | 类型    | 说明                           |
| ------ | ------- | ------------------------------ |
| code   | Integer | 响应状态码，200 表示成功       |
| msg    | String  | 响应消息                       |
| data   | Object  | 响应数据，具体结构根据接口而定 |

---

## 5. 错误码说明

以下是动态流量相关 API 接口可能返回的错误码及其说明：

| 错误码 | 说明           | 解决方案                    |
| ------ | -------------- | --------------------------- |
| 200    | 无效的认证令牌 | 检查 Token 是否正确且未过期 |
| 200    | 账号信息不完整 | 联系管理员检查账户信息      |
| 500    | 服务器内部错误 | 联系技术支持                |

**错误响应示例：**

```json
{
  "code": 200,
  "msg": "无效的认证令牌",
  "data": null
}
```

---

© 2025 动态流量 API 接口文档 - 版本 1.0.0

最后更新时间：2025 年 12 月 26 日
