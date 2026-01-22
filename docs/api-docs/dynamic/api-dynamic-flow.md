---
sidebar_position: 2
slug: /api-dynamic-flow
description: 动态流量 API：查询充值与消耗记录、代理客户流量报表，包含认证方式与示例。
---

# 动态流量

---

## 1. 获取我的流量充值记录

**GET** `/open/dynamic/myRechargeFlowList`

**功能描述：** 获取我的流量充值记录，支持分页查询和条件过滤

### 请求参数

| 参数名    | 类型    | 必填 | 说明                       | 默认值 |
| --------- | ------- | ---- | -------------------------- | ------ |
| pageNum   | Integer | 否   | 页码                       | 1      |
| pageSize  | Integer | 否   | 每页条数                   | 10     |
| type      | Integer | 否   | 类型：0 充值 1 消耗        | 0      |
| account   | String  | 否   | 被充值账号                 | null   |
| startDate | String  | 否   | 开始日期，格式：yyyy-MM-dd | null   |
| endDate   | String  | 否   | 结束日期，格式：yyyy-MM-dd | null   |

### 响应数据格式

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

### 请求示例

```bash
GET http://user.ipweb.cc/prod-api/open/dynamic/myRechargeFlowList?pageNum=1&pageSize=10&type=0&account=test@cc.cc&startDate=2025-04-20&endDate=2025-09-20
```

### 响应示例

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

## 2. 获取给我的流量充值记录

**GET** `/open/dynamic/rechargeFlowListForMe`

**功能描述：** 获取给我的流量充值记录，支持分页查询和条件过滤

### 请求参数

| 参数名    | 类型    | 必填 | 说明                       | 默认值 |
| --------- | ------- | ---- | -------------------------- | ------ |
| pageNum   | Integer | 否   | 页码                       | 1      |
| pageSize  | Integer | 否   | 每页条数                   | 10     |
| account   | String  | 否   | 充值账号                   | null   |
| startDate | String  | 否   | 开始日期，格式：yyyy-MM-dd | null   |
| endDate   | String  | 否   | 结束日期，格式：yyyy-MM-dd | null   |

### 响应数据格式

响应数据格式与[获取客户充值历史列表](#31-获取我的流量充值记录)相同

### 请求示例

```bash
GET http://user.ipweb.cc/prod-api/open/dynamic/rechargeFlowListForMe?pageNum=1&pageSize=10&type=0&account=admin@cc.cc&startDate=2025-04-20&endDate=2025-09-20
```

### 响应示例

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

## 3. 获取代理客户的流量使用记录

**GET** `/open/dynamic/flowReport`

**功能描述：** 获取代理客户的流量使用记录，支持分页查询和条件过滤

### 请求参数

| 参数名     | 类型    | 必填 | 说明                                                          | 默认值 |
| ---------- | ------- | ---- | ------------------------------------------------------------- | ------ |
| pageNum    | Integer | 否   | 页码                                                          | 1      |
| pageSize   | Integer | 否   | 每页条数，默认值 100 最大值为 2000                            | 100    |
| customerId | Long    | 否   | 代理的 customerId，如果不传则获取代理账号下的所有客户使用记录 | null   |
| startDate  | String  | 否   | 开始日期，格式：yyyy-MM-dd                                    | null   |
| endDate    | String  | 否   | 结束日期，格式：yyyy-MM-dd                                    | null   |

### 响应数据格式

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

### 请求示例

```bash
curl -X GET "http://user.ipweb.cc/prod-api/open/dynamic/flowReport?customerId=39&pageSize=10&pageNum=1&startDate=2021-12-26&endDate=2025-12-26" -H "token: your-access-token"
```

### 响应示例

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


© 2025 动态流量 API 接口文档 - 版本 1.0.0

最后更新时间：2025 年 12 月 26 日
