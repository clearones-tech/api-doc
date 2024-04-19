# ClearOnes Open Api v2 接口文档

| Version | Update Time         | Status | Author | Description                                                                                                           |
|---------|---------------------|--------|--------|-----------------------------------------------------------------------------------------------------------------------|
| 2.0.0   | 2023-03-14 12:00:00 |create|clearones| 创建文档                                                                                                                  |
| 2.0.1   | 2024-03-28 12:59:00 |modify|clearones| 授权验证接口新增类型5:连接账号提币；连接账号模块新增接口：查询账号列表、查询账号详情、查询账号币种列表、查询账号币种详情、预估交易手续费、创建交易；连接账号模块查询交易详情接口参数新增：调用方唯一业务ID(customerRefId) |
| 2.0.2   | 2024-04-18 10:59:00 |modify|clearones| 法币预估手续费接口增加必填字段recipientId                                                                                            |

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

### 名词解释
#### 转账通道(channelKey/subChannelKey)分为:

+ swift: swift国际电汇转账方式
+ local: 银行本地支持的转账方式,channelKey为local时,subChannelKey可分为以下子通道
  + chats: 香港的特快转账(RTGS/CHATS)
  + fps: 香港转数快
  + ach: 美国的本地转账
+ conet: Clearones的内部转账
+ crypto: 加密货币链上转账方式

## 用户账号模块
### 为用户创建账户
**URL:** /api/v2/client/create

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
|businessRegistrationNumber|string|false|机构的商业注册编号，机构用户为必填|-|
|materials|array|false|需要上传的材料列表，个人类型为必传，且材料数量符合要求|-|
|identificationNo|string|false|身份证明文件号，个人类型为必传|-|
|address|string|false|地址，个人类型为必传|-|
|birthday|string|false|生日，个人类型为必传|-|
|occupation|string|false|职业，个人类型为必传|-|
|gender|string|false|性别，个人类型为必传|-|
|contactNumber|string|false|电话，个人类型为必传|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/client/create --data '{
  "type": 1,
  "accountName": "Clearones Limit LTD.",
  "firstName": "SAN",
  "lastName": "ZHANG",
  "email": "zhangsan@gmail.com",
  "country": "HK",
  "customerRefId": "1dfeaf-dfoadav",
  "businessRegistrationNumber": "BS100023",
  "materials": [
    "o2tiw0"
  ],
  "identificationNo": "t1ie8g",
  "address": "Apt. 548 977 Gleason Spurs， Port Leatrice， GA 60663",
  "birthday": "2024-03-15",
  "occupation": "manager",
  "gender": "male",
  "contactNumber": "100-19231923"
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

### 查询用户账户详情
**URL:** /api/v2/client/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询用户账户详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|创建的账号Id(和customerRefId至少传一个)|-|
|customerRefId|string|false|调用方唯一业务id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/client/detail --data '{
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

### 查询用户账户列表
**URL:** /api/v2/client/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询用户账户列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|fromClientId|string|false|从哪个clientId开始查询|-|
|limit|int32|false|每页限制 默认20 最大100|-|
|status|int32|false|状态 1-审核中 2-已生效 3-审核拒绝|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/client/list --data '{
  "fromClientId": "1663027675055698121",
  "limit": 20,
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

## 国家
### 查询国家列表
**URL:** /api/v2/country/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询国家列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|原始参数使用空串|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/country/list --data 'f5jyk5'
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
      "nameCn": "中国大陆",
      "nameEn": "China",
      "isoCountryCode": "CN"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 币种
### 查询币种列表
**URL:** /api/v2/currency/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询币种列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|原始参数使用空串|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/currency/list --data 'dkh7t5'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─currencyKey|string|币种唯一标识|-|
|└─currencyName|string|币种名称|-|
|└─currencyCategory|int32|币种类别(1:数字货币;2:法币;)|-|
|└─currencyDecimals|int32|币种精度|-|
|└─withdrawalSwitch|int32|提币开关（0:关闭；1:开启；）|-|
|└─cryptoCurrencyType|string|加密币种类型（NATIVE：原生币种；ERC20：ETH上的代币标准；TRC20：TRON上的代币标准；BEP20：BSC上的代币标准；）|-|
|└─cryptoCurrencyContract|string|加密币种合约|-|
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
      "currencyKey": "USDT_ERC20",
      "currencyName": "USDT",
      "currencyCategory": 1,
      "currencyDecimals": 6,
      "withdrawalSwitch": 1,
      "cryptoCurrencyType": "ERC20",
      "cryptoCurrencyContract": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 文件上传
### 上传文件
**URL:** /api/v2/file/upload

**Type:** POST


**Content-Type:** multipart/form-data

**Description:** 把加签加密参数放在url后?apiKey=xxx&bizContent=参数加密哈希后的串&sign=xxx&key=xxx;支持的文件格式: jpg、jpeg、png、pdf、zip、rar、7z

**Request-headers:**

| Header | Type | Required | Description | Since |
|--------|------|----------|-------------|-------|
|x-user-ip|string|true|No comments found.|-|


**Query-parameters:**

| Parameter  | Type   | Required | Description                                                                       | Since |
|------------|--------|----------|-----------------------------------------------------------------------------------|-------|
| file       | file   | true     | 文件,最大支持10M                                                                        | -     |
| apiKey     | string | true     | apiKey                                                                            | -     |
| timestamp  | string | true     | 时间戳                                                                               | -     |
| bizContent | string | true     | 格式:{"fileType":"ACCOUNT_CREATE/FIAT_PROOF"} ACCOUNT_CREATE:创建用户;FIAT_PROOF:上传交易凭证 | -     |
| key        | string | true     | 加密key                                                                             | -     |
| sign       | string | true     | 签名                                                                                | -     |

**Request-example:**
```
curl -X POST -H 'Content-Type: multipart/form-data' -F 'file=' -i /api/v2/file/upload --data 'apiKey=6o2hdx&timestamp=2024-03-14 17:58:37&bizContent=conkhb&key=w5et16&sign=i8dqwf'
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
    "objectKey": "clearones-fiat-test/ACCOUNT_CREATE/8ecb6d8fa60e47c8a919f8913850a975.png"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 资金账号模块
### 查询账号币种列表,资金账户合并,该接口可同时查询法币和加密货币账户余额
**URL:** /api/v2/fund/account/currency/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号币种列表,资金账户合并,该接口可同时查询法币和加密货币账户余额

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|currencyCategory|int32|false|币种类别(1：数字货币；2：法币；)|-|
|currencyKey|string|false|币种唯一标识|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fund/account/currency/list --data '{
  "clientId": "1663027675055698121",
  "currencyCategory": 1,
  "currencyKey": "USDT_ERC20"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─currencyKey|string|币种唯一标识|-|
|└─currencyCategory|int32|币种类别(1：数字货币；2：法币；)|-|
|└─availableBalance|string|可用余额|-|
|└─frozenBalance|string|冻结余额|-|
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
      "currencyKey": "USDT_ERC20",
      "currencyCategory": 1,
      "availableBalance": "1000.2",
      "frozenBalance": "500.5"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询用户单个币种的收款地址
**URL:** /api/v2/fund/account/currency/deposit

**Type:** POST


**Content-Type:** application/json

**Description:** 查询用户单个币种的收款地址

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|currencyKey|string|true|资金币种|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fund/account/currency/deposit --data '{
  "clientId": "1663027675055698121",
  "currencyKey": "USD"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─conetId|string|用于内部转账的收款账号id|-|
|└─depositAddressKey|string|收款方式唯一标识|-|
|└─currencyCategory|int32|币种分类 1-数字货币 2-法币|-|
|└─currencyKey|string|币种标识|-|
|└─currencyName|string|币种名|-|
|└─channelKey|string|币种-转账通道 crypto,swift,local,conet|-|
|└─subChannelKey|string|法币-转账子通道,当channelKey=local时有值 ach,chats,fps|-|
|└─bankAccountType|int32|法币-银行账号类型 1-CA 2-VA|-|
|└─bankName|string|法币-银行名称|-|
|└─bankAddress|string|法币-银行地址|-|
|└─bankCode|string|法币-银行代码|-|
|└─branchCode|string|法币-分行代码|-|
|└─swiftCode|string|法币-SWIFT|-|
|└─bankCountry|string|法币-银行国家|-|
|└─beneficiaryAccountNo|string|法币-银行账号|-|
|└─beneficiaryName|string|法币-收款人姓名|-|
|└─beneficiaryCountry|string|法币-收款人国家|-|
|└─beneficiaryAddress|string|法币-收款人地址|-|
|└─note|string|法币-转入需要的附言|-|
|└─cryptoAddress|string|加密货币-地址|-|
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
      "conetId": "2101012",
      "depositAddressKey": "189123123104123",
      "currencyCategory": 2,
      "currencyKey": "USD",
      "currencyName": "USD",
      "channelKey": "local",
      "subChannelKey": "chats",
      "bankAccountType": 1,
      "bankName": "China CITIC Bank International Limited",
      "bankAddress": "61-65 Des Voeux Road Central. Hong Kong",
      "bankCode": "012",
      "branchCode": "123",
      "swiftCode": "KWHKHKHH",
      "bankCountry": "Hong Kong",
      "beneficiaryAccountNo": "123123456789",
      "beneficiaryName": "CLEARONES TRUST LIMITED - CLIENT'S A/C",
      "beneficiaryCountry": "Hong Kong",
      "beneficiaryAddress": "61-65 Des Voeux Road Central. Hong Kong",
      "note": "11063639",
      "cryptoAddress": "0x1231231"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 通过conetId查询用户名
**URL:** /api/v2/fund/account/queryConetId

**Type:** POST


**Content-Type:** application/json

**Description:** 通过conetId查询用户名

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|conetId|string|true|conetId|-|
|currencyKey|string|true|币种标识|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fund/account/queryConetId --data '{
  "clientId": "1663027675055698121",
  "conetId": "88271834",
  "currencyKey": "ETH"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─conetId|string|查询的conetId|-|
|└─clientName|string|客户的账户名|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "conetId": "88271834",
    "clientName": "Quan****LTD"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 收款人管理模块
### 查询法币创建收款方可支持的通道
**URL:** /api/v2/recipient/fiat/supportCreateChannel

**Type:** POST


**Content-Type:** application/json

**Description:** 查询法币创建收款方可支持的通道

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|currencyKey|string|true|币种标识|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/fiat/supportCreateChannel --data '{
  "clientId": "1663027675055698121",
  "currencyKey": "USD"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─channelKey|string|法币-转账通道 swift,local,conet|-|
|└─subChannelKey|string|法币-local转账子通道 ach,chats,fps|-|
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
      "channelKey": "local",
      "subChannelKey": "chats"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 法币创建收款方请求
**URL:** /api/v2/recipient/fiat/create

**Type:** POST


**Content-Type:** application/json

**Description:** 法币创建收款方请求

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id|-|
|channelKey|string|true|法币-转账通道 swift,local,conet|-|
|subChannelKey|string|false|法币-local转账子通道 ach,chats,fps,channelKey=local时必传|-|
|currencyKey|string|true|币种标识|-|
|conetId|string|false|平台内部的收款账号id|-|
|swiftCode|string|false|银行swift码|-|
|bankCode|string|false|收款银行code local-fps类型为必填|-|
|bankName|string|false|收款银行名称|-|
|bankCountryCode|string|false|收款银行国家ISO code|-|
|bankAddress|string|false|收款银行地址|-|
|beneficiaryRoutingCode|string|false|ABA/ACH的路由号 local-ach类型为必填|-|
|beneficiaryAccountNo|string|false|收款人银行账户号码/IBAN|-|
|beneficiaryName|string|false|收款人姓名|-|
|beneficiaryCountryCode|string|false|收款人国家ISO code|-|
|beneficiaryStreet|string|false|收款人街道|-|
|beneficiaryCity|string|false|收款人城市|-|
|beneficiaryState|string|false|收款人州/省|-|
|beneficiaryPostalCode|string|false|收款人邮编|-|
|note|string|false|备注|-|
|label|string|false|标签别称|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/fiat/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "channelKey": "swift",
  "subChannelKey": "chats",
  "currencyKey": "USD",
  "conetId": "1009131",
  "swiftCode": "HSBCHKHHHKH",
  "bankCode": "012",
  "bankName": "China CITIC Bank International Limited",
  "bankCountryCode": "HK",
  "bankAddress": "8 Finance Street, Central, Hong Kong",
  "beneficiaryRoutingCode": "123123456",
  "beneficiaryAccountNo": "123123456789",
  "beneficiaryName": "XIAO HONG",
  "beneficiaryCountryCode": "HK",
  "beneficiaryStreet": "8 Finance Street",
  "beneficiaryCity": "Central",
  "beneficiaryState": "Hong Kong",
  "beneficiaryPostalCode": "999077",
  "note": "xtgmqx",
  "label": "zhangsan"
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
    "operateId": "101"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 法币获取收款地址列表
**URL:** /api/v2/recipient/fiat/list

**Type:** POST


**Content-Type:** application/json

**Description:** 法币获取收款地址列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|currencyKey|string|false|资金币种|-|
|status|int32|false|收款方状态(1:审批中；2:已生效；3:审批拒绝；)|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/fiat/list --data '{
  "clientId": "1663027675055698121",
  "currencyKey": "USD",
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
|└─channelKey|string|法币-转账通道 crypto,swift,local,conet|-|
|└─subChannelKey|string|法币-转账子通道 ach,chats,fps|-|
|└─status|int32|收款人状态(1:审批中；2:已生效；3:审批拒绝)|-|
|└─currencyKey|string|币种标识|-|
|└─swiftCode|string|收款银行swift码|-|
|└─bankCode|string|收款银行代号|-|
|└─bankName|string|收款银行名称|-|
|└─bankCountryCode|string|开户银行所在地iso code|-|
|└─beneficiaryRoutingCode|string|ABA/ACH的路由号|-|
|└─beneficiaryAccountNo|string|收款人银行账户号码/IBAN|-|
|└─beneficiaryName|string|收款人姓名|-|
|└─beneficiaryCountryCode|string|收款人国家iso code|-|
|└─beneficiaryStreet|string|收款人街道|-|
|└─beneficiaryCity|string|收款人城市|-|
|└─beneficiaryState|string|收款人州/省|-|
|└─beneficiaryPostalCode|string|收款人邮编|-|
|└─conetId|int64|conet收款方式对方conetId|-|
|└─note|string|备注|-|
|└─label|string|别称|-|
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
      "channelKey": "local",
      "subChannelKey": "chats",
      "status": 2,
      "currencyKey": "USD",
      "swiftCode": "HSBCHKHHHKH",
      "bankCode": "012",
      "bankName": "China CITIC Bank International Limited",
      "bankCountryCode": "HK",
      "beneficiaryRoutingCode": "123",
      "beneficiaryAccountNo": "123123456789",
      "beneficiaryName": "XIAO HONG",
      "beneficiaryCountryCode": "HK",
      "beneficiaryStreet": "8 Finance Street",
      "beneficiaryCity": "Central",
      "beneficiaryState": "Central",
      "beneficiaryPostalCode": "999077",
      "conetId": 1009213,
      "note": "it89z8",
      "label": "9dtzqj"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 法币获取收款地址详情
**URL:** /api/v2/recipient/fiat/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 法币获取收款地址详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|recipientId|string|false|收款人id(recipientId和customerRefId至少传一个)|-|
|customerRefId|string|false|调用方唯一业务id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/fiat/detail --data '{
  "clientId": "1663027675055698121",
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
|└─channelKey|string|法币-转账通道 crypto,swift,local,conet|-|
|└─subChannelKey|string|法币-转账子通道 ach,chats,fps|-|
|└─status|int32|收款人状态(1:审批中；2:已生效；3:审批拒绝)|-|
|└─currencyKey|string|币种标识|-|
|└─swiftCode|string|收款银行swift码|-|
|└─bankCode|string|收款银行代号|-|
|└─bankName|string|收款银行名称|-|
|└─bankCountryCode|string|开户银行所在地iso code|-|
|└─beneficiaryRoutingCode|string|ABA/ACH的路由号|-|
|└─beneficiaryAccountNo|string|收款人银行账户号码/IBAN|-|
|└─beneficiaryName|string|收款人姓名|-|
|└─beneficiaryCountryCode|string|收款人国家iso code|-|
|└─beneficiaryStreet|string|收款人街道|-|
|└─beneficiaryCity|string|收款人城市|-|
|└─beneficiaryState|string|收款人州/省|-|
|└─beneficiaryPostalCode|string|收款人邮编|-|
|└─conetId|int64|conet收款方式对方conetId|-|
|└─note|string|备注|-|
|└─label|string|别称|-|
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
    "channelKey": "local",
    "subChannelKey": "chats",
    "status": 2,
    "currencyKey": "USD",
    "swiftCode": "HSBCHKHHHKH",
    "bankCode": "012",
    "bankName": "China CITIC Bank International Limited",
    "bankCountryCode": "HK",
    "beneficiaryRoutingCode": "123",
    "beneficiaryAccountNo": "123123456789",
    "beneficiaryName": "XIAO HONG",
    "beneficiaryCountryCode": "HK",
    "beneficiaryStreet": "8 Finance Street",
    "beneficiaryCity": "Central",
    "beneficiaryState": "Central",
    "beneficiaryPostalCode": "999077",
    "conetId": 1009213,
    "note": "vdw7km",
    "label": "hn0em6"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 法币删除收款方
**URL:** /api/v2/recipient/fiat/delete

**Type:** POST


**Content-Type:** application/json

**Description:** 法币删除收款方

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|recipientId|string|true|收款地址id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/fiat/delete --data '{
  "clientId": "1663027675055698121",
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

### 加密货币创建收款方请求
**URL:** /api/v2/recipient/crypto/create

**Type:** POST


**Content-Type:** application/json

**Description:** 加密货币创建收款方请求

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id|-|
|currencyKey|string|true|币种标识|-|
|label|string|false|标签别称|-|
|address|string|true|加密货币地址|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/crypto/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "currencyKey": "ETH",
  "label": "zhangsan",
  "address": "0x2B2711eADBb960f99221BF795EDFdc036798822D"
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
    "operateId": "101"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 加密货币获取收款地址列表
**URL:** /api/v2/recipient/crypto/list

**Type:** POST


**Content-Type:** application/json

**Description:** 加密货币获取收款地址列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|currencyKey|string|false|资金币种|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/crypto/list --data '{
  "clientId": "1663027675055698121",
  "currencyKey": "USDT_TRC20"
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
|└─currencyKey|string|币种标识|-|
|└─address|string|加密货币地址|-|
|└─label|string|别称|-|
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
      "currencyKey": "USD",
      "address": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
      "label": "小红的地址"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 加密货币获取收款地址详情
**URL:** /api/v2/recipient/crypto/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 加密货币获取收款地址详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|recipientId|string|false|收款人id(recipientId和customerRefId至少传一个)|-|
|customerRefId|string|false|调用方唯一业务id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/crypto/detail --data '{
  "clientId": "1663027675055698121",
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
|└─currencyKey|string|币种标识|-|
|└─address|string|加密货币地址|-|
|└─label|string|别称|-|
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
    "currencyKey": "USD",
    "address": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
    "label": "小红的地址"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 加密货币删除收款方
**URL:** /api/v2/recipient/crypto/delete

**Type:** POST


**Content-Type:** application/json

**Description:** 加密货币删除收款方

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|recipientId|string|true|收款地址id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/crypto/delete --data '{
  "clientId": "1663027675055698121",
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

## 交易
### 创建交易加密交易
**URL:** /api/v2/transaction/crypto/create

**Type:** POST


**Content-Type:** application/json

**Description:** 创建交易加密交易

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id，最长 100|-|
|currencyKey|string|true|币种唯一标识|-|
|amount|string|true|交易金额|-|
|recipientId|string|true|收款方ID|-|
|note|string|false|备注，最长100|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/crypto/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "currencyKey": "BTC",
  "amount": "1.2",
  "recipientId": "11",
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
    "operateId": "101"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 创建一个法币转账
**URL:** /api/v2/transaction/fiat/create

**Type:** POST


**Content-Type:** application/json

**Description:** 创建一个法币转账

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id|-|
|transferCurrencyKey|string|true|转账币种唯一标识|-|
|transferAmount|string|true|转账金额|-|
|recipientId|string|true|收款方ID|-|
|feeMethod|int32|false|手续费方式（1:支付本地银行服务费；2:支付本地银行服务费与收款行服务费；）<br/>收款人channelKey为swift/local时为必传|-|
|paymentReasonType|int64|true|付款原因类型<br/> 1:同名账户汇款/Same-name Account Payment<br/> 2:业务支出/Business Expenses<br/> 3:派息或分红/Dividend or Distribution<br/> 4:捐赠/Donate<br/> 100:其他/Other|-|
|paymentReason|string|false|付款原因，当paymentReasonType为其他时，必填|-|
|contractList|array|false|交易合同文件列表|-|
|└─objectKey|string|true|上传后的对象key|-|
|note|string|false|备注|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/fiat/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "transferCurrencyKey": "USD",
  "transferAmount": "100",
  "recipientId": "11",
  "feeMethod": 1,
  "paymentReasonType": 2,
  "paymentReason": "利润结算",
  "contractList": [
    {
      "objectKey": "my0sat"
    }
  ],
  "note": "3zanso"
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
    "operateId": "101"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易列表
**URL:** /api/v2/transaction/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|客户的账户ID|-|
|fromNo|string|false|查询开始transactionNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|
|currencyKey|string|false|币种唯一标识|-|
|currencyCategory|int32|false|币种类别(1:数字货币;2:法币;)|-|
|createTimestampFrom|int64|false|创建时间开始时间戳，UNIX 时间戳毫秒数|-|
|createTimestampTo|int64|false|创建时间结束时间戳，UNIX 时间戳毫秒数|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/list --data '{
  "clientId": "1663027675055698121",
  "fromNo": "1663027675055698130",
  "limit": 20,
  "currencyKey": "BTC",
  "currencyCategory": 1,
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
|└─customerRefId|string|调用方唯一业务ID|-|
|└─transactionNo|string|交易号|-|
|└─clientId|string|客户的账户ID|-|
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|└─completedTimestamp|int64|完成时间，UNIX 时间戳毫秒数|-|
|└─transferCurrencyKey|string|转账币种唯一标识|-|
|└─currencyCategory|int32|币种类别(1:数字货币;2:法币;)|-|
|└─transactionType|int32|交易类型（1：接收；2：发送；5：退款；）|-|
|└─payAccountName|string|付款方名称|-|
|└─beneficiaryName|string|收款方名称|-|
|└─transactionAmount|string|交易数量|-|
|└─transactionStatus|string|交易状态（SUBMITTED:审批中；PROCESSING:处理中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF:已上传凭证；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；）|-|
|└─transactionSubStatus|string|交易子状态|-|
|└─platformFee|string|平台手续费|-|
|└─note|string|备注|-|
|└─beneficiaryId|int64|收款人ID|-|
|└─fiatFeeMethod|int32|法币手续费方式（1：支付本地银行服务费；2：支付本地银行服务费与收款行服务费；）|-|
|└─channelKey|string|转账通道（crypto,swift,local,conet）|-|
|└─subChannelKey|string|转账子通道（fps；chats；ach），当转账方式为“local”时，需要指定子类型|-|
|└─proofEn|string|需要上传凭证的英文说明|-|
|└─proofCn|string|需要上传凭证的中文说明|-|
|└─cryptoBlockHeight|int64|数字货币区块高度|-|
|└─cryptoFromAddress|string|数字货币交易来源地址|-|
|└─cryptoToAddress|string|数字货币交易目标地址|-|
|└─cryptoTxHash|string|数字货币交易hash|-|
|└─cryptoTxFee|string|数字货币链上手续费|-|
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
      "clientId": "1663027675055698121",
      "createTimestamp": 1672056033898,
      "completedTimestamp": 1672062254051,
      "transferCurrencyKey": "BTC",
      "currencyCategory": 1,
      "transactionType": 1,
      "payAccountName": "Sam's Wallet",
      "beneficiaryName": "Jack's Wallet",
      "transactionAmount": "1.23456789",
      "transactionStatus": "SUCCESS",
      "transactionSubStatus": "t1wn5c",
      "platformFee": "1.2",
      "note": "差旅费",
      "beneficiaryId": 123,
      "fiatFeeMethod": 1,
      "channelKey": "swift",
      "subChannelKey": "chats",
      "proofEn": "5b7wpm",
      "proofCn": "30l8ou",
      "cryptoBlockHeight": 8371443,
      "cryptoFromAddress": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
      "cryptoToAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93",
      "cryptoTxHash": "0xf9eb33e7edae658035075e7b3dab09d15d7eb2ee20a4e84e0984a3f9d55ccc40",
      "cryptoTxFee": "1.2"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易详情
**URL:** /api/v2/transaction/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|false|调用方唯一业务ID，与参数transactionNo二选一必填，如果两个都有值，将按照两个参数查询|-|
|transactionNo|string|false|交易号，与参数customerRefId二选一必填，如果两个都有值，将按照两个参数查询|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/detail --data '{
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
|└─customerRefId|string|调用方唯一业务ID|-|
|└─transactionNo|string|交易号|-|
|└─clientId|string|客户的账户ID|-|
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|└─completedTimestamp|int64|完成时间，UNIX 时间戳毫秒数|-|
|└─transferCurrencyKey|string|转账币种唯一标识|-|
|└─currencyCategory|int32|币种类别(1:数字货币;2:法币;)|-|
|└─transactionType|int32|交易类型（1：接收；2：发送；5：退款；）|-|
|└─payAccountName|string|付款方名称|-|
|└─beneficiaryName|string|收款方名称|-|
|└─transactionAmount|string|交易数量|-|
|└─transactionStatus|string|交易状态（SUBMITTED:审批中；PROCESSING:处理中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF:已上传凭证；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；）|-|
|└─transactionSubStatus|string|交易子状态|-|
|└─platformFee|string|平台手续费|-|
|└─note|string|备注|-|
|└─beneficiaryId|int64|收款人ID|-|
|└─fiatFeeMethod|int32|法币手续费方式（1：支付本地银行服务费；2：支付本地银行服务费与收款行服务费；）|-|
|└─channelKey|string|转账通道（crypto,swift,local,conet）|-|
|└─subChannelKey|string|转账子通道（fps；chats；ach），当转账方式为“local”时，需要指定子类型|-|
|└─proofEn|string|需要上传凭证的英文说明|-|
|└─proofCn|string|需要上传凭证的中文说明|-|
|└─cryptoBlockHeight|int64|数字货币区块高度|-|
|└─cryptoFromAddress|string|数字货币交易来源地址|-|
|└─cryptoToAddress|string|数字货币交易目标地址|-|
|└─cryptoTxHash|string|数字货币交易hash|-|
|└─cryptoTxFee|string|数字货币链上手续费|-|
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
    "clientId": "1663027675055698121",
    "createTimestamp": 1672056033898,
    "completedTimestamp": 1672062254051,
    "transferCurrencyKey": "BTC",
    "currencyCategory": 1,
    "transactionType": 1,
    "payAccountName": "Sam's Wallet",
    "beneficiaryName": "Jack's Wallet",
    "transactionAmount": "1.23456789",
    "transactionStatus": "SUCCESS",
    "transactionSubStatus": "to62q9",
    "platformFee": "1.2",
    "note": "差旅费",
    "beneficiaryId": 123,
    "fiatFeeMethod": 1,
    "channelKey": "swift",
    "subChannelKey": "chats",
    "proofEn": "jai56f",
    "proofCn": "5tccjn",
    "cryptoBlockHeight": 8371443,
    "cryptoFromAddress": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
    "cryptoToAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93",
    "cryptoTxHash": "0xf9eb33e7edae658035075e7b3dab09d15d7eb2ee20a4e84e0984a3f9d55ccc40",
    "cryptoTxFee": "1.2"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询法币交易手续费
**URL:** /api/v2/transaction/fiat/fee

**Type:** POST


**Content-Type:** application/json

**Description:** 查询法币交易手续费

**Body-parameters:**

| Parameter           | Type   | Required | Description                            | Since |
|---------------------|--------|----------|----------------------------------------|-------|
| clientId            | string | true     | 客户的账户ID                                | -     |
| transferCurrencyKey | string | true     | 转账币种唯一标识                               | -     |
| recipientId         | string | true     | 收款方ID                                  | -     |
| transferAmount      | string | true     | 转账金额                                   | -     |
| feeMethod           | int32  | true     | 手续费方式（1:支付本地银行服务费；2:支付本地银行服务费与收款行服务费；） | -     |
| channelKey          | string | false    | 法币-转账通道 swift,local                    | -     |

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/fiat/fee --data '{
  "clientId": "1663027675055698121",
  "transferCurrencyKey": "USD",
  "transferAmount": "100",
  "feeMethod": 1,
  "channelKey": "swift"
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

### 查询加密货币交易预估手续费
**URL:** /api/v2/transaction/crypto/estimated/fee

**Type:** POST


**Content-Type:** application/json

**Description:** 查询加密货币交易预估手续费

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|currencyKey|string|true|币种唯一标识|-|
|amount|string|true|交易金额|-|
|recipientId|string|true|收款方ID|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/crypto/estimated/fee --data '{
  "clientId": "1663027675055698121",
  "currencyKey": "BTC",
  "amount": "1.2",
  "recipientId": "11"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─feeCurrencyKey|string|手续费币种标识|-|
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
    "feeCurrencyKey": "ETH",
    "fee": "0.0009"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 添加交易凭证
**URL:** /api/v2/transaction/proof/add

**Type:** POST


**Content-Type:** application/json

**Description:** 添加交易凭证

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|transactionNo|string|true|交易号|-|
|objectKey|string|true|上传后的对象key|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/proof/add --data '{
  "clientId": "1663027675055698121",
  "transactionNo": "1663027675055698131",
  "objectKey": "uo6pds"
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

## 授权
### 授权验证
**URL:** /api/v2/authorization/verify

**Type:** POST


**Content-Type:** application/json

**Description:** 对于创建交易和收款人的操作,Clearones会下发邮件验证码给用户,用户收到验证码后需要填写验证码才能继续后续的业务操作;
授权验证接口就是在调用写操作接口(1:加密货币提币；2:法币转账；3:加密货币添加收款地址；4:法币添加收款人；5:连接账号提币；)后续需要验证该操作并真正执行上述接口操作的接口

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|authorizationType|int32|true|授权类型（1:加密货币提币；2:法币转账；3:加密货币添加收款地址；4:法币添加收款人；5:连接账号提币；）|-|
|operateId|string|true|操作记录ID|-|
|verificationCode|string|true|验证码|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/authorization/verify --data '{
  "clientId": "1663027675055698121",
  "authorizationType": 1,
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
|└─customerRefId|string|调用方唯一业务ID|-|
|└─resultId|string|操作业务返回的ID，例如返回交易号、收款人ID等|-|
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
    "resultId": "1663027675055698130"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 账单
### 查询合并账单列表
**URL:** /api/v2/bill/merged/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询合并账单列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|fromNo|string|false|查询开始billNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|
|billNo|string|false|账单编号|-|
|billCategory|int32|false|账单类别（1：开户费；2：托管费；）|-|
|createTimestampFrom|int64|false|创建时间开始时间戳，UNIX 时间戳毫秒数|-|
|createTimestampTo|int64|false|创建时间结束时间戳，UNIX 时间戳毫秒数|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/bill/merged/list --data '{
  "fromNo": "20240307701775180101",
  "limit": 20,
  "billNo": "20240307701775180100",
  "billCategory": 1,
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
|└─billNo|string|账单编号|-|
|└─billCategory|int32|账单类别（1：开户费；2：托管费；）|-|
|└─currencyKey|string|账单币种标识|-|
|└─amount|string|账单金额|-|
|└─payCurrencyKey|string|支付账单的币种标识|-|
|└─payAmount|string|支付金额|-|
|└─payRate|string|支付汇率|-|
|└─payTransactionNo|string|关联的支付交易号|-|
|└─billStatus|int32|账单状态（1：待付款；2：审核中；3：付款中；4：已付款；）|-|
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
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
      "billNo": "20240307701775180101",
      "billCategory": 1,
      "currencyKey": "USD",
      "amount": "100",
      "payCurrencyKey": "USD",
      "payAmount": "100",
      "payRate": "1",
      "payTransactionNo": "1763024953373831168",
      "billStatus": 1,
      "createTimestamp": 1672056033898
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询子账单列表
**URL:** /api/v2/bill/sub/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询子账单列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|fromNo|string|false|查询开始billNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|
|billNo|string|false|账单编号|-|
|mergedBillNo|string|false|聚合账单编号|-|
|billCategory|int32|false|账单类别（1：开户费；2：托管费；）|-|
|createTimestampFrom|int64|false|创建时间开始时间戳，UNIX 时间戳毫秒数|-|
|createTimestampTo|int64|false|创建时间结束时间戳，UNIX 时间戳毫秒数|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/bill/sub/list --data '{
  "fromNo": "20240307701775180101",
  "limit": 20,
  "billNo": "20240307701775180101",
  "mergedBillNo": "20240307701775180100",
  "billCategory": 1,
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
|└─clientId|string|客户的账户ID|-|
|└─billNo|string|账单编号|-|
|└─mergedBillNo|string|主账单编号|-|
|└─billCategory|int32|账单类别（1：开户费；2：托管费；）|-|
|└─currencyKey|string|账单币种标识|-|
|└─amount|string|账单金额|-|
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
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
      "clientId": "1663027675055698121",
      "billNo": "20240307701775180101",
      "mergedBillNo": "20240307701775180100",
      "billCategory": 1,
      "currencyKey": "USD",
      "amount": "100",
      "createTimestamp": 1672056033898
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 连接账号
### 查询账号列表
**URL:** /api/v2/connect/account/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|客户的账户ID|-|
|fromNo|string|false|查询开始transactionNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/account/list --data '{
  "clientId": "1663027675055698121",
  "fromNo": "1663027675055698130",
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
|└─accountNo|string|账号编号|-|
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
      "accountNo": "11063639",
      "accountName": "小红的ETH账户",
      "blockchainKey": "ethereum",
      "addressList": [
        {
          "address": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
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
**URL:** /api/v2/connect/account/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|false|调用方唯一业务id，与参数accountNo二选一必填，如果两个都有值，将按照两个参数查询|-|
|accountNo|string|false|账号编号，与参数customerRefId二选一必填，如果两个都有值，将按照两个参数查询|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/account/detail --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "accountNo": "11063639"
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
|└─accountNo|string|账号编号|-|
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
    "accountNo": "11063639",
    "accountName": "小红的ETH账户",
    "blockchainKey": "ethereum",
    "addressList": [
      {
        "address": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
        "addressType": "DEFAULT"
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询账号币种列表
**URL:** /api/v2/connect/account/currency/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号币种列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|accountNo|string|true|账号编号|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/account/currency/list --data '{
  "clientId": "1663027675055698121",
  "accountNo": "11063639"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─currencyKey|string|币种唯一标识|-|
|└─currencyName|string|币种名称|-|
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
      "currencyKey": "USDT_ERC20",
      "currencyName": "USDT",
      "balance": "101.2",
      "addressList": [
        {
          "address": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
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

### 查询账号币种详情
**URL:** /api/v2/connect/account/currency/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询账号币种详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|accountNo|string|true|账号编号|-|
|currencyKey|string|true|币种唯一标识|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/account/currency/detail --data '{
  "clientId": "1663027675055698121",
  "accountNo": "11063639",
  "currencyKey": "BTC"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─currencyKey|string|币种唯一标识|-|
|└─currencyName|string|币种名称|-|
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
    "currencyKey": "USDT_ERC20",
    "currencyName": "USDT",
    "balance": "101.2",
    "addressList": [
      {
        "address": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
        "addressType": "DEFAULT"
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 连接账号交易
### 预估交易手续费
**URL:** /api/v2/connect/transaction/estimated/fee

**Type:** POST


**Content-Type:** application/json

**Description:** 预估交易手续费

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|accountNo|string|true|账号编号|-|
|currencyKey|string|true|币种唯一标识|-|
|amount|string|false|提币数量|-|
|toAddress|string|false|提币目标地址|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/transaction/estimated/fee --data '{
  "clientId": "1663027675055698121",
  "accountNo": "11063639",
  "currencyKey": "USDT_ERC20",
  "amount": "1.23456789",
  "toAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─feeCurrencyKey|string|手续费币种标识|-|
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
    "feeCurrencyKey": "ETH",
    "fee": "0.0009"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 创建交易
**URL:** /api/v2/connect/transaction/create

**Type:** POST


**Content-Type:** application/json

**Description:** 创建交易

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id，最长 100|-|
|accountNo|string|true|账号编号|-|
|currencyKey|string|true|币种唯一标识|-|
|amount|string|true|交易金额|-|
|toAddress|string|true|目标地址|-|
|note|string|false|备注，最长100|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/transaction/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "accountNo": "11063639",
  "currencyKey": "ETH",
  "amount": "1.2",
  "toAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93",
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
    "operateId": "101"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询交易列表
**URL:** /api/v2/connect/transaction/list

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|客户的账户ID|-|
|fromNo|string|false|查询开始transactionNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|
|accountNo|string|false|账号编号|-|
|currencyKey|string|false|币种唯一标识|-|
|transactionType|int32|false|交易类型（1:接收；2:发送；）<br/><br/>mock 1|-|
|createTimestampFrom|int64|false|创建时间开始时间戳，UNIX 时间戳毫秒数|-|
|createTimestampTo|int64|false|创建时间结束时间戳，UNIX 时间戳毫秒数|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/transaction/list --data '{
  "clientId": "1663027675055698121",
  "fromNo": "1663027675055698130",
  "limit": 20,
  "accountNo": "11063639",
  "currencyKey": "BTC",
  "transactionType": 1,
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
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|└─transactionNo|string|交易号|-|
|└─clientId|string|客户的账户ID|-|
|└─accountNo|string|账号编号|-|
|└─currencyKey|string|币种唯一标识|-|
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
      "createTimestamp": 1672056033898,
      "transactionNo": "1663027675055698130",
      "clientId": "1663027675055698121",
      "accountNo": "11063639",
      "currencyKey": "BTC",
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
**URL:** /api/v2/connect/transaction/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 查询交易详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|false|调用方唯一业务ID，与参数transactionNo二选一必填，如果两个都有值，将按照两个参数查询|-|
|transactionNo|string|false|交易号，与参数customerRefId二选一必填，如果两个都有值，将按照两个参数查询|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/transaction/detail --data '{
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
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|└─transactionNo|string|交易号|-|
|└─clientId|string|客户的账户ID|-|
|└─accountNo|string|账号编号|-|
|└─currencyKey|string|币种唯一标识|-|
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
    "createTimestamp": 1672056033898,
    "transactionNo": "1663027675055698130",
    "clientId": "1663027675055698121",
    "accountNo": "11063639",
    "currencyKey": "BTC",
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

| eventType                       | eventDetail                                               | Description  |
|---------------------------------|-----------------------------------------------------------|--------------|
| CLIENT_STATUS_CHANGED           | [clientDetail](#clientDetail)                             | 创建用户状态变更     |
| FUND_ACCOUNT_CREATED            | [fundAccountCreated](#fundAccountCreated)                 | 资金账号已创建      |
| CRYPTO_TX_CREATED               | [transactionDetail](#transactionDetail)                   | 数字货币交易创建     |
| CRYPTO_TX_STATUS_CHANGED        | [transactionDetail](#transactionDetail)                   | 数字货币交易状态变更   |
| FIAT_RECIPIENT_STATUS_CHANGED   | [fiatRecipientDetail](#fiatRecipientDetail)               | 法币收款人信息变更    |
| CRYPTO_RECIPIENT_STATUS_CHANGED | [cryptoRecipientDetail](#cryptoRecipientDetail)           | 加密货币收款人信息变更  |
| FIAT_TX_CREATED                 | [transactionDetail](#transactionDetail)                   | 法币创建交易       |
| FIAT_TX_STATUS_CHANGED          | [transactionDetail](#transactionDetail)                   | 法币交易状态变更     |
| CURRENCY_STATUS_CHANGED         | [currencyStatusDetail](#currencyStatusDetail)             | 账户币种状态变更     |
| DEPOSIT_INFO_ADD                | [depositAddressAddDetail](#depositAddressAddDetail)       | 账户币种收款信息添加   |
| DEPOSIT_INFO_CHANGED            | [depositAddressChangeDetail](#depositAddressChangeDetail) | 账户币种收款信息状态变更 |

### 事件详情
**<div id="clientDetail"> clientDetail </div>**

| Parameter      | Type   | Description           |
|----------------|--------|-----------------------|
| customerRefId  | string | 调用方唯一业务id             |
| clientId       | string | 客户的账户Id               |
| name           | string | 账号名                   |
| email          | string | 创建时的邮箱地址              |
| status         | int32  | 状态 1-审核中 2-已生效 3-审核拒绝 |

**<div id="fundAccountCreated"> fundAccountCreated </div>**

| Parameter          | Type   | Description      |
|--------------------|--------|------------------|
| clientId           | string | 客户的账户Id          |
| currencyList       | array  | 币种列表             | -     |
| └─currencyKey      | string | 币种唯一标识           | -     |
| └─currencyCategory | int32  | 币种分类 1-数字货币 2-法币 | -     |

**<div id="fiatRecipientDetail"> fiatRecipientDetail </div>**

| Field                  | Type   | Description                      | Since |
|------------------------|--------|----------------------------------|-------|
| customerRefId          | string | 调用方唯一业务id                        | -     |
| recipientId            | string | 收款方地址id                          | -     |
| channelKey             | string | 法币-转账通道 crypto,swift,local,conet | -     |
| subChannelKey          | string | 法币-转账子通道 ach,chats,fps           | -     |
| status                 | int32  | 收款人状态(1:审批中；2:已生效；3:审批拒绝)        | -     |
| currencyKey            | string | 币种标识                             | -     |
| swiftCode              | string | 收款银行swift码                       | -     |
| bankCode               | string | 收款银行代号                           | -     |
| bankName               | string | 收款银行名称                           | -     |
| bankCountryCode        | string | 开户银行所在地iso code                  | -     |
| beneficiaryRoutingCode | string | ABA/ACH的路由号                      | -     |
| beneficiaryAccountNo   | string | 收款人银行账户号码/IBAN                   | -     |
| beneficiaryName        | string | 收款人姓名                            | -     |
| beneficiaryCountryCode | string | 收款人国家iso code                    | -     |
| beneficiaryStreet      | string | 收款人街道                            | -     |
| beneficiaryCity        | string | 收款人城市                            | -     |
| beneficiaryState       | string | 收款人州/省                           | -     |
| beneficiaryPostalCode  | string | 收款人邮编                            | -     |
| conetId                | int64  | conet收款方式对方conetId               | -     |
| note                   | string | 备注                               | -     |
| label                  | string | 别称                               | -     |

**<div id="cryptoRecipientDetail"> cryptoRecipientDetail </div>**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─customerRefId|string|调用方唯一业务id|-|
|└─recipientId|string|收款方地址id|-|
|└─status|int32|收款人状态(1:审批中；2:已生效；3:审批拒绝)|-|
|└─currencyKey|string|币种标识|-|
|└─address|string|加密货币地址|-|
|└─label|string|别称|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**<div id="transactionDetail"> transactionDetail </div>**

| Field                | Type   | Description                                                                                                                                                                | Since |
|----------------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------|
| customerRefId        | string | 调用方唯一业务ID                                                                                                                                                                  | -     |
| transactionNo        | string | 交易号                                                                                                                                                                        | -     |
| clientId             | string | 客户的账户ID                                                                                                                                                                    | -     |
| createTimestamp      | int64  | 创建时间，UNIX 时间戳毫秒数                                                                                                                                                           | -     |
| completedTimestamp   | int64  | 完成时间，UNIX 时间戳毫秒数                                                                                                                                                           | -     |
| transferCurrencyKey  | string | 转账币种唯一标识                                                                                                                                                                   | -     |
| currencyCategory     | int32  | 币种类别(1:数字货币;2:法币;)                                                                                                                                                         | -     |
| transactionType      | int32  | 交易类型（1：接收；2：发送；5：退款；）                                                                                                                                                      | -     |
| payAccountName       | string | 付款方名称                                                                                                                                                                      | -     |
| beneficiaryName      | string | 收款方名称                                                                                                                                                                      | -     |
| transactionAmount    | string | 交易数量                                                                                                                                                                       | -     |
| transactionStatus    | string | 交易状态（SUBMITTED:审批中；PROCESSING:处理中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF:已上传凭证；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；） | -     |
| transactionSubStatus | string | 交易子状态                                                                                                                                                                      | -     |
| platformFee          | string | 平台手续费                                                                                                                                                                      | -     |
| note                 | string | 备注                                                                                                                                                                         | -     |
| beneficiaryId        | int64  | 收款人ID                                                                                                                                                                      | -     |
| fiatFeeMethod        | int32  | 法币手续费方式（1：支付本地银行服务费；2：支付本地银行服务费与收款行服务费；）                                                                                                                                   | -     |
| channelKey           | string | 转账通道（crypto,swift,local,conet）                                                                                                                                             | -     |
| subChannelKey        | string | 转账子通道（fps；chats；ach），当转账方式为“local”时，存在子通道的值                                                                                                                                | -     |
| proofEn              | string | 需要上传凭证的英文说明                                                                                                                                                                | -     |
| proofCn              | string | 需要上传凭证的中文说明                                                                                                                                                                | -     |
| cryptoBlockHeight    | int64  | 数字货币区块高度                                                                                                                                                                   | -     |
| cryptoFromAddress    | string | 数字货币交易来源地址                                                                                                                                                                 | -     |
| cryptoToAddress      | string | 数字货币交易目标地址                                                                                                                                                                 | -     |
| cryptoTxHash         | string | 数字货币交易hash                                                                                                                                                                 | -     |
| cryptoTxFee          | string | 数字货币链上手续费                                                                                                                                                                  | -     |

**<div id="currencyStatusDetail"> currencyStatusDetail </div>**

| Field       | Type   | Description    | Since |
|-------------|--------|----------------|-------|
| clientId    | string | 客户的账户ID        | -     |
| currencyKey | string | 币种key          | -     |
| status      | int32  | 状态 1:未生效 2:已生效 | -     |

**<div id="depositAddressAddDetail"> depositAddressAddDetail </div>**

| Field                  | Type   | Description                                 | Since |
|------------------------|--------|---------------------------------------------|-------|
| clientId               | string | 客户的账户ID                                     | -     |
| depositAddressList     | array  | 地址列表                                        | -     |
| └─conetId              | int64  | 用于内部转账的收款账号id                               | -     |
| └─currencyCategory     | int32  | 币种分类 1-数字货币 2-法币                            | -     |
| └─currencyKey          | string | 币种标识                                        | -     |
| └─currencyName         | string | 币种名                                         | -     |
| └─channelKey           | string | 币种-转账通道 crypto,swift,local,conet            | -     |
| └─subChannelKey        | string | 法币-转账子通道,当channelKey=local时有值 ach,chats,fps | -     |
| └─bankAccountType      | int32  | 法币-银行账号类型 1-CA 2-VA                         | -     |
| └─bankName             | string | 法币-银行名称                                     | -     |
| └─bankAddress          | string | 法币-银行地址                                     | -     |
| └─bankCode             | string | 法币-银行代码                                     | -     |
| └─branchCode           | string | 法币-分行代码                                     | -     |
| └─swiftCode            | string | 法币-SWIFT                                    | -     |
| └─bankCountry          | string | 法币-银行国家                                     | -     |
| └─beneficiaryAccountNo | string | 法币-银行账号                                     | -     |
| └─beneficiaryName      | string | 法币-收款人姓名                                    | -     |
| └─beneficiaryCountry   | string | 法币-收款人国家                                    | -     |
| └─beneficiaryAddress   | string | 法币-收款人地址                                    | -     |
| └─note                 | string | 法币-转入需要的附言                                  | -     |

**<div id="depositAddressChangeDetail"> depositAddressChangeDetail </div>**

| Field                | Type   | Description                                 | Since |
|----------------------|--------|---------------------------------------------|-------|
| clientId             | string | 客户的账户ID                                     | -     |
| conetId              | int64  | 用于内部转账的收款账号id                               | -     |
| currencyCategory     | int32  | 币种分类 1-数字货币 2-法币                            | -     |
| currencyKey          | string | 币种标识                                        | -     |
| currencyName         | string | 币种名                                         | -     |
| channelKey           | string | 币种-转账通道 crypto,swift,local,conet            | -     |
| subChannelKey        | string | 法币-转账子通道,当channelKey=local时有值 ach,chats,fps | -     |
| bankAccountType      | int32  | 法币-银行账号类型 1-CA 2-VA                         | -     |
| bankName             | string | 法币-银行名称                                     | -     |
| bankAddress          | string | 法币-银行地址                                     | -     |
| bankCode             | string | 法币-银行代码                                     | -     |
| branchCode           | string | 法币-分行代码                                     | -     |
| swiftCode            | string | 法币-SWIFT                                    | -     |
| bankCountry          | string | 法币-银行国家                                     | -     |
| beneficiaryAccountNo | string | 法币-银行账号                                     | -     |
| beneficiaryName      | string | 法币-收款人姓名                                    | -     |
| beneficiaryCountry   | string | 法币-收款人国家                                    | -     |
| beneficiaryAddress   | string | 法币-收款人地址                                    | -     |
| status               | int32  | 状态 1-关闭 2-开启                                | -     |
| note                 | string | 法币-转入需要的附言                                  | -     |




## 错误码列表

| Error code | Description |
|------------|-------------|
|401|未登录|
|412|参数错误|
|413|远程调用参数错误|
|414|远程调用业务异常|
|415|远程调用超时|
|500|系统异常|
|10101|验证签名失败|
|10102|解密请求失败|
|10103|重复提交|
|10104|请求频繁，请稍后重试|
|10105|幂等调用|
|10106|IP拒绝|
|10201|钱包名称不能超过{0}个字符|
|10202|只能删除余额小于10美元的账号|
|10203|账号已冻结|
|10204|审批人数量大于管理员数量|
|10205|钱包账号不支持该币种|
|10206|存在进行中的审批|
|10207|冻结时间必须是将来时间|
|10208|账号不存在|
|10209|账号不支持该币种|
|10301|输入错误次数过多，请{0}分钟后再试|
|10302|验证码无效，在您的账户被锁定{1}分钟前，您还有{0}次尝试机会|
|10303|验证码无效，您还有{0}次尝试机会|
|10401|用户已存在|
|10402|用户不存在|
|10403|不能删除审批中的用户|
|10404|不能删除自己|
|10405|当前钱包存在审批，无法删除钱包管理员|
|10406|在删除管理员之前需要先减少审批数量|
|10501|钱包提币白名单标签不能超过20个字符|
|10502|钱包提币白名单地址不能超过200个字符|
|10503|该白名单地址已存在|
|10504|不能删除审批中的白名单|
|10601|提币规则已存在|
|10701|接收地址格式错误|
|10702|您的收款地址有风险，不能转账|
|10703|目标地址不能为合约地址|
|10704|钱包资产余额不足，无法提币|
|10705|预估网络费用不足，您需要向此钱包至少存入{0}来支付提币网络费用|
|10706|提币数量小数精度不能超过{0}|
|10707|提币数量超过组织限额|
|10708|提币数量超过限制|
|10709|暂停充币|
|10710|暂停提币|
|10711|钱包被冻结，无法提币|
|10712|更新二次验证码后，24小时内禁止提币|
|10713|更新登录密码后，24小时内禁止提币|
|10714|该机构已失效，无法提币|
|10715|收款账号未生效，不能收款|
|10801|该机构已失效，无法转账|
|10802|暂停充值|
|10803|暂停转账|
|10804|钱包被冻结，无法转账|
|10805|更新二次验证码后，24小时内禁止转账|
|10806|更新登录密码后，24小时内禁止转账|
|10807|转账数量小数精度不能超过{0}|
|10808|单笔转账最低{0} {1}|
|10808|单笔转账最大{0} {1}|
|10809|托管⾏不同，请选择银行转账|
|10810|收款账号被冻结，不能收款|
|10811|收款机构已失效，无法收款|
|10812|转账手续费已经变更，请重新提交|
|10813|准备金不足|
|10814|币种不存在|
|10815|手续费规则不存在|
|10901|操作授权验证失败|
|10902|收款人不存在|
|10903|收款人信息错误|
|10904|收款人状态不允许删除|
|10905|收款人添加通道已关闭|
|11001|交易记录不存在|
|11002|当前交易状态不允许的操作|
|11101|账单自动付款方式已存在|
|11102|账单已经支付|
|11103|账单已经存在支付请求|
|11104|账单支付超时|
|11105|超过自动支付限额|
|11106|账户余额不足|
|11201|只能添加一个CA账号|
|11202|银行不存在|
|11203|银行不支持创建虚拟账号|
|11204|只能添加一种VA账号|
|11301|冻结金额大于可用余额|
|11302|解冻金额大于冻结金额|
|11401|该机构已失效，无法交易|
|11402|该机构未开通交易服务|
|11403|交易对不可用|
|11404|钱包被冻结，无法交易|
|11405|更新二次验证码后，24小时内禁止交易|
|11406|更新登录密码后，24小时内禁止交易|
|11407|交易数量小数精度不能超过{0}|
|11408|交易数量超过限额|
|11409|钱包资产余额不足，无法交易|
|11410|预估网络费用不足，您需要向此钱包至少存入{0}来支付提币网络费用|
|11411|兑换金额过大，请联系客服处理|
|11412|单笔交易最低{0} {1}|
|11413|单笔交易最大{0} {1}|
|11501|汇率异常|

