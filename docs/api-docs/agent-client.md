---
sidebar_position: 1
slug: /agent-client
---

# 代理客户

## 目录

- [代理客户](#代理客户)
  - [目录](#目录)
  - [1. 接口概述](#1-接口概述)
  - [2. 认证方式](#2-认证方式)
    - [请求头格式](#请求头格式)
    - [示例](#示例)
    - [Token 格式说明](#token-格式说明)
  - [3. API 接口列表](#3-api-接口列表)
    - [3.1 获取代理的客户记录](#31-获取代理的客户记录)
      - [请求参数](#请求参数)
      - [响应数据格式](#响应数据格式)
      - [请求示例](#请求示例)
      - [响应示例](#响应示例)
  - [4. 响应格式说明](#4-响应格式说明)
  - [5. 错误码说明](#5-错误码说明)

---

## 1. 接口概述

代理的客户记录 API 提供了查询为客户功能。

**基础 URL：**

- **HTTP：** `http://user.ipweb.cc/prod-api`
- **HTTPS：** `https://user.ipweb.cc/prod-api`

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
     -H "Token: your-access-token"

# 使用JavaScript fetch示例
fetch("http://user.ipweb.cc/prod-api/open/dynamic/myRechargeFlowList", {
  method: 'GET',
  headers: {
    'Token': 'your-access-token',
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

### 3.1 获取代理的客户记录

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

© 2025 代理客户查询 API 文档 - 版本 1.0.0

最后更新时间：2025 年 12 月 26 日
