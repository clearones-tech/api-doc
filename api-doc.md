# ClearOnes Open Api接口文档

| Version | Update Time | Status | Author | Description |
|---------|-------------|--------|--------|-------------|
|1.0.0|2023-04-20 12:00:00|create|clearones|创建文档|
|1.0.1|2023-05-29 10:00:00|modify|clearones|修改部分接口字段；类似功能接口返回值命名统一|
|1.0.2|2023-05-31 10:00:00|modify|clearones|增加webhook返回内容|
|1.0.3|2023-07-04 14:17:00|modify|clearones|修改法币转账手续费规则，创建转账接口新增fee参数；新增查询手续费接口；数字货币交易状态新增PROCESSING:处理中；|

## 接入说明
### 请求统一参数
| Parameter | Type   | Required | Description  | Since |
|-----------|--------|----------|--------------|-------|
|apiKey| string |false| apiKey       |-|
|timestamp| int64  |false| 时间戳毫秒        |-|
|bizContent| string |false| 请求业务参数加密后的内容 |-|
|key| string |false| 加密key        |-|
|sign| string |false| 签名           |-|
### API鉴权
ClearOnes 所有的 API 接口在请求时，采用了对称密钥加密和非对称公钥加密的混合加密方案，并且使用非对称私钥对请求参数进行签名。对称加密算法采用 AES-256 算法，非对称加密和签名算法采用 RSA-4096 算法，具体流程如下：

+ 将业务请求参数序列化为 JSON 字符串，**随机生成 32 字节 AES Key 和 16 字节初始化向量 IV**
+ 使用 AES Key 和 IV 对该 JSON 字符串进行加密，加密结果 base64 编码后作为请求参数中的 `bizContent` 字段
+ 使用**ClearOnes平台的API RSA公钥**对该 AES Key + IV 48 个字节加密，加密结果 base64 编码后作为请求参数中的 `key` 字段
+ 对所有请求参数按照字典 key 升序排序序列化为字符串，即序列化为 `apiKey=...&bizContent=...&key=...&timestamp=...` 格式
+ 使用**您的API RSA私钥**对请求序列化字符串进行签名，签名结果 base64 编码后作为请求参数中的 `sign` 字段

ClearOnes 会使用同样的流程对接口响应业务参数进行加密和签名，您可以使用以下流程进行解密和验签：

+ 对响应数据按照字典 key 升序排序序列化为字符串，即序列化为 `code=...&data=...&key=...&message=...&timestamp=...` 格式
+ 使用 ClearOnes API RSA 公钥，响应序列化字符串和响应中 `sign` 字段进行验签，如果验签通过，进行响应数据解密
+ 使用您的 API RSA 私钥解密响应数据中 `key` 字段，获取随机 AES Key + IV
+ 使用该 AES Key 和 IV 解密响应数据中 `data` 字段，获取响应业务数据明文
+ 您可以使用 OpenSSL 生成 RSA 私钥（`api_private.pem` 为您的 API RSA 私钥 ）：

```shell
openssl genpkey -out api_private.pem -algorithm RSA -pkeyopt rsa_keygen_bits:4096
```
使用 OpenSSL 生成 RSA 私钥对应的公钥（`api_public.pem` 为您的 API RSA 公钥）：
```shell
openssl rsa -in api_private.pem -out api_public.pem -pubout
```

注意：

+ 您只需要在 ClearOnes 控制台中设置您的 API RSA 公钥，请妥善保管您的 API RSA 私钥，避免泄漏带来安全隐患

### IP白名单
在调用 ClearOnes API 时，只允许从您设置的 IP 白名单地址发起请求，您需要在创建 API Key 时设置调用发起的 IP 地址。

## 账号模块
### 上传文件
**URL:** /api/v1/account/upload

**Type:** POST


**Content-Type:** multipart/form-data

**Description:** 把加签加密参数放在url后?apiKey=xxx&bizContent=参数加密哈希后的串&sign=xxx&key=xxx;支持的文件格式: jpg、jpeg、png、pdf、zip、rar、7z

**Query-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|file|file|false|No comments found.|-|
|apiKey|string|true|No comments found.|-|
|timestamp|string|true|No comments found.|-|
|bizContent|string|true|{"fileType":"ACCOUNT_CREATE/FIAT_PROOF"}|-|
|key|string|true|No comments found.|-|
|sign|string|true|No comments found.|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: multipart/form-data' -F 'file=' -i /api/v1/account/upload --data 'apiKey=wr1y24&timestamp=2023-07-04 14:18:12&bizContent=waob7s&key=rtodo9&sign=vvxxb3'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─objectKey|string|上传文件识别key|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "objectKey": "ybmxjh"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 为用户创建账户
**URL:** /api/v1/account/create

**Type:** POST


**Content-Type:** application/json

**Description:** 为用户创建账户

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|type|int32|true|账号类型 1-机构 2-个人|-|
|accountName|string|true|账户名称|-|
|firstName|string|true|用户名|-|
|lastName|string|true|用户姓|-|
|email|string|true|邮箱|-|
|country|string|true|国家/地区|-|
|customerRefId|string|true|调用方唯一业务id|-|
|materials|array|false|需要上传的材料列表，个人类型为必传，且材料数量符合要求|-|
|identificationNo|string|false|身份证明文件号，个人类型为必传|-|
|address|string|false|地址，个人类型为必传|-|
|birthday|string|false|生日，个人类型为必传|-|
|occupation|string|false|职业，个人类型为必传|-|
|gender|string|false|性别，个人类型为必传|-|
|contactNumber|string|false|电话，个人类型为必传|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/account/create --data '{
  "type": 358,
  "accountName": "lissette.mann",
  "firstName": "lissette.mann",
  "lastName": "lissette.mann",
  "email": "sasha.lind@hotmail.com",
  "country": "855tbw",
  "customerRefId": "115",
  "materials": [
    "4fste9"
  ],
  "identificationNo": "karrz7",
  "address": "Suite 321 397 Cummerata Creek， Welchview， HI 92380",
  "birthday": "2023-07-04",
  "occupation": "rwoflj",
  "gender": "1w6g7p",
  "contactNumber": "exr949"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─clientId|string|客户的账户标识|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "clientId": "1663027675055698121"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询用户账户详情（可获取审核中、已拒绝、已生效账户）
**URL:** /api/v1/account/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询用户账户详情（可获取审核中、已拒绝、已生效账户）

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|创建的账号Id(和customerRefId至少传一个)|-|
|customerRefId|string|false|调用方唯一业务id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/account/detail --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─clientId|string|客户的账户Id|-|
|└─name|string|账号名|-|
|└─email|string|创建时的邮箱地址|-|
|└─status|int32|状态 1-审核中 2-已生效 3-审核拒绝|-|
|└─note|string|审核拒绝的备注|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "clientId": "1663027675055698121",
    "name": "小蓝的账户",
    "email": "xiaolan@clearones.io",
    "status": 2,
    "note": "There is a risk to user funds"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询已经生效的账户列表
**URL:** /api/v1/account/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询已经生效的账户列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|fromClientId|string|false|从哪个clientId开始查询|-|
|limit|int32|false|每页限制 默认20 最大100|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/account/list --data '{
  "fromClientId": "1663027675055698121",
  "limit": 20
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─clientId|string|客户的账户Id|-|
|└─name|string|账号名|-|
|└─email|string|创建时的邮箱地址|-|
|└─status|int32|状态 1-审核中 2-已生效 3-审核拒绝|-|
|└─note|string|审核拒绝的备注|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
      "clientId": "1663027675055698121",
      "name": "小蓝的账户",
      "email": "xiaolan@clearones.io",
      "status": 2,
      "note": "There is a risk to user funds"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 加密货币币种
### 查询币种列表
**URL:** /api/v1/crypto/coin/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询币种列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|No comments found.|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/coin/list --data 'se714d'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─coinKey|string|币种唯一标识|-|
|└─coinName|string|币种名称|-|
|└─isNative|int8|是否原生币种（0：否；1：是）|-|
|└─blockChainKey|string|区块链唯一标识|-|
|└─contract|string|合约|-|
|└─coinDecimals|int8|币种精度|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "coinKey": "BTC",
      "coinName": "BTC",
      "isNative": 1,
      "blockChainKey": "ethereum",
      "contract": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
      "coinDecimals": 18
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询区块链列表
**URL:** /api/v1/crypto/blockchain/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询区块链列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|No comments found.|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/blockchain/list --data 'wmk5yo'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─blockchainKey|string|区块链唯一标识|-|
|└─blockchainName|string|区块链名称|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "blockchainKey": "ethereum",
      "blockchainName": "Ethereum"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 加密货币账号
### 创建账号
**URL:** /api/v1/crypto/account/create

**Type:** POST


**Content-Type:** application/json

**Description:** 创建账号

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id，最长 100|-|
|accountName|string|true|账号名称，最长20|-|
|blockchainKey|string|true|区块链唯一标识|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/account/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "accountName": "小红的ETH账户",
  "blockchainKey": "ethereum"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─cryptoAccountNo|string|账号编号|-|
|└─addressList|array|地址列表|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─address|string|地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressType|string|地址类型（DEFAULT：默认地址）|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "cryptoAccountNo": "11063639",
    "addressList": [
      {
        "address": "0x006619aa8a4DaF1323e9AA109e6A9E1b3DeF308b",
        "addressType": "DEFAULT"
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询账号列表
**URL:** /api/v1/crypto/account/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|客户的账户ID|-|
|fromNo|string|false|查询开始cryptoAccountNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/account/list --data '{
  "clientId": "1663027675055698121",
  "fromNo": "11063639",
  "limit": 20
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─clientId|string|客户的账户ID|-|
|└─cryptoAccountNo|string|账号编号|-|
|└─accountName|string|账号名称|-|
|└─blockchainKey|string|区块链唯一标识|-|
|└─addressList|array|地址列表|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─address|string|地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressType|string|地址类型（DEFAULT：默认地址）|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
      "clientId": "1663027675055698121",
      "cryptoAccountNo": "11063639",
      "accountName": "小红的ETH账户",
      "blockchainKey": "ethereum",
      "addressList": [
        {
          "address": "0x006619aa8a4DaF1323e9AA109e6A9E1b3DeF308b",
          "addressType": "DEFAULT"
        }
      ]
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询账号详情
**URL:** /api/v1/crypto/account/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|false|调用方唯一业务id，与参数cryptoAccountNo二选一必填，如果两个都有值，将按照两个参数查询|-|
|cryptoAccountNo|string|false|账号编号，与参数customerRefId二选一必填，如果两个都有值，将按照两个参数查询|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/account/detail --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "cryptoAccountNo": "11063639"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─clientId|string|客户的账户ID|-|
|└─cryptoAccountNo|string|账号编号|-|
|└─accountName|string|账号名称|-|
|└─blockchainKey|string|区块链唯一标识|-|
|└─addressList|array|地址列表|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─address|string|地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressType|string|地址类型（DEFAULT：默认地址）|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "clientId": "1663027675055698121",
    "cryptoAccountNo": "11063639",
    "accountName": "小红的ETH账户",
    "blockchainKey": "ethereum",
    "addressList": [
      {
        "address": "0x006619aa8a4DaF1323e9AA109e6A9E1b3DeF308b",
        "addressType": "DEFAULT"
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询账号下币种列表
**URL:** /api/v1/crypto/account/coin/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号下币种列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|cryptoAccountNo|string|true|账号编号|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/account/coin/list --data '{
  "clientId": "1663027675055698121",
  "cryptoAccountNo": "11063639"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─coinKey|string|币种唯一标识|-|
|└─coinName|string|币种名称|-|
|└─balance|string|余额|-|
|└─addressList|array|地址列表|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─address|string|地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressType|string|地址类型（DEFAULT：默认地址）|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "coinKey": "BTC",
      "coinName": "BTC",
      "balance": "101.2",
      "addressList": [
        {
          "address": "0x006619aa8a4DaF1323e9AA109e6A9E1b3DeF308b",
          "addressType": "DEFAULT"
        }
      ]
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询账号下币种详情
**URL:** /api/v1/crypto/account/coin/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号下币种详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|cryptoAccountNo|string|true|账号编号|-|
|coinKey|string|true|币种唯一标识|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/account/coin/detail --data '{
  "clientId": "1663027675055698121",
  "cryptoAccountNo": "11063639",
  "coinKey": "BTC"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─coinKey|string|币种唯一标识|-|
|└─coinName|string|币种名称|-|
|└─balance|string|余额|-|
|└─addressList|array|地址列表|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─address|string|地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressType|string|地址类型（DEFAULT：默认地址）|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "coinKey": "BTC",
    "coinName": "BTC",
    "balance": "101.2",
    "addressList": [
      {
        "address": "0x006619aa8a4DaF1323e9AA109e6A9E1b3DeF308b",
        "addressType": "DEFAULT"
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 加密货币交易
### 创建交易
**URL:** /api/v1/crypto/transaction/create

**Type:** POST


**Content-Type:** application/json

**Description:** 创建交易

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id，最长 100|-|
|cryptoAccountNo|string|true|账号编号|-|
|coinKey|string|true|币种唯一标识|-|
|amount|string|true|交易金额|-|
|feeRate|object|true|手续费费率|-|
|└─baseFee|string|false|EIP-1559 的 baseFee|-|
|└─fee|string|false|TRON 的预估手续费，只有波场有|-|
|└─feeRate|string|false|费率：UTXO 的 feePerByte; EVM 类的 gasPrice; 以及 TRON 的 feeLimit (对于 TRON 也可以不传)|-|
|└─gasLimit|int32|false|EVM 类的 gasLimit|-|
|└─maxFee|string|false|EIP-1559 的 maxFee|-|
|└─maxPriorityFee|string|false|EIP-1559 的 maxPriorityFee|-|
|└─bytesSize|int32|false|UTXO 字节数，不包含小于 1000 聪的 UTXO|-|
|toAddress|string|true|目标地址|-|
|note|string|false|备注，最长100|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/transaction/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "cryptoAccountNo": "11063639",
  "coinKey": "BTC",
  "amount": "1.2",
  "feeRate": {
    "baseFee": "0.000049522",
    "fee": "0.00001",
    "feeRate": "10",
    "gasLimit": 21000,
    "maxFee": "1.100055713",
    "maxPriorityFee": "1.1",
    "bytesSize": 53
  },
  "toAddress": "0x006619aa8a4DaF1323e9AA109e6A9E1b3DeF308b",
  "note": "差旅费"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─operateId|string|操作ID|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "operateId": "10"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 创建交易授权
**URL:** /api/v1/crypto/transaction/create/authorize

**Type:** POST


**Content-Type:** application/json

**Description:** 创建交易授权

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|operateId|string|true|操作id|-|
|verificationCode|string|true|验证码|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/transaction/create/authorize --data '{
  "clientId": "1663027675055698121",
  "operateId": "10",
  "verificationCode": "123456"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─transactionNo|string|交易号|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "transactionNo": "1663027675055698130"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易列表
**URL:** /api/v1/crypto/transaction/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|客户的账户ID|-|
|fromNo|string|false|查询开始transactionNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|
|cryptoAccountNo|string|false|账号编号|-|
|coinKey|string|false|币种唯一标识|-|
|createTimestampFrom|int64|false|创建时间开始时间戳，UNIX 时间戳毫秒数|-|
|createTimestampTo|int64|false|创建时间结束时间戳，UNIX 时间戳毫秒数|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/transaction/list --data '{
  "clientId": "1663027675055698121",
  "fromNo": "1663027675055698130",
  "limit": 20,
  "cryptoAccountNo": "@mock 11063639",
  "coinKey": "BTC",
  "createTimestampFrom": 1672056033898,
  "createTimestampTo": 1672056033898
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|└─transactionNo|string|交易号|-|
|└─clientId|string|客户的账户ID|-|
|└─cryptoAccountNo|string|账号编号|-|
|└─coinKey|string|币种唯一标识|-|
|└─transactionType|int8|交易类型（1:接收；2:发送；）|-|
|└─txAmount|string|交易金额|-|
|└─transactionStatus|string|交易状态（SUBMITTED:审批中；PROCESSING:处理中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；）|-|
|└─transactionSubStatus|string|交易子状态|-|
|└─note|string|备注|-|
|└─blockHeight|int64|区块高度|-|
|└─fromAddress|string|交易来源地址|-|
|└─toAddress|string|交易目标地址|-|
|└─txHash|string|交易hash|-|
|└─txFee|string|交易手续费|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
      "createTimestamp": 1672056033898,
      "transactionNo": "1663027675055698130",
      "clientId": "1663027675055698121",
      "cryptoAccountNo": "11063639",
      "coinKey": "BTC",
      "transactionType": 1,
      "txAmount": "1.2",
      "transactionStatus": "SUCCESS",
      "transactionSubStatus": "ETH_BLOCK_CHAIN_INSUFFICIENT_FUNDS",
      "note": "差旅费",
      "blockHeight": 8371443,
      "fromAddress": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
      "toAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93",
      "txHash": "0xf9eb33e7edae658035075e7b3dab09d15d7eb2ee20a4e84e0984a3f9d55ccc40",
      "txFee": "1.2"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易详情
**URL:** /api/v1/crypto/transaction/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|false|调用方唯一业务id，与参数transactionNo二选一必填，如果两个都有值，将按照两个参数查询|-|
|transactionNo|string|false|交易号，与参数customerRefId二选一必填，如果两个都有值，将按照两个参数查询|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/crypto/transaction/detail --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "transactionNo": "1663027675055698130"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|└─transactionNo|string|交易号|-|
|└─clientId|string|客户的账户ID|-|
|└─cryptoAccountNo|string|账号编号|-|
|└─coinKey|string|币种唯一标识|-|
|└─transactionType|int8|交易类型（1:接收；2:发送；）|-|
|└─txAmount|string|交易金额|-|
|└─transactionStatus|string|交易状态（SUBMITTED:审批中；PROCESSING:处理中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；）|-|
|└─transactionSubStatus|string|交易子状态|-|
|└─note|string|备注|-|
|└─blockHeight|int64|区块高度|-|
|└─fromAddress|string|交易来源地址|-|
|└─toAddress|string|交易目标地址|-|
|└─txHash|string|交易hash|-|
|└─txFee|string|交易手续费|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "createTimestamp": 1672056033898,
    "transactionNo": "1663027675055698130",
    "clientId": "1663027675055698121",
    "cryptoAccountNo": "11063639",
    "coinKey": "BTC",
    "transactionType": 1,
    "txAmount": "1.2",
    "transactionStatus": "SUCCESS",
    "transactionSubStatus": "ETH_BLOCK_CHAIN_INSUFFICIENT_FUNDS",
    "note": "差旅费",
    "blockHeight": 8371443,
    "fromAddress": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
    "toAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93",
    "txHash": "0xf9eb33e7edae658035075e7b3dab09d15d7eb2ee20a4e84e0984a3f9d55ccc40",
    "txFee": "1.2"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 法币模块
### 国家
**URL:** /api/v1/fiat/country/list

**Type:** POST


**Content-Type:** application/json

**Description:** 国家

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|原始参数使用空串|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/country/list --data 're00uw'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─nameCn|string|简体中文名称|-|
|└─nameEn|string|英文名称|-|
|└─isoCountryCode|string|IOS国家代码|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "nameCn": "中国香港",
      "nameEn": "Hong Kong",
      "isoCountryCode": "HK"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 获取所有支持的币种
**URL:** /api/v1/fiat/currency/list

**Type:** POST


**Content-Type:** application/json

**Description:** 获取所有支持的币种

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|原始参数使用空串|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/currency/list --data 'xz7mhr'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─currencyKey|string|币种唯一标识|-|
|└─currencyDecimal|int32|币种精度|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "currencyKey": "USD",
      "currencyDecimal": 2
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询银行账户列表
**URL:** /api/v1/fiat/account/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询银行账户列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/list --data '{
  "clientId": "1663027675055698121"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─fiatAccountNo|string|客户的法币账户号|-|
|└─name|string|账号名|-|
|└─balances|array|账户余额列表|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─currencyKey|string|币种|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─availableBalance|number|可用余额|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─availableBalanceUsd|number|可用余额转usd金额|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─frozenBalance|number|冻结余额|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─frozenBalanceUsd|number|冻结余额转usd金额|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─fee|number|转账服务费|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─depositAddress|object|存款转账地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankName|string|银行名称|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankAddress|string|银行地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankCode|string|银行代码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─branchCode|string|分行代码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─swiftCode|string|SWIFT|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankCountry|string|银行国家|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─beneficiaryAccountNo|string|银行账号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─beneficiaryName|string|收款人姓名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─note|string|转入需要的附言|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "fiatAccountNo": "11063639",
      "name": "小蓝的账户",
      "balances": [
        {
          "currencyKey": "USD",
          "availableBalance": 1000300.12,
          "availableBalanceUsd": 1000300.12,
          "frozenBalance": 300.12,
          "frozenBalanceUsd": 300.12,
          "fee": 300,
          "depositAddress": {
            "bankName": "China CITIC Bank International Limited",
            "bankAddress": "61-65 Des Voeux Road Central. Hong Kong",
            "bankCode": "012",
            "branchCode": "123",
            "swiftCode": "KWHKHKHH",
            "bankCountry": "中国香港",
            "beneficiaryAccountNo": "123123456789",
            "beneficiaryName": "CLEARONES TRUST LIMITED - CLIENT'S A/C",
            "note": "11063639"
          }
        }
      ]
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询银行账户下某个币种的余额
**URL:** /api/v1/fiat/account/balance

**Type:** POST


**Content-Type:** application/json

**Description:** 查询银行账户下某个币种的余额

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|currencyKey|string|true|币种唯一标识|-|
|fiatAccountNo|string|true|法币账号No|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/balance --data '{
  "clientId": "1663027675055698121",
  "currencyKey": "USD",
  "fiatAccountNo": "11063639"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─currencyKey|string|币种|-|
|└─availableBalance|number|可用余额|-|
|└─availableBalanceUsd|number|可用余额转usd金额|-|
|└─frozenBalance|number|冻结余额|-|
|└─frozenBalanceUsd|number|冻结余额转usd金额|-|
|└─fee|number|转账服务费|-|
|└─depositAddress|object|存款转账地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankName|string|银行名称|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankAddress|string|银行地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankCode|string|银行代码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─branchCode|string|分行代码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─swiftCode|string|SWIFT|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─bankCountry|string|银行国家|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─beneficiaryAccountNo|string|银行账号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─beneficiaryName|string|收款人姓名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─note|string|转入需要的附言|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "currencyKey": "USD",
    "availableBalance": 1000300.12,
    "availableBalanceUsd": 1000300.12,
    "frozenBalance": 300.12,
    "frozenBalanceUsd": 300.12,
    "fee": 300,
    "depositAddress": {
      "bankName": "China CITIC Bank International Limited",
      "bankAddress": "61-65 Des Voeux Road Central. Hong Kong",
      "bankCode": "012",
      "branchCode": "123",
      "swiftCode": "KWHKHKHH",
      "bankCountry": "中国香港",
      "beneficiaryAccountNo": "123123456789",
      "beneficiaryName": "CLEARONES TRUST LIMITED - CLIENT'S A/C",
      "note": "11063639"
    }
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 创建收款方请求
**URL:** /api/v1/fiat/account/recipient/create

**Type:** POST


**Content-Type:** application/json

**Description:** 创建收款方请求

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id|-|
|fiatAccountNo|string|true|账号No|-|
|currencyKey|string|true|币种key|-|
|bankCountryCode|string|true|开户银行所在地iso code|-|
|bankCodeType|string|true|收款银行代号类型（Only 'SWIFT' be supported）|-|
|bankCode|string|true|收款银行代号|-|
|bankName|string|true|收款银行名称|-|
|beneficiaryAccountNo|string|true|收款人银行账户号码/IBAN|-|
|beneficiaryName|string|true|收款人姓名|-|
|beneficiaryCountryCode|string|true|收款人国家iso code|-|
|beneficiaryAddress|string|true|收款人地址|-|
|note|string|false|备注|-|
|label|string|false|标签别称|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/recipient/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "fiatAccountNo": "11063639",
  "currencyKey": "USD",
  "bankCountryCode": "HK",
  "bankCodeType": "SWIFT",
  "bankCode": "012",
  "bankName": "China CITIC Bank International Limited",
  "beneficiaryAccountNo": "123123456789",
  "beneficiaryName": "小红",
  "beneficiaryCountryCode": "HK",
  "beneficiaryAddress": "8 Finance Street, Central, Hong Kong",
  "note": "9v6beq",
  "label": "Blue Account"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─operateId|string|操作ID|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "operateId": "10"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 提交创建收款方操作码
**URL:** /api/v1/fiat/account/recipient/create/authorize

**Type:** POST


**Content-Type:** application/json

**Description:** 提交创建收款方操作码

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|operateId|string|true|操作记录id|-|
|verificationCode|string|true|邮件下发的验证码|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/recipient/create/authorize --data '{
  "clientId": "1663027675055698121",
  "operateId": "10",
  "verificationCode": "123456"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─recipientId|string|收款方地址id|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "recipientId": "11"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 删除收款方
**URL:** /api/v1/fiat/account/recipient/delete

**Type:** POST


**Content-Type:** application/json

**Description:** 删除收款方

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|fiatAccountNo|string|true|法币账号No|-|
|recipientId|string|true|收款地址id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/recipient/delete --data '{
  "clientId": "1663027675055698121",
  "fiatAccountNo": "11063639",
  "recipientId": "11"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─result|boolean|执行结果（true：成功；false：失败；）|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "result": true
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 获取收款地址
**URL:** /api/v1/fiat/account/recipient/list

**Type:** POST


**Content-Type:** application/json

**Description:** 获取收款地址

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|fiatAccountNo|string|true|法币账号No|-|
|status|int32|false|收款方状态(1:审批中；2:已生效；3:审批拒绝；)|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/recipient/list --data '{
  "clientId": "1663027675055698121",
  "fiatAccountNo": "11063639",
  "status": 2
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─recipientId|string|收款方地址id|-|
|└─fiatAccountNo|string|账号No|-|
|└─currencyKey|string|币种key|-|
|└─bankCountryCode|string|开户银行所在地iso code|-|
|└─bankCodeType|string|收款银行代号类型（Only 'SWIFT' be supported）|-|
|└─bankCode|string|收款银行代号|-|
|└─bankName|string|收款银行名称|-|
|└─beneficiaryAccountNo|string|收款人银行账户号码/IBAN|-|
|└─beneficiaryName|string|收款人姓名|-|
|└─beneficiaryCountryCode|string|收款人国家iso code|-|
|└─beneficiaryAddress|string|收款人地址|-|
|└─note|string|备注|-|
|└─label|string|标签别称|-|
|└─status|int32|收款方状态(1:审批中；2:已生效；3:审批拒绝；)|-|
|└─approvalNote|string|审批备注|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
      "recipientId": "11",
      "fiatAccountNo": "11063639",
      "currencyKey": "USD",
      "bankCountryCode": "HK",
      "bankCodeType": "SWIFT",
      "bankCode": "012",
      "bankName": "China CITIC Bank International Limited",
      "beneficiaryAccountNo": "123123456789",
      "beneficiaryName": "小红",
      "beneficiaryCountryCode": "HK",
      "beneficiaryAddress": "8 Finance Street, Central, Hong Kong",
      "note": "de9j9p",
      "label": "小蓝的账户",
      "status": 2,
      "approvalNote": "同意"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 获取收款地址详情
**URL:** /api/v1/fiat/account/recipient/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 获取收款地址详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|fiatAccountNo|string|true|法币账号No|-|
|recipientId|string|false|收款人id(recipientId和customerRefId至少传一个)|-|
|customerRefId|string|false|调用方唯一业务id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/recipient/detail --data '{
  "clientId": "1663027675055698121",
  "fiatAccountNo": "11063639",
  "recipientId": "24221",
  "customerRefId": "53d73bed-0a15-4ad6-95f6-9e73381ec17d"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─recipientId|string|收款方地址id|-|
|└─fiatAccountNo|string|账号No|-|
|└─currencyKey|string|币种key|-|
|└─bankCountryCode|string|开户银行所在地iso code|-|
|└─bankCodeType|string|收款银行代号类型（Only 'SWIFT' be supported）|-|
|└─bankCode|string|收款银行代号|-|
|└─bankName|string|收款银行名称|-|
|└─beneficiaryAccountNo|string|收款人银行账户号码/IBAN|-|
|└─beneficiaryName|string|收款人姓名|-|
|└─beneficiaryCountryCode|string|收款人国家iso code|-|
|└─beneficiaryAddress|string|收款人地址|-|
|└─note|string|备注|-|
|└─label|string|标签别称|-|
|└─status|int32|收款方状态(1:审批中；2:已生效；3:审批拒绝；)|-|
|└─approvalNote|string|审批备注|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "recipientId": "11",
    "fiatAccountNo": "11063639",
    "currencyKey": "USD",
    "bankCountryCode": "HK",
    "bankCodeType": "SWIFT",
    "bankCode": "012",
    "bankName": "China CITIC Bank International Limited",
    "beneficiaryAccountNo": "123123456789",
    "beneficiaryName": "小红",
    "beneficiaryCountryCode": "HK",
    "beneficiaryAddress": "8 Finance Street, Central, Hong Kong",
    "note": "448g3c",
    "label": "小蓝的账户",
    "status": 2,
    "approvalNote": "同意"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 发起转出申请
**URL:** /api/v1/fiat/account/transaction/create

**Type:** POST


**Content-Type:** application/json

**Description:** 发起转出申请

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id|-|
|fiatAccountNo|string|true|钱包账号编号|-|
|transferCurrencyKey|string|true|转账币种唯一标识|-|
|transferAmount|string|true|转账金额|-|
|fee|string|true|服务费|-|
|recipientId|string|true|收款方ID|-|
|note|string|false|备注|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/transaction/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "fiatAccountNo": "11063639",
  "transferCurrencyKey": "USD",
  "transferAmount": "100",
  "fee": "10",
  "recipientId": "11",
  "note": "ixwl98"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─operateId|string|操作ID|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "operateId": "10"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 提交创建转出交易操作码
**URL:** /api/v1/fiat/account/transaction/create/authorize

**Type:** POST


**Content-Type:** application/json

**Description:** 提交创建转出交易操作码

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|operateId|string|true|操作记录id|-|
|verificationCode|string|true|邮件下发的验证码|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/transaction/create/authorize --data '{
  "clientId": "1663027675055698121",
  "operateId": "10",
  "verificationCode": "123456"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─transactionNo|string|交易号|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "transactionNo": "1663027675055698131"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易详情
**URL:** /api/v1/fiat/account/transaction/info

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|fiatAccountNo|string|true|法币账号No|-|
|transactionNo|string|false|交易记录号(和customerRefId 至少传一个)|-|
|customerRefId|string|false|调用方唯一业务id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/transaction/info --data '{
  "clientId": "1663027675055698121",
  "fiatAccountNo": "11063639",
  "transactionNo": "1663027675055698131",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─transactionNo|string|交易号|-|
|└─transactionType|int8|交易类型（1：接收；2：发送；5：退款；）|-|
|└─fiatAccountNo|string|钱包账号编号|-|
|└─payAccountName|string|付款方名称|-|
|└─beneficiaryName|string|收款方名称|-|
|└─transferCurrencyKey|string|币种名称|-|
|└─transactionAmount|string|交易数量|-|
|└─fee|string|交易手续费|-|
|└─transactionStatus|string|交易状态（SUBMITTED:审批中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF: 已上传凭证；SUCCESS:成功；REJECTED:拒绝；）|-|
|└─proofEn|string|需要上传凭证的英文说明|-|
|└─proofCn|string|需要上传凭证的中文说明|-|
|└─transactionSubStatus|string|交易子状态（失败的状态码）|-|
|└─createTimestamp|int64|创建时间戳毫秒|-|
|└─completedTimestamp|int64|完成时间戳毫秒|-|
|└─note|string|备注|-|
|└─approvalNote|string|审批备注|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
    "transactionNo": "1663027675055698130",
    "transactionType": 1,
    "fiatAccountNo": "11063639",
    "payAccountName": "Sam's Wallet",
    "beneficiaryName": "Jack's Wallet",
    "transferCurrencyKey": "USD",
    "transactionAmount": "1.23456789",
    "fee": "200",
    "transactionStatus": "SUBMITTED",
    "proofEn": "uzl0gq",
    "proofCn": "don5k1",
    "transactionSubStatus": "neuysi",
    "createTimestamp": 1672062254051,
    "completedTimestamp": 1672062254051,
    "note": "awu9mo",
    "approvalNote": "同意"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易列表
**URL:** /api/v1/fiat/account/transaction/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|fromNo|string|false|从哪个交易号开始|-|
|currencyKey|string|false|币种|-|
|transactionStatus|string|false|交易状态（SUBMITTED:审批中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF: 已上传凭证；SUCCESS:成功；REJECTED:拒绝；）|-|
|transactionType|int8|false|交易类型（1：接收；2：发送；5：退款；）|-|
|createTimeFrom|int64|false|交易创建时间从什么时候开始 UNIX 时间戳毫秒数|-|
|createTimeTo|int64|false|交易创建时间到什么时候结束 UNIX 时间戳毫秒数|-|
|completedTimeFrom|int64|false|交易完成时间从什么时候开始 UNIX 时间戳毫秒数|-|
|completedTimeTo|int64|false|交易完成时间到什么时候结束 UNIX 时间戳毫秒数|-|
|limit|int32|false|每次查询多少条 默认20 最大100|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/transaction/list --data '{
  "clientId": "1663027675055698121",
  "fromNo": "1663027675055698131",
  "currencyKey": "USD",
  "transactionStatus": "SUCCESS",
  "transactionType": 1,
  "createTimeFrom": 1672056033898,
  "createTimeTo": 1672056033898,
  "completedTimeFrom": 1672056033898,
  "completedTimeTo": 1672056033898,
  "limit": 20
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─transactionNo|string|交易号|-|
|└─transactionType|int8|交易类型（1：接收；2：发送；5：退款；）|-|
|└─fiatAccountNo|string|钱包账号编号|-|
|└─payAccountName|string|付款方名称|-|
|└─beneficiaryName|string|收款方名称|-|
|└─transferCurrencyKey|string|币种名称|-|
|└─transactionAmount|string|交易数量|-|
|└─fee|string|交易手续费|-|
|└─transactionStatus|string|交易状态（SUBMITTED:审批中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF: 已上传凭证；SUCCESS:成功；REJECTED:拒绝；）|-|
|└─proofEn|string|需要上传凭证的英文说明|-|
|└─proofCn|string|需要上传凭证的中文说明|-|
|└─transactionSubStatus|string|交易子状态（失败的状态码）|-|
|└─createTimestamp|int64|创建时间戳毫秒|-|
|└─completedTimestamp|int64|完成时间戳毫秒|-|
|└─note|string|备注|-|
|└─approvalNote|string|审批备注|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": [
    {
      "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
      "transactionNo": "1663027675055698130",
      "transactionType": 1,
      "fiatAccountNo": "11063639",
      "payAccountName": "Sam's Wallet",
      "beneficiaryName": "Jack's Wallet",
      "transferCurrencyKey": "USD",
      "transactionAmount": "1.23456789",
      "fee": "200",
      "transactionStatus": "SUBMITTED",
      "proofEn": "030gg3",
      "proofCn": "u8j94u",
      "transactionSubStatus": "wrffeg",
      "createTimestamp": 1672062254051,
      "completedTimestamp": 1672062254051,
      "note": "xiomyx",
      "approvalNote": "同意"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 入账交易上传交易凭证
**URL:** /api/v1/fiat/account/transaction/proof/add

**Type:** POST


**Content-Type:** application/json

**Description:** 入账交易上传交易凭证

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|transactionNo|string|true|需要上传凭证的交易号|-|
|objectKey|string|true|上传后的对象key|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/transaction/proof/add --data '{
  "clientId": "1663027675055698121",
  "transactionNo": "1663027675055698131",
  "objectKey": "5pa40v"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─result|boolean|执行结果（true：成功；false：失败；）|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "result": true
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易手续费
**URL:** /api/v1/fiat/account/transaction/fee

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易手续费

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|fiatAccountNo|string|true|钱包账号编号|-|
|transferCurrencyKey|string|true|转账币种唯一标识|-|
|transferAmount|string|true|转账金额|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v1/fiat/account/transaction/fee --data '{
  "clientId": "1663027675055698121",
  "fiatAccountNo": "11063639",
  "transferCurrencyKey": "USD",
  "transferAmount": "100"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─fee|string|手续费|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "fee": "10"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## Webhook
### 概述
通过Webhook主动通知您账户下发生的一些事件，比如
+ 创建用户账户
+ 加密货币交易状态变更
+ 银行账户收款地址创建状态变更
+ 银行交易状态变更
+ ...

Webhook 回调时采用与 API鉴权 中相同的数据加密和签名方案，需要您在 ClearOnes 控制台配置您的 Webhook RSA 公钥。您可以参考 API 鉴权章节生成 RSA 公私钥对，在 ClearOnes 控制台可获取 ClearOnes Webhook RSA 公钥，您可以使用该公钥对回调数据进行验签。

### 回调请求描述
| Parameter | Type   | Description                               |
|-----------|--------|-------------------------------------------|
|timestamp| int64  | 时间戳毫秒                                     |
|bizContent| string | 业务请求参数 AES 加密后数据                          |
|key| string | 使用 ClearOnes Webhook RSA 私钥对请求参数签名得到的签名数据 |
|sign| string | 使用您的 Webhook RSA 公钥对随机 AES Key 加密后的数据     |

您可以使用以下流程对回调数据进行解密和验签：

+ 对回调数据按照字典 key 升序排序序列化为字符串，即序列化为 `bizContent=...&key=...&timestamp=...` 格式
+ 使用 ClearOnes Webhook RSA 公钥，序列化字符串和回调数据中 `sign` 字段进行验签，如果验签通过，进行响应数据解密
+ 使用您的 Webhook RSA 私钥解密回调数据中 `key` 字段，获取随机 AES Key + IV
+ 使用该 AES Key 和 IV 解密回调数据中 `bizContent` 字段，获取回调业务数据明文

您在收到 Webhook 推送通知后，需要返回成功（HTTP状态码 200 ）应答，成功响应内容格式固定为为：

| Parameter | Type   | Description   |
  |-----------|--------|---------------|
| code      | int    | 成功固定返回200     |
| message   | string | 成功固定返回Success |

ClearOnes 在收到非200成功状态码以及响应内容非以上成功格式时，会认为此次推送失败，会再次进行重试推送，再次推送的频率为 30s，1m，5m，1h，12h，24h
### 事件格式

| Parameter   | Type   | Description |
|-------------|--------|-------------|
| eventType   | string | 回调事件类型      |
| eventDetail | object | 回调事件详情      |

### 事件类型
| eventType                | eventDetail                                         | Description  |
|--------------------------|-----------------------------------------------------|--------------|
| CLIENT_STATUS_CHANGED    | [clientDetail](#clientDetail)                       | 创建用户状态变更     |
| CRYPTO_TX_CREATED        | [cryptoTransactionDetail](#cryptoTransactionDetail) | 数字货币交易创建     |
| CRYPTO_TX_STATUS_CHANGED | [cryptoTransactionDetail](#cryptoTransactionDetail) | 数字货币交易状态变更   |
| RECIPIENT_STATUS_CHANGED | [recipientDetail](#recipientDetail)                 | 法币收款人信息变更    |
| FIAT_TX_CREATED          | [fiatTransactionDetail](#fiatTransactionDetail)     | 法币创建交易       |
| FIAT_TX_STATUS_CHANGED   | [fiatTransactionDetail](#fiatTransactionDetail)     | 法币交易状态变更     |

### 事件详情
**<div id="clientDetail"> clientDetail </div>**

| Parameter      | Type   | Description           |
|----------------|--------|-----------------------|
| customerRefId  | string | 调用方唯一业务id             |
| clientId       | string | 客户的账户Id               |
| name           | string | 账号名                   |
| email          | string | 创建时的邮箱地址              |
| status         | int32  | 状态 1-审核中 2-已生效 3-审核拒绝 |

**<div id="cryptoTransactionDetail">cryptoTransactionDetail</div>**

| Parameter            | Type   | Description                                                                                                    |
|----------------------|--------|----------------------------------------------------------------------------------------------------------------|
| customerRefId        | string | 调用方唯一业务id                                                                                                      |
| createTimestamp      | int64  | 创建时间，UNIX 时间戳毫秒数                                                                                               |
| transactionNo        | string | 交易号                                                                                                            |
| clientId             | string | 客户的账户ID                                                                                                        |
| cryptoAccountNo      | string | 账号编号                                                                                                           |
| coinKey              | string | 币种唯一标识                                                                                                         |
| transactionType      | int8   | 交易类型（1:接收；2:发送；）                                                                                               |
| txAmount             | string | 交易金额                                                                                                           |
| transactionStatus    | string | 交易状态（SUBMITTED:审批中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；） |
| transactionSubStatus | string | 交易子状态                                                                                                          |
| note                 | string | 备注                                                                                                             |
| blockHeight          | int64  | 区块高度                                                                                                           |
| fromAddress          | string | 交易来源地址                                                                                                         |
| toAddress            | string | 交易目标地址                                                                                                         |
| txHash               | string | 交易hash                                                                                                         |
| txFee                | string | 交易手续费                                                                                                          |

**<div id="recipientDetail">recipientDetail</div>**

| Parameter              | Type   | Description                         |
|------------------------|--------|-------------------------------------|
| customerRefId          | string | 调用方唯一业务id                           |
| recipientId            | string | 收款方地址id                             |
| fiatAccountNo          | string | 账号No                                |
| currencyKey            | string | 币种key                               |
| bankCountryCode        | string | 开户银行所在地iso code                     |
| bankCodeType           | string | 收款银行代号类型（Only 'SWIFT' be supported） |
| bankCode               | string | 收款银行代号                              |
| bankName               | string | 收款银行名称                              |
| beneficiaryAccountNo   | string | 收款人银行账户号码/IBAN                      |
| beneficiaryName        | string | 收款人姓名                               |
| beneficiaryCountryCode | string | 收款人国家iso code                       |
| beneficiaryAddress     | string | 收款人地址                               |
| note                   | string | 备注                                  |
| label                  | string | 标签别称                                |
| status                 | int32  | 收款方状态(1:审批中；2:已生效；3:审批拒绝；)          |
| approvalNote           | string | 审批备注                                |

**<div id="fiatTransactionDetail">fiatTransactionDetail</div>**

| Parameter            | Type    | Description                                                                               |
|----------------------|---------|-------------------------------------------------------------------------------------------|
| customerRefId        | string  | 调用方唯一业务id                                                                                 |
| transactionNo        | string  | 交易号                                                                                       |
| transactionType      | int8    | 交易类型（1：接收；2：发送；5：退款；）                                                                     |
| fiatAccountNo        | string  | 钱包账号编号                                                                                    |
| payAccountName       | string  | 付款方名称                                                                                     |
| beneficiaryName      | string  | 收款方名称                                                                                     |
| transferCurrencyKey  | string  | 币种名称                                                                                      |
| transactionAmount    | string  | 交易数量                                                                                      |
| fee                  | string  | 交易手续费                                                                                     |
| transactionStatus    | string  | 交易状态（SUBMITTED:审批中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF: 已上传凭证；SUCCESS:成功；REJECTED:拒绝；） |
| proofEn              | string  | 需要上传凭证的英文说明                                                                               |
| proofCn              | string  | 需要上传凭证的中文说明                                                                               |
| transactionSubStatus | string  | 交易子状态（失败的状态码）                                                                             |
| createTimestamp      | int64   | 创建时间戳毫秒                                                                                   |
| completedTimestamp   | int64   | 完成时间戳毫秒                                                                                   |
| note                 | string  | 备注                                                                                        |
| approvalNote         | string  | 审批备注                                                                                      |

## 错误码列表

| Error code | Description |
|------------|-------------|
|302|Repeat operation|
|400|Request parameter format error|
|401|Insufficient permissions|
|412|Remote call parameter error|
|413|Remote call business exception|
|429|To many requests|
|500|System error|
|501|Remote call time out|
|1001|Sign Verification failed|
|1002|Parameter decrypt failed|
|1003|Not in the ip whitelist|
|403|未授权|
|10105|接口幂等|
|10203|账号已冻结|
|10204|审批人数量大于钱包管理员数量|
|10205|账号不支持的币种|
|10206|存在进行中的审批|
|10207|冻结时间必须是将来时间|
|10208|账号不存在|
|10401|用户已存在|
|10402|用户不存在|
|10403|不能删除审批中的用户|
|10404|不能删除自己|
|10405|当前钱包存在审批，无法删除钱包管理员|
|10406|在删除管理员之前需要先减少审批数|
|10501|钱包提币白名单标签不能超过20个字符|
|10502|钱包提币白名单地址不能超过200个字符|
|10503|白名单地址已存在|
|10504|不能删除审批中的白名单|
|10601|提币规则已存在|
|10701|无效的地址|
|10702|您的收款地址有风险，不能转账。|
|10703|目标地址不能为合约地址|
|10704|余额不足|
|10705|手续费余额不足|
|10706|提币数量小数精度错误|
|10707|提币数量超过组织限额|
|10708|提币数量超过钱包限额|
|10709|暂停充币|
|10710|暂停提币|
|10711|钱包被冻结，无法提币|
|10712|更新GA后，24小时内禁止提币|
|10713|更新密码后，24小时内禁止提币|
|10714|该机构已失效，无法提币|
|10801|该机构已失效，无法转账|
|10802|暂停充值|
|10803|暂停转账|
|10804|钱包被冻结，无法转账|
|10805|更新GA后，24小时内禁止转账|
|10806|更新密码后，24小时内禁止转账|
|10807|转账数量小数精度错误|
|10808|单笔转账最低数量检查不通过|
|10812|手续费变更，请重新提交|
|10901|操作授权验证失败|
|11001|交易记录不存在|
|11002|当前交易状态不允许操作|

