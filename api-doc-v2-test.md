# ClearOnes Open Api v2 接口文档

| Version | Update Time         | Status | Author | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|---------|---------------------|------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2.0.0   | 2023-03-14 12:00:00 |create|clearones| 创建文档                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 2.0.1   | 2024-03-28 12:59:00 |modify|clearones| 授权验证接口新增类型5:连接账号提币；连接账号模块新增接口：查询账号列表、查询账号详情、查询账号币种列表、查询账号币种详情、预估交易手续费、创建交易；连接账号模块查询交易详情接口参数新增：调用方唯一业务ID(customerRefId)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 2.0.2   | 2024-04-18 10:59:00 |modify|clearones| 法币预估手续费接口增加必填字段recipientId                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
| 2.0.3   | 2024-04-23 21:25:00 |modify|clearones| webhook新增“连接账号交易创建“和”连接账号交易状态变更”事件                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 2.0.4   | 2024-04-24 10:59:00 |modify|clearones| 添加交易凭证接口支持一次传递多个objectKey                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
| 2.0.5   | 2024-04-26 15:22:00 |modify|clearones| 1、/api/v2/recipient/fiat/create接口增加参数branchCode，sortCode，beneficiaryEntityType，beneficiaryCompanyName，beneficiaryFirstName，beneficiaryLastName. 2、/api/v2/recipient/fiat/list接口返回值新增branchCode，bankAddress，sortCode，beneficiaryEntityType，beneficiaryCompanyName，beneficiaryFirstName，beneficiaryLastName. 3、/api/v2/recipient/fiat/detail接口返回值新增branchCode，bankAddress，sortCode，beneficiaryEntityType，beneficiaryCompanyName，beneficiaryFirstName，beneficiaryLastName. 4、/api/v2/fund/account/currency/deposit接口返回值新增sortCode、routingCode.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   
| 2.0.6   | 2024-04-28 15:41:00 |modify|clearones| 1、/api/v2/connect/transaction/list接口返回值增加customerRefId。2、/api/v2/connect/transaction/detail接口返回值增加customerRefId。3、webhook事件“CONNECT_TX_CREATED”和“CONNECT_TX_STATUS_CHANGED”的内容增加customerRefId。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
| 2.0.7   | 2024-05-13 11:36:00 |modify|clearones| 预估交易手续费接口（/api/v2/connect/transaction/estimated/fee），参数toAddress校验规则修改为：提币目标地址（当blockchainKey是solana时，必传，其他传了计算预估手续费会更精确）。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
| 2.0.8   | 2024-05-31 18:50:00 |modify|clearones| 预估交易手续费接口（/api/v2/transaction/crypto/estimated/fee），增加参数address                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               
| 2.0.9   | 2024-06-04 17:03:00 |modify|clearones| 新增webhook事件：SUPER_ORG_CRYPTO_RECIPIENT_CREATE（主机构数字货币收款人信息创建）、SUPER_ORG_FIAT_RECIPIENT_CREATE（主机构法币收款人信息创建）、SUPER_ORG_CRYPTO_RECIPIENT_STATUS_CHANGED（主机构数字货币收款人信息变更）、SUPER_ORG_FIAT_RECIPIENT_STATUS_CHANGED（主机构法币收款人信息变更）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   
| 2.0.10  | 2024-06-12 10:49:00 |modify|clearones| 1、修改“授权验证”接口：/api/v2/authorization/verify，接口参数authorizationType取值范围新增：“6：FX创建交易”；2、新增“查询用户FX交易对列表”接口：/api/v2/fx/client/pair/list；3、新增“查询FX交易列表”接口：/api/v2/fx/transaction/list；4、新增“查询FX交易详情”接口：/api/v2/fx/transaction/detail；5、新增“创建FX交易”接口：/api/v2/fx/transaction/create；6、新增“FX交易创建”事件：FX_TX_CREATED；7、新增“FX交易状态变更”事件：FX_TX_STATUS_CHANGED；8、新增“FX交易对添加”事件：FX_PAIR_ADD；9、新增“FX交易对更新”事件：FX_PAIR_UPDATE；10、新增“FX交易对删除”事件：FX_PAIR_DELETE                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 2.0.11  | 2024-06-19 16:39:00 |modify|clearones| 1、完善FX交易状态描述；2、FX交易记录查询接口返回新增转账交易号、收款交易号和退款交易号；                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 2.0.12  | 2024-06-27 12:26:00 |modify|clearones| 1. /api/v2/recipient/fiat/supportCreateChannel接口返回值channelKey的取值范围新增”china_mainland”付款方式。<br><br> 2. /api/v2/recipient/fiat/create接口参数变更：<br> （1）接口参数channelKey取值范围新增“china_mainland”（法币-中国大陆）。<br>（2）接口参数bankName修改为：当channelKey为swift、local、china_mainland时，必填。新增了当channelKey为china_mainland时必填规则。<br>（3）接口参数beneficiaryName修改为：银行账号持有者姓名，当channelKey为swift、local和china_mainland时，必填。新增了当channelKey为china_mainland时必填规则。<br>（4）接口参数beneficiaryAccountNo修改为：收款人银行账户号码/IBAN（当收款银行国家为欧盟成员时，填写IBAN），当channelKey为swift、local和china_mainland时，必填。新增了当channelKey为china_mainland时必填规则。<br>（5）接口参数新增beneficiaryIdNumber（收款人证件号）、beneficiaryPhoneNumber（收款人手机号），当channelKey为china_mainland时，必填。<br><br> 3. /api/v2/recipient/fiat/list接口修改：<br>（1）返回值channelKey的取值范围新增”china_mainland”付款方式。<br>（2）返回值新增beneficiaryIdNumber（收款人证件号）、beneficiaryPhoneNumber（收款人手机号）。<br><br> 4. /api/v2/recipient/fiat/detail接口修改，返回值新增beneficiaryIdNumber（收款人证件号）、beneficiaryPhoneNumber（收款人手机号）。<br><br> 5. webhook事件FIAT_RECIPIENT_STATUS_CHANGED通知内容新增beneficiaryIdNumber（收款人证件号）、beneficiaryPhoneNumber（收款人手机号）。<br><br> 6. /api/v2/transaction/fiat/fee接口参数channelKey的取值范围新增”china_mainland”付款方式。<br><br> 7. /api/v2/transaction/list接口返回值channelKey的取值范围新增”china_mainland”付款方式。<br><br> 8. /api/v2/transaction/detail接口返回值channelKey的取值范围新增”china_mainland”付款方式。 |
| 2.0.13  | 2024-07-05 12:26:00 |modify|clearones| 添加获取转账凭证下载地址接口                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 2.0.14  | 2024-07-19 10:50:00 |modify|clearones| 查询用户FX交易对列表增加 thresholdAmount、thresholdFeeRate两个字段，兑换金额低于thresholdAmount时费率使用thresholdFeeRate计算，webhook交易对同步增加上述两个字段                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 2.0.15  | 2024-07-23 15:20:00 |modify|clearones| 交易记录查询增加hasTransferNotice(是否可下载转账凭证)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 2.0.16  | 2024-08-23 15:20:00 |modify|clearones| 加密货币和法币收款人查询增加 superOrgRecipientId(收款人添加来源id)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 2.0.17  | 2024-11-21 11:20:00 |modify|clearones| 法币转出交易详情增加payBankName,payBankAddress,payAccountNo字段                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 2.0.18  | 2024-12-17 11:20:00 |modify|clearones| 新增fx预算接口（/api/v2/fx/transaction/check），fx创建交易接口新增exchangeRate字段                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 2.0.19  | 2025-01-20 11:20:00 |modify|clearones| 1. 新增接口：连接账号交易-直接创建交易（/api/v2/connect/transaction/create/direct）<br> 2. 新增接口：FX兑换业务-直接创建FX交易（/api/v2/fx/transaction/create/direct）<br> 3. 新增接口：收款人管理模块-法币直接创建收款方请求（/api/v2/recipient/fiat/create/direct）<br> 4. 新增接口：收款人管理模块-加密货币直接创建收款方请求（/api/v2/recipient/crypto/create/direct）<br> 5. 新增接口：交易-直接创建交易加密交易（/api/v2/transaction/crypto/create/direct）<br> 6. 新增接口：交易-直接创建一个法币转账（/api/v2/transaction/fiat/create/direct）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 2.0.20  | 2025-02-05 11:20:00 |modify|clearones| 增加卡业务相关接口                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 2.0.21  | 2025-03-20 11:20:00 |modify|clearones| 1. 修改接口：conet-交易-创建一个法币转账（需要用户授权）（/api/v2/transaction/fiat/create），新增参数additionalFee（附加手续费（转账手续费=平台手续费+附加手续费））<br> 2. 修改接口：conet-交易-直接创建一个法币转账（/api/v2/transaction/fiat/create/direct），新增参数additionalFee（附加手续费（转账手续费=平台手续费+附加手续费））<br> 3. 修改接口：conet-交易-查询交易列表（/api/v2/transaction/list），新增返回值additionalFee（附加手续费（转账手续费=平台手续费+附加手续费））<br> 4. 修改接口：conet-交易-查询交易详情（/api/v2/transaction/detail），新增返回值additionalFee（附加手续费（转账手续费=平台手续费+附加手续费））<br> 5. webhook事件'FIAT_TX_CREATED'，新增参数additionalFee（附加手续费（转账手续费=平台手续费+附加手续费））<br> 6. webhook事件'FIAT_TX_STATUS_CHANGED'，新增参数additionalFee（附加手续费（转账手续费=平台手续费+附加手续费））<br> 7. 新增错误码：10816：转账附加服务费小数精度错误；                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

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

ClearOnes 所有的 API 接口在请求时，采用了对称密钥加密和非对称公钥加密的混合加密方案，并且使用非对称私钥对请求参数进行签名。对称加密算法采用
AES-256 算法，非对称加密和签名算法采用 RSA-4096 算法，具体流程如下：

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

**<div id="channelKey">转账通道(channelKey/subChannelKey)分为:</div>**

+ swift: swift国际电汇转账方式，适用于法币
+ local: 银行本地支持的转账方式,channelKey为local时,subChannelKey可分为以下子通道，适用于法币
    + chats: 香港的特快转账(RTGS/CHATS)
    + fps: 香港转数快
    + ach: 美国电子资金转账网络
    + fedwire: 美国联邦储备银行运营的实时全额结算资金转账系统
    + sepa: 欧盟推行的统一欧元支付区
    + faster_payment: 实时支付系统
    + eft: 电子资金转账
+ conet: Clearones的内部转账，适用于法币和加密货币
+ crypto: 加密货币链上转账方式，适用于加密货币
+ china_mainland: 中国大陆转账方式，适用于法币

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
|materials|array|false|需要上传的材料列表|-|
|identificationNo|string|false|身份证明文件号，个人类型为必传|-|
|address|string|true|地址|-|
|birthday|string|false|生日|-|
|occupation|string|false|职业|-|
|gender|string|false|性别|-|
|contactNumber|string|false|电话|-|

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

**Description:** 把加签加密参数放在url后?apiKey=xxx&bizContent=参数加密哈希后的串&sign=xxx&key=xxx;支持的文件格式:
jpg、jpeg、png、pdf、zip、rar、7z

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
curl -X POST -H 'Content-Type: multipart/form-data' -F 'file=' -i /api/v2/file/upload --data 'apiKey=6o2hdx&timestamp=1715065450461266&bizContent=conkhb&key=w5et16&sign=i8dqwf'
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
|└─channelKey|string|币种-[转账通道](#channelKey)|-|
|└─subChannelKey|string|法币-[转账子通道](#channelKey)|-|
|└─bankAccountType|int32|法币-银行账号类型 1-CA 2-VA|-|
|└─bankCountry|string|法币-银行国家|-|
|└─bankName|string|法币-银行名称|-|
|└─bankAddress|string|法币-银行地址|-|
|└─bankCode|string|法币-银行代码|-|
|└─branchCode|string|法币-分行代码|-|
|└─swiftCode|string|法币-SWIFT|-|
|└─sortCode|string|法币-Sort Code|-|
|└─routingCode|string|法币-Routing Code|-|
|└─beneficiaryAccountNo|string|法币-银行账户号码/IBAN|-|
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
      "subChannelKey": "fps",
      "bankAccountType": 1,
      "bankCountry": "Hong Kong",
      "bankName": "China CITIC Bank International Limited",
      "bankAddress": "61-65 Des Voeux Road Central. Hong Kong",
      "bankCode": "012",
      "branchCode": "123",
      "swiftCode": "KWHKHKHH",
      "sortCode": "041404",
      "routingCode": "35210009",
      "beneficiaryAccountNo": "8334331484",
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
|└─channelKey|string|法币-[转账通道](#channelKey)|-|
|└─subChannelKey|string|法币-[转账子通道](#channelKey)|-|
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

### 法币创建收款方请求（需要用户授权）

**URL:** /api/v2/recipient/fiat/create

**Type:** POST

**Content-Type:** application/json

**Description:** 法币创建收款方请求（需要用户授权）

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id|-|
|channelKey|string|true|法币-[转账通道](#channelKey)|-|
|subChannelKey|string|false|法币-[转账子通道](#channelKey),当channelKey为local时，必填。|-|
|currencyKey|string|true|币种标识|-|
|conetId|string|false|平台内部的收款账号id，当channelKey为conet时，必填。|-|
|swiftCode|string|false|银行swift码，当channelKey为swift或local时，必填。|-|
|bankCode|string|false|收款银行code, 当channelKey为local，subChannelKey为fps、chats时，必填。|-|
|branchCode|string|false|收款银行分行code, 当channelKey为local，subChannelKey为fps、chats时，可选。|-|
|bankName|string|false|收款银行名称，当channelKey为swift、local和china_mainland时，必填。|-|
|bankCountryCode|string|false|收款银行国家ISO code，当channelKey为swift或local时，必填。|-|
|bankAddress|string|false|收款银行地址|-|
|sortCode|string|false|Sort Code, 当channelKey为local，subChannelKey为faster_payment时，必填|-|
|beneficiaryRoutingCode|string|false|Routing Code, 当channelKey为local，subChannelKey为ach、fedwire、sepa、eft时，必填。|-|
|beneficiaryAccountNo|string|false|收款人银行账户号码/IBAN（当收款银行国家为欧盟成员时，填写IBAN），当channelKey为swift、local和china_mainland时，必填。|-|
|beneficiaryName|string|false|银行账号持有者姓名，当channelKey为swift、local和china_mainland时，必填。|-|
|beneficiaryEntityType|string|false|收款人实体类型（individual：个人；company：公司；），当channelKey为swift或local时，必填。|-|
|beneficiaryCompanyName|string|false|收款人公司名，当beneficiaryEntityType为company时，必填|-|
|beneficiaryFirstName|string|false|收款人first name，当beneficiaryEntityType为individual时，必填|-|
|beneficiaryLastName|string|false|收款人last name，当beneficiaryEntityType为individual时，必填|-|
|beneficiaryCountryCode|string|false|收款人国家ISO code，当channelKey为swift或local时，必填。|-|
|beneficiaryStreet|string|false|收款人街道，当channelKey为swift或local时，必填。|-|
|beneficiaryCity|string|false|收款人城市，当channelKey为swift或local时，必填。|-|
|beneficiaryState|string|false|收款人州/省，当收款人国家为美国（US）、加拿大（CA）、墨西哥（MX）时，必填|-|
|beneficiaryPostalCode|string|false|收款人邮编，当收款人国家为美国（US）、加拿大（CA）、墨西哥（MX）时，必填|-|
|beneficiaryIdNumber|string|false|收款人证件号，当channelKey为china_mainland时，必填。|-|
|beneficiaryPhoneNumber|string|false|收款人手机号，当channelKey为china_mainland时，必填。|-|
|note|string|false|备注|-|
|label|string|false|标签别称|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/fiat/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "channelKey": "local",
  "subChannelKey": "fps",
  "currencyKey": "USD",
  "conetId": "1009131",
  "swiftCode": "HSBCHKHHHKH",
  "bankCode": "012",
  "branchCode": "456",
  "bankName": "China CITIC Bank International Limited",
  "bankCountryCode": "HK",
  "bankAddress": "8 Finance Street, Central, Hong Kong",
  "sortCode": "123456",
  "beneficiaryRoutingCode": "123123456",
  "beneficiaryAccountNo": "123123456789",
  "beneficiaryName": "XIAO HONG",
  "beneficiaryEntityType": "individual",
  "beneficiaryCompanyName": "Xiaohong Technology Co., Ltd.",
  "beneficiaryFirstName": "XIAO",
  "beneficiaryLastName": "HONG",
  "beneficiaryCountryCode": "HK",
  "beneficiaryStreet": "8 Finance Street",
  "beneficiaryCity": "Central",
  "beneficiaryState": "Hong Kong",
  "beneficiaryPostalCode": "999077",
  "beneficiaryIdNumber": "231010199707010101",
  "beneficiaryPhoneNumber": "15800001010",
  "note": "小白",
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

### 法币直接创建收款方请求

**URL:** /api/v2/recipient/fiat/create/direct

**Type:** POST

**Content-Type:** application/json

**Description:** 法币直接创建收款方请求

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id|-|
|channelKey|string|true|法币-[转账通道](#channelKey)|-|
|subChannelKey|string|false|法币-[转账子通道](#channelKey),当channelKey为local时，必填。|-|
|currencyKey|string|true|币种标识|-|
|conetId|string|false|平台内部的收款账号id，当channelKey为conet时，必填。|-|
|swiftCode|string|false|银行swift码，当channelKey为swift或local时，必填。|-|
|bankCode|string|false|收款银行code, 当channelKey为local，subChannelKey为fps、chats时，必填。|-|
|branchCode|string|false|收款银行分行code, 当channelKey为local，subChannelKey为fps、chats时，可选。|-|
|bankName|string|false|收款银行名称，当channelKey为swift、local和china_mainland时，必填。|-|
|bankCountryCode|string|false|收款银行国家ISO code，当channelKey为swift或local时，必填。|-|
|bankAddress|string|false|收款银行地址|-|
|sortCode|string|false|Sort Code, 当channelKey为local，subChannelKey为faster_payment时，必填|-|
|beneficiaryRoutingCode|string|false|Routing Code, 当channelKey为local，subChannelKey为ach、fedwire、sepa、eft时，必填。|-|
|beneficiaryAccountNo|string|false|收款人银行账户号码/IBAN（当收款银行国家为欧盟成员时，填写IBAN），当channelKey为swift、local和china_mainland时，必填。|-|
|beneficiaryName|string|false|银行账号持有者姓名，当channelKey为swift、local和china_mainland时，必填。|-|
|beneficiaryEntityType|string|false|收款人实体类型（individual：个人；company：公司；），当channelKey为swift或local时，必填。|-|
|beneficiaryCompanyName|string|false|收款人公司名，当beneficiaryEntityType为company时，必填|-|
|beneficiaryFirstName|string|false|收款人first name，当beneficiaryEntityType为individual时，必填|-|
|beneficiaryLastName|string|false|收款人last name，当beneficiaryEntityType为individual时，必填|-|
|beneficiaryCountryCode|string|false|收款人国家ISO code，当channelKey为swift或local时，必填。|-|
|beneficiaryStreet|string|false|收款人街道，当channelKey为swift或local时，必填。|-|
|beneficiaryCity|string|false|收款人城市，当channelKey为swift或local时，必填。|-|
|beneficiaryState|string|false|收款人州/省，当收款人国家为美国（US）、加拿大（CA）、墨西哥（MX）时，必填|-|
|beneficiaryPostalCode|string|false|收款人邮编，当收款人国家为美国（US）、加拿大（CA）、墨西哥（MX）时，必填|-|
|beneficiaryIdNumber|string|false|收款人证件号，当channelKey为china_mainland时，必填。|-|
|beneficiaryPhoneNumber|string|false|收款人手机号，当channelKey为china_mainland时，必填。|-|
|note|string|false|备注|-|
|label|string|false|标签别称|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/fiat/create/direct --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "channelKey": "local",
  "subChannelKey": "fps",
  "currencyKey": "USD",
  "conetId": "1009131",
  "swiftCode": "HSBCHKHHHKH",
  "bankCode": "012",
  "branchCode": "456",
  "bankName": "China CITIC Bank International Limited",
  "bankCountryCode": "HK",
  "bankAddress": "8 Finance Street, Central, Hong Kong",
  "sortCode": "123456",
  "beneficiaryRoutingCode": "123123456",
  "beneficiaryAccountNo": "123123456789",
  "beneficiaryName": "XIAO HONG",
  "beneficiaryEntityType": "individual",
  "beneficiaryCompanyName": "Xiaohong Technology Co., Ltd.",
  "beneficiaryFirstName": "XIAO",
  "beneficiaryLastName": "HONG",
  "beneficiaryCountryCode": "HK",
  "beneficiaryStreet": "8 Finance Street",
  "beneficiaryCity": "Central",
  "beneficiaryState": "Hong Kong",
  "beneficiaryPostalCode": "999077",
  "beneficiaryIdNumber": "231010199707010101",
  "beneficiaryPhoneNumber": "15800001010",
  "note": "小白",
  "label": "zhangsan"
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
|└─channelKey|string|法币-[转账通道](#channelKey)|-|
|└─subChannelKey|string|法币-[转账子通道](#channelKey)|-|
|└─status|int32|收款人状态(1:审批中；2:已生效；3:审批拒绝)|-|
|└─currencyKey|string|币种标识|-|
|└─swiftCode|string|收款银行swift码|-|
|└─bankCode|string|收款银行代号|-|
|└─branchCode|string|收款银行分行code|-|
|└─bankName|string|收款银行名称|-|
|└─bankCountryCode|string|收款银行国家ISO code|-|
|└─bankAddress|string|收款银行地址|-|
|└─sortCode|string|Sort Code|-|
|└─beneficiaryRoutingCode|string|Routing Code|-|
|└─beneficiaryAccountNo|string|收款人银行账户号码/IBAN|-|
|└─beneficiaryName|string|银行账号持有者姓名|-|
|└─beneficiaryEntityType|string|收款人实体类型（individual：个人；company：公司；）|-|
|└─beneficiaryCompanyName|string|收款人公司名|-|
|└─beneficiaryFirstName|string|收款人first name|-|
|└─beneficiaryLastName|string|收款人last name|-|
|└─beneficiaryCountryCode|string|收款人国家ISO code|-|
|└─beneficiaryStreet|string|收款人街道|-|
|└─beneficiaryCity|string|收款人城市|-|
|└─beneficiaryState|string|收款人州/省|-|
|└─beneficiaryPostalCode|string|收款人邮编|-|
|└─beneficiaryIdNumber|string|收款人证件号|-|
|└─beneficiaryPhoneNumber|string|收款人手机号|-|
|└─conetId|int64|conet收款方式对方conetId|-|
|└─note|string|备注|-|
|└─label|string|别称|-|
|└─superOrgRecipientId|string|收款人来源id|-|
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
      "subChannelKey": "pfs",
      "status": 2,
      "currencyKey": "USD",
      "swiftCode": "HSBCHKHHHKH",
      "bankCode": "012",
      "branchCode": "456",
      "bankName": "China CITIC Bank International Limited",
      "bankCountryCode": "HK",
      "bankAddress": "8 Finance Street, Central, Hong Kong",
      "sortCode": "123456",
      "beneficiaryRoutingCode": "123123456",
      "beneficiaryAccountNo": "123123456789",
      "beneficiaryName": "XIAO HONG",
      "beneficiaryEntityType": "individual",
      "beneficiaryCompanyName": "Xiaohong Technology Co., Ltd.",
      "beneficiaryFirstName": "XIAO",
      "beneficiaryLastName": "HONG",
      "beneficiaryCountryCode": "HK",
      "beneficiaryStreet": "8 Finance Street",
      "beneficiaryCity": "Central",
      "beneficiaryState": "Hong Kong",
      "beneficiaryPostalCode": "999077",
      "beneficiaryIdNumber": "231010199707010101",
      "beneficiaryPhoneNumber": "15800001010",
      "conetId": 1009213,
      "superOrgRecipientId": "45",
      "note": "小白",
      "label": "zhangsan"
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
|└─channelKey|string|法币-[转账通道](#channelKey)|-|
|└─subChannelKey|string|法币-[转账子通道](#channelKey)|-|
|└─status|int32|收款人状态(1:审批中；2:已生效；3:审批拒绝)|-|
|└─currencyKey|string|币种标识|-|
|└─swiftCode|string|收款银行swift码|-|
|└─bankCode|string|收款银行代号|-|
|└─branchCode|string|收款银行分行code|-|
|└─bankName|string|收款银行名称|-|
|└─bankCountryCode|string|收款银行国家ISO code|-|
|└─bankAddress|string|收款银行地址|-|
|└─sortCode|string|Sort Code|-|
|└─beneficiaryRoutingCode|string|Routing Code|-|
|└─beneficiaryAccountNo|string|收款人银行账户号码/IBAN|-|
|└─beneficiaryName|string|银行账号持有者姓名|-|
|└─beneficiaryEntityType|string|收款人实体类型（individual：个人；company：公司；）|-|
|└─beneficiaryCompanyName|string|收款人公司名|-|
|└─beneficiaryFirstName|string|收款人first name|-|
|└─beneficiaryLastName|string|收款人last name|-|
|└─beneficiaryCountryCode|string|收款人国家ISO code|-|
|└─beneficiaryStreet|string|收款人街道|-|
|└─beneficiaryCity|string|收款人城市|-|
|└─beneficiaryState|string|收款人州/省|-|
|└─beneficiaryPostalCode|string|收款人邮编|-|
|└─beneficiaryIdNumber|string|收款人证件号|-|
|└─beneficiaryPhoneNumber|string|收款人手机号|-|
|└─conetId|int64|conet收款方式对方conetId|-|
|└─note|string|备注|-|
|└─label|string|别称|-|
|└─superOrgRecipientId|string|收款人来源id|-|
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
    "subChannelKey": "pfs",
    "status": 2,
    "currencyKey": "USD",
    "swiftCode": "HSBCHKHHHKH",
    "bankCode": "012",
    "branchCode": "456",
    "bankName": "China CITIC Bank International Limited",
    "bankCountryCode": "HK",
    "bankAddress": "8 Finance Street, Central, Hong Kong",
    "sortCode": "123456",
    "beneficiaryRoutingCode": "123123456",
    "beneficiaryAccountNo": "123123456789",
    "beneficiaryName": "XIAO HONG",
    "beneficiaryEntityType": "individual",
    "beneficiaryCompanyName": "Xiaohong Technology Co., Ltd.",
    "beneficiaryFirstName": "XIAO",
    "beneficiaryLastName": "HONG",
    "beneficiaryCountryCode": "HK",
    "beneficiaryStreet": "8 Finance Street",
    "beneficiaryCity": "Central",
    "beneficiaryState": "Hong Kong",
    "beneficiaryPostalCode": "999077",
    "beneficiaryIdNumber": "231010199707010101",
    "beneficiaryPhoneNumber": "15800001010",
    "conetId": 1009213,
    "note": "小白",
    "label": "zhangsan",
    "superOrgRecipientId": "45"
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

### 加密货币创建收款方请求（需要用户授权）

**URL:** /api/v2/recipient/crypto/create

**Type:** POST

**Content-Type:** application/json

**Description:** 加密货币创建收款方请求（需要用户授权）

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

### 加密货币直接创建收款方请求

**URL:** /api/v2/recipient/crypto/create/direct

**Type:** POST

**Content-Type:** application/json

**Description:** 加密货币直接创建收款方请求

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
curl -X POST -H 'Content-Type: application/json' -i /api/v2/recipient/crypto/create/direct --data '{
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
|└─superOrgRecipientId|string|收款人来源id|-|
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
      "label": "小红的地址",
      "superOrgRecipientId": "45",
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
|└─superOrgRecipientId|string|收款人来源id|-|
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
    "label": "小红的地址",
    "superOrgRecipientId": "45",
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

### 创建交易加密交易（需要用户授权）

**URL:** /api/v2/transaction/crypto/create

**Type:** POST

**Content-Type:** application/json

**Description:** 创建交易加密交易（需要用户授权）

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

### 直接创建交易加密交易

**URL:** /api/v2/transaction/crypto/create/direct

**Type:** POST

**Content-Type:** application/json

**Description:** 直接创建交易加密交易

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
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/crypto/create/direct --data '{
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

### 创建一个法币转账（需要用户授权）
**URL:** /api/v2/transaction/fiat/create

**Type:** POST


**Content-Type:** application/json

**Description:** 创建一个法币转账（需要用户授权）

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
|additionalFee|string|false|附加手续费（转账手续费=平台手续费+附加手续费）|-|

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
      "objectKey": "pq02dk"
    }
  ],
  "note": "w9tred",
  "additionalFee": "0.1"
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

### 直接创建一个法币转账
**URL:** /api/v2/transaction/fiat/create/direct

**Type:** POST


**Content-Type:** application/json

**Description:** 直接创建一个法币转账

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
|additionalFee|string|false|附加手续费（转账手续费=平台手续费+附加手续费）|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/fiat/create/direct --data '{
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
      "objectKey": "1v0m4v"
    }
  ],
  "note": "91iyz3",
  "additionalFee": "0.1"
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
|└─additionalFee|string|附加手续费（转账手续费=平台手续费+附加手续费）|-|
|└─note|string|备注|-|
|└─beneficiaryId|int64|收款人ID|-|
|└─fiatFeeMethod|int32|法币手续费方式（1：支付本地银行服务费；2：支付本地银行服务费与收款行服务费；）|-|
|└─channelKey|string|[转账通道](#channelKey)|-|
|└─subChannelKey|string|[转账子通道](#channelKey)，当转账方式为“local”时，需要指定子类型|-|
|└─proofEn|string|需要上传凭证的英文说明|-|
|└─proofCn|string|需要上传凭证的中文说明|-|
|└─cryptoBlockHeight|int64|数字货币区块高度|-|
|└─cryptoFromAddress|string|数字货币交易来源地址|-|
|└─cryptoToAddress|string|数字货币交易目标地址|-|
|└─cryptoTxHash|string|数字货币交易hash|-|
|└─cryptoTxFee|string|数字货币链上手续费|-|
|└─hasTransferNotice|boolean|是否可下载转账凭证|-|
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
      "transactionSubStatus": "x6bn9g",
      "platformFee": "1.2",
      "additionalFee": "0.2",
      "note": "差旅费",
      "beneficiaryId": 123,
      "fiatFeeMethod": 1,
      "channelKey": "swift",
      "subChannelKey": "chats",
      "proofEn": "xxaiwe",
      "proofCn": "yg0do6",
      "cryptoBlockHeight": 8371443,
      "cryptoFromAddress": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
      "cryptoToAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93",
      "cryptoTxHash": "0xf9eb33e7edae658035075e7b3dab09d15d7eb2ee20a4e84e0984a3f9d55ccc40",
      "cryptoTxFee": "1.2",
      "hasTransferNotice": true
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
|└─additionalFee|string|附加手续费（转账手续费=平台手续费+附加手续费）|-|
|└─note|string|备注|-|
|└─beneficiaryId|int64|收款人ID|-|
|└─fiatFeeMethod|int32|法币手续费方式（1：支付本地银行服务费；2：支付本地银行服务费与收款行服务费；）|-|
|└─channelKey|string|[转账通道](#channelKey)|-|
|└─subChannelKey|string|[转账子通道](#channelKey)，当转账方式为“local”时，需要指定子类型|-|
|└─proofEn|string|需要上传凭证的英文说明|-|
|└─proofCn|string|需要上传凭证的中文说明|-|
|└─cryptoBlockHeight|int64|数字货币区块高度|-|
|└─cryptoFromAddress|string|数字货币交易来源地址|-|
|└─cryptoToAddress|string|数字货币交易目标地址|-|
|└─cryptoTxHash|string|数字货币交易hash|-|
|└─cryptoTxFee|string|数字货币链上手续费|-|
|└─hasTransferNotice|boolean|是否可下载转账凭证|-|
|└─payBankName|string|付款方银行名|-|
|└─payBankAddress|string|付款方银行地址|-|
|└─payAccountNo|string|付款方账号|-|
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
    "transactionSubStatus": "6emop0",
    "platformFee": "1.2",
    "additionalFee": "0.2",
    "note": "差旅费",
    "beneficiaryId": 123,
    "fiatFeeMethod": 1,
    "channelKey": "swift",
    "subChannelKey": "chats",
    "proofEn": "5aloif",
    "proofCn": "6ae3nc",
    "cryptoBlockHeight": 8371443,
    "cryptoFromAddress": "0x2B2711eADBb960f99221BF795EDFdc036798822D",
    "cryptoToAddress": "0xfDb1FC3Ff8479bA88D4Ee44fF5Dbf8BB904a0E93",
    "cryptoTxHash": "0xf9eb33e7edae658035075e7b3dab09d15d7eb2ee20a4e84e0984a3f9d55ccc40",
    "cryptoTxFee": "1.2",
    "hasTransferNotice": true,
    "payBankName": "BISON BANK, S.A.",
    "payBankAddress": "Rua Barata Salgueiro,Lisbon, Portugal",
    "payAccountNo": "PT1009192310001"
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

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|transferCurrencyKey|string|true|转账币种唯一标识|-|
|transferAmount|string|true|转账金额|-|
|feeMethod|int32|true|手续费方式（1:支付本地银行服务费；2:支付本地银行服务费与收款行服务费；）|-|
|channelKey|string|false|法币-[转账通道](#channelKey)|-|
|recipientId|int64|true|收款方ID|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/fiat/fee --data '{
  "clientId": "1663027675055698121",
  "transferCurrencyKey": "USD",
  "transferAmount": "100",
  "feeMethod": 1,
  "channelKey": "swift",
  "recipientId": 140
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

**Description:** 查询加密货币交易预估手续费,必须是用户账户已经存在的币种

**Body-parameters:**

| Parameter   | Type | Required | Description                    | Since |
|-------------|------|----------|--------------------------------|-------|
| clientId    |string| true     | 客户的账户ID                        |-|
| currencyKey |string| true     | 币种唯一标识                         |-|
| amount      |string| true     | 交易金额                           |-|
| recipientId |string| false    | 收款方ID,和address任选其一,优先address   |-|
| address     |string| false    | 收款地址,和recipientId任选其一,优先address |-|

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
|objectKeyList|array|true| 上传后的对象key列表 |-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/proof/add --data '{
  "clientId": "1663027675055698121",
  "transactionNo": "1663027675055698131",
  "objectKey": ["clearones-fiat-test/tt/1.zip", "clearones-fiat-test/tt/2.zip"]
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

### 获取转账凭证下载地址

**URL:** /api/v2/transaction/transferNotice/download

**Type:** POST

**Content-Type:** application/json

**Description:** 获取转账凭证下载地址

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|transactionNo|string|true|交易号|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/transaction/transferNotice/download --data '{
  "clientId": "1663027675055698121",
  "transactionNo": "1663027675055698130"
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─path|string|临时下载地址，有效期 1 天|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "path": "https://clearones-prod.s3.ap-east-1.amazonaws.com/ACCOUNT_CREATE_CONET/59d2114dc6594c81bd61a11cea18cc54.jpg"
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
授权验证接口就是在调用写操作接口(1:加密货币提币；2:法币转账；3:加密货币添加收款地址；4:法币添加收款人；5:连接账号提币；6:
FX创建交易；)后续需要验证该操作并真正执行上述接口操作的接口

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|authorizationType|int32|true|授权类型（1:加密货币提币；2:法币转账；3:加密货币添加收款地址；4:法币添加收款人；5:连接账号提币；6:FX创建交易；）|-|
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
|toAddress|string|false|提币目标地址（当blockchainKey是solana时，必传，其他传了计算预估手续费会更精确）|-|

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

### 创建交易（需要用户授权）

**URL:** /api/v2/connect/transaction/create

**Type:** POST

**Content-Type:** application/json

**Description:** 创建交易（需要用户授权）

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

### 直接创建交易

**URL:** /api/v2/connect/transaction/create/direct

**Type:** POST

**Content-Type:** application/json

**Description:** 直接创建交易

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
curl -X POST -H 'Content-Type: application/json' -i /api/v2/connect/transaction/create/direct --data '{
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
|└─customerRefId|string|调用方唯一业务ID|-|
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
      "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
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
|└─customerRefId|string|调用方唯一业务ID|-|
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
    "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
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

## FX兑换业务

### 查询用户FX交易对列表

**URL:** /api/v2/fx/client/pair/list

**Type:** POST

**Content-Type:** application/json

**Description:** 查询用户FX交易对列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|参数使用空字符串|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fx/client/pair/list --data '801hmr'
```

**Response-fields:**

| Field | Type   | Description | Since |
|-------|--------|-------------|-------|
|code| int32  |响应码|-|
|message| string |响应描述|-|
|data| object |响应数据|-|
|└─weeklyLimitUsd| number |周交易限额（单位：USD），统计范围：按照东八区本周周一至周日，toCurrencyKey兑换数量按照兑换时汇率折算为USD进行统计|-|
|└─pairs| array  |交易对列表|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─fromCurrencyKey| string |付款币种Key|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─toCurrencyKey| string |收款币种Key|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─exchangeRate| string |兑换汇率|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─feeRate| string |手续费费率|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─minAmount| string |最小兑换fromCurrencyKey数量|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─maxAmount| string |最大兑换fromCurrencyKey数量|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─thresholdAmount| string |高手续费阈值金额(fromAmount < thresholdAmount时，手续费率使用thresholdFeeRate)|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─thresholdFeeRate| string |高手续费率|-|
|timestamp| string |时间戳毫秒|-|
|key| string |加密key|-|
|sign| string |签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "weeklyLimitUsd": 5000,
    "pairs": [
      {
        "fromCurrencyKey": "USD",
        "toCurrencyKey": "USDT_TRC20",
        "exchangeRate": 0.98,
        "feeRate": 0.001,
        "minAmount": 100,
        "maxAmount": 100000
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询FX交易列表

**URL:** /api/v2/fx/transaction/list

**Type:** POST

**Content-Type:** application/json

**Description:** 查询FX交易列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|false|客户的账户ID|-|
|fromNo|string|false|查询开始transactionNo|-|
|limit|int32|false|查询数量，默认20，最大100|-|
|fromCurrencyKey|string|false|付款币种Key|-|
|toCurrencyKey|string|false|收款币种Key|-|
|createTimestampFrom|int64|false|创建时间开始时间戳，UNIX 时间戳毫秒数|-|
|createTimestampTo|int64|false|创建时间结束时间戳，UNIX 时间戳毫秒数|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fx/transaction/list --data '{
  "clientId": "1663027675055698121",
  "fromNo": "1663027675055698130",
  "limit": 20,
  "fromCurrencyKey": "USD",
  "toCurrencyKey": "USDT_TRC20",
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
|└─createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|└─completedTimestamp|int64|完成时间，UNIX 时间戳毫秒数|-|
|└─transactionNo|string|交易号|-|
|└─clientId|string|客户的账户ID|-|
|└─transactionStatus|string|交易状态<br/><br/><pre><br/>SUBMITTED：已提交，此状态时用户账户已经冻结交易金额<br/>PROCESSING：进行中，此状态时用户账户已发起转账，等待交易完成<br/>SUCCESS：成功，用户转账完成并且已收到对应币种<br/>FAILED：失败，失败原因有用户转账失败、用户收款失败等。如用户转账完成后失败，会退款给用户<br/></pre>|-|
|└─fromCurrencyKey|string|付款币种Key|-|
|└─toCurrencyKey|string|收款币种Key|-|
|└─exchangeRate|string|兑换汇率|-|
|└─fromAmount|string|付款币种数量|-|
|└─toAmount|string|收款币种数量|-|
|└─feeRate|string|总服务费费率（平台服务费费率+附加服务费费率）|-|
|└─fee|string|总服务费|-|
|└─feeCurrencyKey|string|服务费币种Key|-|
|└─additionalFeeRate|string|附加服务费费率|-|
|└─additionalFee|string|附加服务费|-|
|└─transferNo|string|转账交易号|-|
|└─receiveNo|string|收款交易号|-|
|└─refundNo|string|退款交易号|-|
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
      "transactionStatus": "SUCCESS",
      "fromCurrencyKey": "USD",
      "toCurrencyKey": "USDT_TRC20",
      "exchangeRate": "0.98",
      "fromAmount": "1000",
      "toAmount": "978.04",
      "feeRate": "0.001",
      "fee": "2",
      "feeCurrencyKey": "USD",
      "additionalFeeRate": "0.001",
      "additionalFee": "1",
      "transferNo": "1803342430657843206",
      "receiveNo": "1803342430657843207",
      "refundNo": "1803342430657843208"
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询FX交易详情

**URL:** /api/v2/fx/transaction/detail

**Type:** POST

**Content-Type:** application/json

**Description:** 查询FX交易详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|false|调用方唯一业务ID，与参数transactionNo二选一必填，如果两个都有值，将按照两个参数查询|-|
|transactionNo|string|false|交易号，与参数customerRefId二选一必填，如果两个都有值，将按照两个参数查询|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fx/transaction/detail --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "transactionNo": "1663027675055698130"
}'
```

**Response-fields:**

| Field | Type   | Description | Since |
|-------|--------|-------------|-------|
|code| int32  |响应码|-|
|message| string |响应描述|-|
|data| object |响应数据|-|
|└─customerRefId| string |调用方唯一业务ID|-|
|└─createTimestamp| int64  |创建时间，UNIX 时间戳毫秒数|-|
|└─completedTimestamp| int64  |完成时间，UNIX 时间戳毫秒数|-|
|└─transactionNo| string |交易号|-|
|└─clientId| string |客户的账户ID|-|
|└─transactionStatus| string |交易状态<br/><br/><pre><br/>SUBMITTED：已提交，此状态时用户账户已经冻结交易金额<br/>PROCESSING：进行中，此状态时用户账户已发起转账，等待交易完成<br/>SUCCESS：成功，用户转账完成并且已收到对应币种<br/>FAILED：失败，失败原因有用户转账失败、用户收款失败等。如用户转账完成后失败，会退款给用户<br/></pre>|-|
|└─fromCurrencyKey| string |付款币种Key|-|
|└─toCurrencyKey| string |收款币种Key|-|
|└─exchangeRate| string |兑换汇率|-|
|└─fromAmount| string |付款币种数量|-|
|└─toAmount| string |收款币种数量|-|
|└─feeRate| string |总服务费费率（平台服务费费率+附加服务费费率）|-|
|└─fee| string |总服务费|-|
|└─feeCurrencyKey| string |服务费币种Key|-|
|└─additionalFeeRate| string |附加服务费费率|-|
|└─additionalFee| string |附加服务费|-|
|└─transferNo| string |转账交易号|-|
|└─receiveNo| string |收款交易号|-|
|└─refundNo| string |退款交易号|-|
|timestamp| string |时间戳毫秒|-|
|key| string |加密key|-|
|sign| string |签名|-|

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
    "transactionStatus": "SUCCESS",
    "fromCurrencyKey": "USD",
    "toCurrencyKey": "USDT_TRC20",
    "exchangeRate": "0.98",
    "fromAmount": "1000",
    "toAmount": "978.04",
    "feeRate": "0.001",
    "fee": "2",
    "feeCurrencyKey": "USD",
    "additionalFeeRate": "0.001",
    "additionalFee": "1",
    "transferNo": "1803342430657843206",
    "receiveNo": "1803342430657843207",
    "refundNo": "1803342430657843208"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 创建FX交易（需要用户授权）

**URL:** /api/v2/fx/transaction/create

**Type:** POST

**Content-Type:** application/json

**Description:** 创建FX交易（需要用户授权）

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id，最长 100|-|
|fromCurrencyKey|string|true|付款币种Key|-|
|toCurrencyKey|string|true|收款币种Key|-|
|fromAmount|string|true|付款币种数量|-|
|additionalFeeRate|string|false|附加服务费费率|-|
|exchangeRate|string|false|使用的汇率|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fx/transaction/create --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "fromCurrencyKey": "USD",
  "toCurrencyKey": "USDT_TRC20",
  "fromAmount": "1000",
  "additionalFeeRate": "0.001",
  "exchangeRate": "7.7832"
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

### 直接创建FX交易

**URL:** /api/v2/fx/transaction/create/direct

**Type:** POST

**Content-Type:** application/json

**Description:** 直接创建FX交易

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|customerRefId|string|true|调用方唯一业务id，最长 100|-|
|fromCurrencyKey|string|true|付款币种Key|-|
|toCurrencyKey|string|true|收款币种Key|-|
|fromAmount|string|true|付款币种数量|-|
|additionalFeeRate|string|false|附加服务费费率|-|
|exchangeRate|string|false|使用的汇率|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fx/transaction/create/direct --data '{
  "clientId": "1663027675055698121",
  "customerRefId": "53d73bed-0a15-4ef6-95f6-9e73304e6d7d",
  "fromCurrencyKey": "USD",
  "toCurrencyKey": "USDT_TRC20",
  "fromAmount": "1000",
  "additionalFeeRate": "0.001",
  "exchangeRate": "7.7832"
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

### 预算FX交易

**URL:** /api/v2/fx/transaction/check

**Type:** POST

**Content-Type:** application/json

**Description:** 预算FX交易

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|clientId|string|true|客户的账户ID|-|
|fromCurrencyKey|string|true|付款币种Key|-|
|toCurrencyKey|string|true|收款币种Key|-|
|fromAmount|string|true|付款币种数量|-|
|additionalFeeRate|string|false|附加服务费费率|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/fx/transaction/check --data '{
  "clientId": "1663027675055698121",
  "fromCurrencyKey": "USD",
  "toCurrencyKey": "USDT_TRC20",
  "fromAmount": "1000",
  "additionalFeeRate": "0.001"
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─fromCurrencyKey|string|付款币种Key|-|
|└─toCurrencyKey|string|收款币种Key|-|
|└─fromAmount|string|from币种数量|-|
|└─toAmount|string|to币种数量|-|
|└─exchangeRate|string|交易汇率|-|
|└─feeRate|string|手续费费率|-|
|└─fee|string|手续费数量|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "fromCurrencyKey": "USD",
    "toCurrencyKey": "HKD",
    "fromAmount": "10000",
    "toAmount": "77454.93",
    "exchangeRate": "7.7688",
    "feeRate": "0.003",
    "fee": "30"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 卡业务

### 机构账户信息

**URL:** /api/v2/build/card/merchant/info

**Type:** POST

**Content-Type:** application/json

**Description:** 机构账户信息

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|empty|string|false|No comments found.|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/merchant/info --data '5uc7bs'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─depositFee|number|充值手续费率|-|
|└─cardAuthUrl|string|卡片消费认证回调接口|-|
|└─cardTdsUrl|string|卡片3DS回调接口|-|
|└─assets|array|资金情况|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─currencyKey|string|币种|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─availableBalance|number|可用余额|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─frozenBalance|number|冻结余额|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardCurrencyStatus|int32|卡币种状态 1-可用 2-停用|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "depositFee": 0.004,
    "cardAuthUrl": "https://test.quantumtrust.com/card/callback/auth",
    "cardAuthUrl": "https://test.quantumtrust.com/card/callback/3ds",
    "assets": [
      {
        "currencyKey": "USD",
        "availableBalance": 8130.3,
        "frozenBalance": 200.13,
        "cardCurrencyStatus": 1
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 机构账户充值

**URL:** /api/v2/build/card/merchant/deposit

**Type:** POST

**Content-Type:** application/json

**Description:** 机构账户充值

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|currencyKey|string|true|交易号|-|
|amount|number|true|交易号|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/merchant/deposit --data '{
  "currencyKey": "USD",
  "amount": 200.1
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─transactionNo|string|充值订单号|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionNo": "1727595385687322624"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 机构账户充值记录

**URL:** /api/v2/build/card/merchant/deposit/list

**Type:** POST

**Content-Type:** application/json

**Description:** 机构账户充值记录

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|fromNo|string|false|查询开始transactionNo|-|
|currencyKey|string|false|币种|-|
|limit|int32|false|查询数量，默认20，最大100|-|
|createTimestampFrom|int64|false|创建时间开始时间戳|-|
|createTimestampTo|int64|false|创建时间开始时间戳|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/merchant/deposit/list --data '{
  "fromNo": "1663027675055698130",
  "currencyKey": "USD",
  "limit": 30,
  "createTimestampFrom": 1672056033898,
  "createTimestampTo": 1772056033898
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|array|响应数据|-|
|└─transactionNo|string|充值订单号|-|
|└─currencyKey|string|币种|-|
|└─amount|number|充值金额(实际到账，扣减手续费后的)|-|
|└─fee|number|手续费|-|
|└─createTimestamp|int64|创建时间|-|
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
      "transactionNo": "1727595385687322624",
      "currencyKey": "USD",
      "amount": 490,
      "fee": 10,
      "createTimestamp": 1672056033898
    }
  ],
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 创建用户

**URL:** /api/v2/build/card/user/create

**Type:** POST

**Content-Type:** application/json

**Description:** 创建用户

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|individual|object|false|个人信息|-|
|└─gender|string|false|性别 Allowed: M,F,X|-|
|└─address|object|true|地址信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|true|国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state|string|true|地址所在州、省、县或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postalCode|string|true|邮政编码或邮政编码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city|string|true|城市或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1|string|true|地址第一行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2|string|false|地址第二行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3|string|false|地址第三行|-|
|└─document|object|false|No comments found.|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid|string|false|uid|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─issuer|string|false|证件发行国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─nationality|string|false|文档国籍ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentType|string|false|文档类型 Allowed: NONE,PASSPORT,DRIVERS_LICENSE,NATIONAL_ID,SOCIAL_SECURITY_NUMBER,GREEN_CARD,VISA,MATRICULA_CONSULAR,<br/>REGISTRO_FEDERAL_DE_CONTRIBUYENTES,CREDENTIAL_DE_ELECTOR,SOCIAL_INSURANCE_NUMBER,CITIZENSHIP_PAPERS,<br/>DRIVERS_LICENSE_CANADIAN,EXISTING_CREDIT_CARD_DETAILS,EMPLOYER_IDENTIFICATION_NUMBER,INCORPORATION_NUMBER,OTHERS|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentNumber|string|false|证件号码 2-128|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─issueDate|string|false|文件签发者日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─expirationDate|string|false|文档到期日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentAddress|string|false|文档地址|-|
|└─taxes|array|false|No comments found.|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─tax|string|false|税号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|false|税国家ISO|-|
|└─firstName|string|true|名|-|
|└─lastName|string|true|姓|-|
|└─middleName|string|false|No comments found.|-|
|└─mobile|string|true|手机号码。最长为 30 个字符（包括国家代码）。电话号码必须采用国际区号 + '-' + 电话号码的格式。|-|
|└─email|string|true|电子邮件，限制为 255 个字符|-|
|└─dateOfBirth|string|true|出生日期，格式为 YYYY-MM-DD|-|
|└─phoneNumber|string|false|电话|-|
|└─locale|string|false|地区代码|-|
|└─timezone|string|false|时区|-|
|company|object|false|企业信息|-|
|└─email|string|true|邮箱地址|-|
|└─industry|string|false|企业所属行业 1-200字符|-|
|└─address|object|true||-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|true|国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state|string|true|地址所在州、省、县或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postalCode|string|true|邮政编码或邮政编码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city|string|true|城市或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1|string|true|地址第一行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2|string|false|地址第二行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3|string|false|地址第三行|-|
|└─contact|object|true||-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─gender|string|true|联系人的性别 Allowed: M,F,X|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName|string|true|名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName|string|true|姓|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─middleName|string|false|中间名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─mobile|string|true|手机号码。最长为 30 个字符（包括国家代码）。电话号码必须采用国际区号 + '-' + 电话号码的格式。|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─dateOfBirth|string|true|出生日期，格式为 YYYY-MM-DD|-|
|└─website|string|true|公司网址|-|
|└─companyName|string|true|公司名|-|
|└─phoneNumber|string|true|公司电话号码|-|
|└─locale|string|false|地区代码|-|
|└─timezone|string|false|时区|-|
|└─registrationNumber|string|true|公司注册编号|-|
|metadata|map|false|以key-value形式存储用户元数据信息，Key长度必须小于等于50，Value长度必须小于等于50|-|
|accountName|string|true|账户名 1-32长度|-|
|userReference|string|true|用户参考码，防重|-|
|legalEntityType|string|true|用户类型:INDIVIDUAL,COMPANY|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/user/create --data '{
  "individual": {
    "gender": "M",
    "address": {
      "countryCode": "US",
      "state": "CA",
      "postalCode": "100001",
      "city": "Hollywood",
      "addressLine1": "cheer Building",
      "addressLine2": "Chifley Tower",
      "addressLine3": "2 Square"
    },
    "document": {
      "uid": "17818239123",
      "issuer": "US",
      "nationality": "US",
      "documentType": "CREDENTIAL_DE_ELECTOR",
      "documentNumber": "91923",
      "issueDate": "2019-10-11",
      "expirationDate": "2029-10-10",
      "documentAddress": "1t-street"
    },
    "taxes": [
      {
        "tax": "123123",
        "countryCode": "US"
      }
    ],
    "firstName": "Mike",
    "lastName": "Lee",
    "middleName": "arlene.wiegand",
    "mobile": "+61-414555555",
    "email": "jet@test.com",
    "dateOfBirth": "1971-09-21",
    "phoneNumber": "+61-414555555",
    "locale": "en-US",
    "timezone": "America/New_York"
  },
  "company": {
    "email": "jet@test.com",
    "industry": "Computer",
    "address": {
      "countryCode": "US",
      "state": "CA",
      "postalCode": "100001",
      "city": "Hollywood",
      "addressLine1": "cheer Building",
      "addressLine2": "Chifley Tower",
      "addressLine3": "2 Square"
    },
    "contact": {
      "gender": "M",
      "firstName": "Mike",
      "lastName": "Lee",
      "middleName": "G",
      "mobile": "+61-414555555",
      "dateOfBirth": "1971-09-21"
    },
    "website": "www.clearones.com",
    "companyName": "Quantum Trust",
    "phoneNumber": "+61-414555555",
    "locale": "en-US",
    "timezone": "America/New_York",
    "registrationNumber": "8819291923"
  },
  "metadata": {
    "mapKey": "e2n4ef"
  },
  "accountName": "qt trust",
  "userReference": "1929391923",
  "legalEntityType": "COMPANY"
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─id|string|记录id|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "id": "ETLPzgGfSzDtuPNfeTMQqhonq"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 修改用户

**URL:** /api/v2/build/card/user/update

**Type:** POST

**Content-Type:** application/json

**Description:** 修改用户

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|individual|object|false|个人信息|-|
|└─gender|string|false|性别 Allowed: M,F,X|-|
|└─address|object|true|地址信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|true|国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state|string|true|地址所在州、省、县或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postalCode|string|true|邮政编码或邮政编码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city|string|true|城市或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1|string|true|地址第一行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2|string|false|地址第二行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3|string|false|地址第三行|-|
|└─document|object|false|No comments found.|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid|string|false|uid|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─issuer|string|false|证件发行国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─nationality|string|false|文档国籍ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentType|string|false|文档类型 Allowed: NONE┃PASSPORT┃DRIVERS_LICENSE┃NATIONAL_ID┃SOCIAL_SECURITY_NUMBER┃GREEN_CARD┃VISA┃MATRICULA_CONSULAR<br/>┃REGISTRO_FEDERAL_DE_CONTRIBUYENTES┃CREDENTIAL_DE_ELECTOR┃SOCIAL_INSURANCE_NUMBER┃CITIZENSHIP_PAPERS<br/>┃DRIVERS_LICENSE_CANADIAN┃EXISTING_CREDIT_CARD_DETAILS┃EMPLOYER_IDENTIFICATION_NUMBER┃INCORPORATION_NUMBER┃OTHERS|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentNumber|string|false|证件号码 2-128|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─issueDate|string|false|文件签发者日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─expirationDate|string|false|文档到期日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentAddress|string|false|文档地址|-|
|└─taxes|array|false|No comments found.|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─tax|string|false|税号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|false|税国家ISO|-|
|└─firstName|string|true|名|-|
|└─lastName|string|true|姓|-|
|└─middleName|string|false|No comments found.|-|
|└─mobile|string|true|手机号码。最长为 30 个字符（包括国家代码）。电话号码必须采用国际区号 + '-' + 电话号码的格式。|-|
|└─email|string|true|电子邮件，限制为 255 个字符|-|
|└─dateOfBirth|string|true|出生日期，格式为 YYYY-MM-DD|-|
|└─phoneNumber|string|false|电话|-|
|└─locale|string|false|地区代码|-|
|└─timezone|string|false|时区|-|
|company|object|false|企业信息|-|
|└─email|string|true|邮箱地址|-|
|└─industry|string|false|企业所属行业 1-200字符|-|
|└─address|object|true||-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|true|国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state|string|true|地址所在州、省、县或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postalCode|string|true|邮政编码或邮政编码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city|string|true|城市或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1|string|true|地址第一行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2|string|false|地址第二行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3|string|false|地址第三行|-|
|└─contact|object|true||-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─gender|string|true|联系人的性别 Allowed: M,F,X|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName|string|true|名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName|string|true|姓|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─middleName|string|false|中间名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─mobile|string|true|手机号码。最长为 30 个字符（包括国家代码）。电话号码必须采用国际区号 + '-' + 电话号码的格式。|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─dateOfBirth|string|true|出生日期，格式为 YYYY-MM-DD|-|
|└─website|string|true|公司网址|-|
|└─companyName|string|true|公司名|-|
|└─phoneNumber|string|true|公司电话号码|-|
|└─locale|string|false|地区代码|-|
|└─timezone|string|false|时区|-|
|└─registrationNumber|string|true|公司注册编号|-|
|metadata|map|false|以key-value形式存储用户元数据信息，Key长度必须小于等于50，Value长度必须小于等于50|-|
|accountName|string|true|账户名 1-32长度|-|
|userReference|string|true|用户参考码，防重|-|
|legalEntityType|string|true|用户类型:INDIVIDUAL,COMPANY|-|
|id|string|true|用户id|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/user/update --data '{
  "individual": {
    "gender": "M",
    "address": {
      "countryCode": "US",
      "state": "CA",
      "postalCode": "100001",
      "city": "Hollywood",
      "addressLine1": "cheer Building",
      "addressLine2": "Chifley Tower",
      "addressLine3": "2 Square"
    },
    "document": {
      "uid": "17818239123",
      "issuer": "US",
      "nationality": "US",
      "documentType": "CREDENTIAL_DE_ELECTOR",
      "documentNumber": "91923",
      "issueDate": "2019-10-11",
      "expirationDate": "2029-10-10",
      "documentAddress": "1t-street"
    },
    "taxes": [
      {
        "tax": "123123",
        "countryCode": "US"
      }
    ],
    "firstName": "Mike",
    "lastName": "Lee",
    "middleName": "arlene.wiegand",
    "mobile": "+61-414555555",
    "email": "jet@test.com",
    "dateOfBirth": "1971-09-21",
    "phoneNumber": "+81-8918321",
    "locale": "en-US",
    "timezone": "America/New_York"
  },
  "company": {
    "email": "jet@test.com",
    "industry": "E-Tech",
    "address": {
      "countryCode": "US",
      "state": "CA",
      "postalCode": "100001",
      "city": "Hollywood",
      "addressLine1": "cheer Building",
      "addressLine2": "Chifley Tower",
      "addressLine3": "2 Square"
    },
    "contact": {
      "gender": "M",
      "firstName": "Mike",
      "lastName": "Lee",
      "middleName": "G",
      "mobile": "+61-414555555",
      "dateOfBirth": "1971-09-21"
    },
    "website": "https://www.clearones.com",
    "companyName": "Quantum Trust",
    "phoneNumber": "+71-87123123",
    "locale": "en-US",
    "timezone": "America/New_York",
    "registrationNumber": "8819291923"
  },
  "metadata": {
    "mapKey": "t7qxqb"
  },
  "accountName": "qt trust",
  "userReference": "1929391923",
  "legalEntityType": "COMPANY",
  "id": "18719918238131"
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─id|string|记录id|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "id": "ETLPzgGfSzDtuPNfeTMQqhonq"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 用户列表

**URL:** /api/v2/build/card/user/list

**Type:** POST

**Content-Type:** application/json

**Description:** 用户列表

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|pageNo|int64|false|No comments found.|-|
|pageSize|int64|false|No comments found.|-|
|firstName|string|false|名|-|
|lastName|string|false|姓|-|
|type|string|false|类型INDIVIDUAL:个人 COMPANY:公司|-|
|status|string|false|用户状态 SUBMITTED,ENABLED,DISABLED,FROZEN,CLOSED,UNKNOWN|-|
|createTimestampFrom|int64|false|创建时间开始时间戳|-|
|createTimestampTo|int64|false|创建时间开始时间戳|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/user/list --data '{
  "pageNo": 211,
  "pageSize": 130,
  "firstName": "Bob",
  "lastName": "Johnson",
  "type": "INDIVIDUAL",
  "status": "ENABLED",
  "createTimestampFrom": 1672056033898,
  "createTimestampTo": 1772056033898
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─totalCount|int64|Total record count|-|
|└─pageSize|int64|Page size|-|
|└─totalPage|int64|Total pages|-|
|└─pageNo|int64|Current page|-|
|└─data|array|data records|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─id|string|build关联的用户uid|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName|string|姓|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName|string|名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─accountName|string|账号名称|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─email|string|邮箱地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─type|string|类型INDIVIDUAL:个人 COMPANY:公司|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─reference|string|客户唯一标识|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─status|string|用户状态 SUBMITTED,ENABLED,DISABLED,FROZEN,CLOSED,UNKNOWN|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createTimestamp|int64|创建时间戳|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "totalCount": 291,
    "pageSize": 144,
    "totalPage": 491,
    "pageNo": 475,
    "data": [
      {
        "id": "18182731231",
        "firstName": "Bob",
        "lastName": "Johnson",
        "accountName": "Bob Kevin",
        "email": "test@example.com",
        "type": "INDIVIDUAL",
        "reference": "mdaeriqfadf",
        "status": "ENABLED",
        "createTimestamp": 1672056033898
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 用户详情

**URL:** /api/v2/build/card/user/detail

**Type:** POST

**Content-Type:** application/json

**Description:** 用户详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|id|string|true|记录id|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/user/detail --data '{
  "id": "81823"
}'
```

**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─individual|object|个人信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─gender|string|性别 Allowed: M,F,X|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─address|object|地址信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state|string|地址所在州、省、县或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postalCode|string|邮政编码或邮政编码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city|string|城市或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1|string|地址第一行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2|string|地址第二行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3|string|地址第三行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─document|object|No comments found.|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid|string|uid|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─issuer|string|证件发行国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─nationality|string|文档国籍ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentType|string|文档类型 Allowed: NONE,PASSPORT,DRIVERS_LICENSE,NATIONAL_ID,SOCIAL_SECURITY_NUMBER,GREEN_CARD,VISA,MATRICULA_CONSULAR,<br/>REGISTRO_FEDERAL_DE_CONTRIBUYENTES,CREDENTIAL_DE_ELECTOR,SOCIAL_INSURANCE_NUMBER,CITIZENSHIP_PAPERS,<br/>DRIVERS_LICENSE_CANADIAN,EXISTING_CREDIT_CARD_DETAILS,EMPLOYER_IDENTIFICATION_NUMBER,INCORPORATION_NUMBER,OTHERS|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentNumber|string|证件号码 2-128|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─issueDate|string|文件签发者日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─expirationDate|string|文档到期日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─documentAddress|string|文档地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─taxes|array|No comments found.|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─tax|string|税号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|税国家ISO|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName|string|名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName|string|姓|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─middleName|string|No comments found.|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─mobile|string|手机号码。最长为 30 个字符（包括国家代码）。电话号码必须采用国际区号 + '-' + 电话号码的格式。|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─email|string|电子邮件，限制为 255 个字符|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─dateOfBirth|string|出生日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneNumber|string|电话|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─locale|string|地区代码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─timezone|string|时区|-|
|└─company|object|企业信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─email|string|邮箱地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─industry|string|企业所属行业 1-200字符|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─address|object||-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─countryCode|string|国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state|string|地址所在州、省、县或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postalCode|string|邮政编码或邮政编码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city|string|城市或地区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1|string|地址第一行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2|string|地址第二行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3|string|地址第三行|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─contact|object||-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─gender|string|联系人的性别 Allowed: M,F,X|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName|string|名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName|string|姓|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─middleName|string|中间名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─mobile|string|手机号码。最长为 30 个字符（包括国家代码）。电话号码必须采用国际区号 + '-' + 电话号码的格式。|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─dateOfBirth|string|出生日期，格式为 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─website|string|公司网址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─companyName|string|公司名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneNumber|string|公司电话号码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─locale|string|地区代码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─timezone|string|时区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─registrationNumber|string|公司注册编号|-|
|└─metadata|map|以key-value形式存储用户元数据信息，Key长度必须小于等于50，Value长度必须小于等于50|-|
|└─accountName|string|账户名 1-32长度|-|
|└─userReference|string|用户参考码，防重|-|
|└─legalEntityType|string|用户类型:INDIVIDUAL,COMPANY|-|
|└─id|string|用户id|-|
|└─shortReference|string|用户参考编号|-|
|└─status|string|用户状态 ENABLED,DISABLED,FROZEN,CLOSED,UNKNOWN|-|
|└─createdAt|string|创建时间|-|
|└─updatedAt|string|更新时间|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "individual": {
      "gender": "M",
      "address": {
        "countryCode": "US",
        "state": "CA",
        "postalCode": "100001",
        "city": "Hollywood",
        "addressLine1": "cheer Building",
        "addressLine2": "Chifley Tower",
        "addressLine3": "2 Square"
      },
      "document": {
        "uid": "17818239123",
        "issuer": "US",
        "nationality": "US",
        "documentType": "CREDENTIAL_DE_ELECTOR",
        "documentNumber": "91923",
        "issueDate": "2019-10-11",
        "expirationDate": "2029-10-10",
        "documentAddress": "1t-street"
      },
      "taxes": [
        {
          "tax": "123123",
          "countryCode": "US"
        }
      ],
      "firstName": "Mike",
      "lastName": "Lee",
      "middleName": "arlene.wiegand",
      "mobile": "+61-414555555",
      "email": "jet@test.com",
      "dateOfBirth": "1971-09-21",
      "phoneNumber": "+61-414555555",
      "locale": "en-US",
      "timezone": "America/New_York"
    },
    "company": {
      "email": "jet@test.com",
      "industry": "Computer",
      "address": {
        "countryCode": "US",
        "state": "CA",
        "postalCode": "100001",
        "city": "Hollywood",
        "addressLine1": "cheer Building",
        "addressLine2": "Chifley Tower",
        "addressLine3": "2 Square"
      },
      "contact": {
        "gender": "M",
        "firstName": "Mike",
        "lastName": "Lee",
        "middleName": "G",
        "mobile": "+61-414555555",
        "dateOfBirth": "1971-09-21"
      },
      "website": "www.clearones.com",
      "companyName": "Quantum Trust",
      "phoneNumber": "+61-414555555",
      "locale": "en-US",
      "timezone": "America/New_York",
      "registrationNumber": "8819291923"
    },
    "metadata": {
      "mapKey": "ubjvpv"
    },
    "accountName": "qt trust",
    "userReference": "1929391923",
    "legalEntityType": "COMPANY",
    "id": "1238123123",
    "shortReference": "1231123123",
    "status": "ENABLED",
    "createdAt": "1688439539546",
    "updatedAt": "1688439539546"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 创建卡

**URL:** /api/v2/build/card/create

**Type:** POST

**Content-Type:** application/json

**Description:** 创建卡

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|uid|string|true|用户id|-|
|reference|string|false|唯一标识防重|-|
|firstName|string|true|名|-|
|midName|string|false|中间名|-|
|lastName|string|true|姓|-|
|birthday|string|true|出生日期，格式为YYYY-MM-DD|-|
|phoneNo|string|true|电话号码，格式：{countryCode,0 ... 4 个字符}-{phoneNumber,0 ... 33 个字符}|-|
|email|string|true|邮箱地址|-|
|gender|string|true|性别Allowed: MALE┃FEMALE|-|
|userAddress|object|false|用户地址|-|
|└─country|string|true|国家IOS码|-|
|└─city|string|true|城市或地区|-|
|└─neighborhood|string|false|最近的建築物|-|
|└─state|string|true|地址所在州、省、县或地区|-|
|└─addressLine1|string|true|地址第一行|-|
|└─addressLine2|string|false|地址第二行|-|
|└─addressLine3|string|false|地址第三行|-|
|└─phoneticLine1|string|false||-|
|└─phoneticLine2|string|false||-|
|└─phoneticLine3|string|false||-|
|└─postcode|string|true|邮政编码或邮政编码|-|
|cardBillingAddress|object|false|邮寄账单地址|-|
|└─country|string|true|国家IOS码|-|
|└─city|string|true|城市或地区|-|
|└─neighborhood|string|false|最近的建築物|-|
|└─state|string|true|地址所在州、省、县或地区|-|
|└─addressLine1|string|true|地址第一行|-|
|└─addressLine2|string|false|地址第二行|-|
|└─addressLine3|string|false|地址第三行|-|
|└─phoneticLine1|string|false||-|
|└─phoneticLine2|string|false||-|
|└─phoneticLine3|string|false||-|
|└─postcode|string|true|邮政编码或邮政编码|-|
|cardPostAddress|object|false|卡片寄送地址|-|
|└─country|string|true|国家IOS码|-|
|└─city|string|true|城市或地区|-|
|└─neighborhood|string|false|最近的建築物|-|
|└─state|string|true|地址所在州、省、县或地区|-|
|└─addressLine1|string|true|地址第一行|-|
|└─addressLine2|string|false|地址第二行|-|
|└─addressLine3|string|false|地址第三行|-|
|└─phoneticLine1|string|false||-|
|└─phoneticLine2|string|false||-|
|└─phoneticLine3|string|false||-|
|└─postcode|string|true|邮政编码或邮政编码|-|
|cardType|string|true|卡类型 Allowed: VIRTUAL,PHY|-|
|cardNetwork|string|false|卡片网络|-|
|category|string|false|卡类别 Allowed: DEBIT_CARD,PREPAID_CARD,CREDIT_CARD|-|
|profile|string|false|申请卡简介|-|
|program|object|false|卡的配置|-|
|└─fee|string|false|卡的费用|-|
|└─segment|string|false|卡号范围|-|
|└─currency|string|true|卡的交易币种|-|
|└─material|string|false|卡片材质|-|

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/create --data '{
  "uid": "18812391293",
  "reference": "812931023123",
  "firstName": "Bob",
  "midName": "D",
  "lastName": "Josh",
  "birthday": "1970-10-11",
  "phoneNo": "+61-123456789",
  "email": "contactMe@gmail.com",
  "gender": "MALE",
  "userAddress": {
    "country": "US",
    "city": "Hollywood",
    "neighborhood": "The neighborhood",
    "state": "CA",
    "addressLine1": "cheer Building",
    "addressLine2": "Chifley Tower",
    "addressLine3": "2 Square",
    "phoneticLine1": "phoneticLine1",
    "phoneticLine2": "phoneticLine2",
    "phoneticLine3": "phoneticLine3",
    "postcode": "100001"
  },
  "cardBillingAddress": {
    "country": "US",
    "city": "Hollywood",
    "neighborhood": "The neighborhood",
    "state": "CA",
    "addressLine1": "cheer Building",
    "addressLine2": "Chifley Tower",
    "addressLine3": "2 Square",
    "phoneticLine1": "phoneticLine1",
    "phoneticLine2": "phoneticLine2",
    "phoneticLine3": "phoneticLine3",
    "postcode": "100001"
  },
  "cardPostAddress": {
    "country": "US",
    "city": "Hollywood",
    "neighborhood": "The neighborhood",
    "state": "CA",
    "addressLine1": "cheer Building",
    "addressLine2": "Chifley Tower",
    "addressLine3": "2 Square",
    "phoneticLine1": "phoneticLine1",
    "phoneticLine2": "phoneticLine2",
    "phoneticLine3": "phoneticLine3",
    "postcode": "100001"
  },
  "cardType": "VIRTUAL",
  "cardNetwork": "string",
  "category": "CREDIT_CARD",
  "profile": "test profile",
  "program": {
    "fee": "8dqpxs",
    "segment": "05eb1r",
    "currency": "USD",
    "material": "52qvmb"
  }
}'
```

**Response-fields:**

| Field           | Type | Description | Since |
|-----------------|------|-------------|-------|
| code            |int32|响应码|-|
| message         |string|响应描述|-|
| data            |object|响应数据|-|
| └─applicationId |string|申请id |-||  timestamp       |string|时间戳毫秒|-|
| key             |string|加密key|-|
| sign            |string|签名|-|

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "applicationId": "ETLPzgGfSzDtuPNfeTMQqhonq-"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 卡申请列表

**URL:** /api/v2/build/card/application/list

**Type:** POST

**Content-Type:** application/json

**Description:** 卡申请列表

**Body-parameters:**

| Parameter           | Type   | Required | Description                                       | Since |
|---------------------|--------|----------|---------------------------------------------------|-------|
| pageNo              | int64  | false    | No comments found.                                | -     |
| pageSize            | int64  | false    | No comments found.                                | -     |
| firstName           | string | false    | 名                                                 | -     |
| lastName            | string | false    | 姓                                                 | -     |
| cardType            | string | false    | 卡类型 PHY,VIRTUAL                                   | -     |
| category            | string | false    | 卡分类 GIFT_CARD,DEBIT_CARD,PREPAID_CARD,CREDIT_CARD | -     |
| currency            | string | false    | 币种                                                | -     |
| applicationStatus   | string | false    | 申请状态                                              | -     |
| reference           | string | false    | 唯一标识                                              | -     |
| createTimestampFrom | int64  | false    | 创建时间开始时间戳                                         | -     |
| createTimestampTo   | int64  | false    | 创建时间开始时间戳                                         | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/application/list --data '{
  "pageNo": 297,
  "pageSize": 853,
  "firstName": "Bob",
  "lastName": "Josh",
  "cardType": "PHY",
  "category": "CREDIT_CARD",
  "currency": "USD",
  "applicationStatus": "PENDING",
  "reference": "182931923123",
  "createTimestampFrom": 1672056033898,
  "createTimestampTo": 1772056033898
}'
```

**Response-fields:**

| Field                                             | Type   | Description                                                                    | Since |
|---------------------------------------------------|--------|--------------------------------------------------------------------------------|-------|
| code                                              | int32  | 响应码                                                                            | -     |
| message                                           | string | 响应描述                                                                           | -     |
| data                                              | object | 响应数据                                                                           | -     |
| └─totalCount                                      | int64  | Total record count                                                             | -     |
| └─pageSize                                        | int64  | Page size                                                                      | -     |
| └─totalPage                                       | int64  | Total pages                                                                    | -     |
| └─pageNo                                          | int64  | Current page                                                                   | -     |
| └─data                                            | array  | data records                                                                   | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid               | string | 用户uid                                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName         | string | 名                                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName          | string | 姓                                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardType          | string | 卡类型 PHY,VIRTUAL                                                                | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─category          | string | 卡分类 GIFT_CARD,DEBIT_CARD,PREPAID_CARD,CREDIT_CARD                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─currency          | string | 币种                                                                             | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneNo           | string | 手机号                                                                            | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─email             | string | 邮箱                                                                             | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─gender            | string | 性别 MALE,FEMALE                                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardId            | string | 卡id                                                                            | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─reference         | string | 唯一标识                                                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─applicationId     | string | 申请记录id                                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─applicationStatus | string | 申请状态 PENDING_INITIALIZATION, UNDER_REVIEW, REJECTED, APPROVED, SUCCESS, FAILED | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createTimestamp   | int64  | 创建时间                                                                           | -     |
| timestamp                                         | string | 时间戳毫秒                                                                          | -     |
| key                                               | string | 加密key                                                                          | -     |
| sign                                              | string | 签名                                                                             | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "totalCount": 158,
    "pageSize": 985,
    "totalPage": 79,
    "pageNo": 17,
    "data": [
      {
        "uid": "18277382199312",
        "firstName": "Bob",
        "lastName": "Josh",
        "cardType": "PHY",
        "category": "CREDIT_CARD",
        "currency": "USD",
        "phoneNo": "+87-192939123",
        "email": "test@example.com",
        "gender": "MALE",
        "cardId": "ETLPzgGfSzgmhqs",
        "reference": "18238192313",
        "applicationId": "ETLPzgGfSzDtuPNfeTMQqhonq-",
        "applicationStatus": "PENDING_INITIALIZATION",
        "createTimestamp": 1672056033898
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 卡申请详情
**URL:** /api/v2/build/card/application/detail

**Type:** POST


**Content-Type:** application/json

**Description:** 卡申请详情

**Body-parameters:**

| Parameter | Type | Required | Description | Since |
|-----------|------|----------|-------------|-------|
|id|string|true|记录id|-|

**Request-example:**
```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/application/detail --data '{
  "id": "81823"
}'
```
**Response-fields:**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|code|int32|响应码|-|
|message|string|响应描述|-|
|data|object|响应数据|-|
|└─id|string|记录id|-|
|└─uid|string|用户uid|-|
|└─name|string|姓名|-|
|└─firstName|string|名|-|
|└─midName|string|中间名|-|
|└─lastName|string|姓|-|
|└─birthday|string|生日 YYYY-MM-DD|-|
|└─phoneNo|string|手机号|-|
|└─email|string|邮箱地址|-|
|└─gender|string|性别 MALE,FEMALE|-|
|└─cardType|string|卡类型 VIRTUAL,PHY|-|
|└─cardId|string|卡id|-|
|└─status|string|申请状态 PENDING_INITIALIZATION, UNDER_REVIEW, REJECTED, APPROVED, SUCCESS, FAILED|-|
|└─cardInf|object|卡信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─id|string|卡id|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─pan|string|卡号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─panFirst6|string|卡号前6位|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─panLast4|string|卡号后4位|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid|string|用户id|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cvv2|string|卡cvv2|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─expiry|string|有效期|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardType|string|卡类型 VIRTUAL,PHY|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─status|string|卡状态 NEW, CREATED, PRE_ACTIVATION, DISPATCHED, INVALID, ACTIVE, TEMP_BLOCKED, PERM_BLOCKED, REJECTED|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createdAt|int64|创建时间戳|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─updatedAt|int64|更新时间戳|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─category|string|卡类型 DEBIT_CARD,PREPAID_CARD,CREDIT_CARD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─currency|string|币种|-|
|└─cardHolder|object|持卡人信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid|string|用户uid|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─name|string|姓名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName|string|名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─midName|string|中间名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName|string|姓|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─birthday|string|生日 YYYY-MM-DD|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneNo|string|手机号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─email|string|邮箱地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─gender|string|性别 MALE,FEMALE|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addr|object|地址信息|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─type|string|类型|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country|string|国家ISO码|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city|string|市|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─neighborhood|string|邻居|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state|string|区|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1|string|地址|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postcode|string|邮编|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createdAt|int64|创建时间戳|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─updatedAt|int64|更新时间戳|-|
|└─cardProfile|object|卡简介|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─pinFailCount|int32|密码失败次数|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─reissue|int32|重新发行次数|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─embossedName|string|图案名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─profileName|string|简介名|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─upstreamSeqNum|int32|上游序列号|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createdAt|int64|创建时间戳|-|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─updatedAt|int64|更新时间戳|-|
|└─createdAt|int64|创建时间戳|-|
|└─updatedAt|int64|更新时间戳|-|
|timestamp|string|时间戳毫秒|-|
|key|string|加密key|-|
|sign|string|签名|-|

**Response-example:**
```
{
  "code": 200,
  "message": "Success",
  "data": {
    "id": "ETLPzgGfSzDtuPNfeTMQqhnij-",
    "uid": "1877543923242373120",
    "name": "Jack D Joe",
    "firstName": "Jack",
    "midName": "D",
    "lastName": "Joe",
    "birthday": "1990-01-01",
    "phoneNo": "+61-123456789",
    "email": "example@example.com",
    "gender": "MALE",
    "cardType": "VIRTUAL",
    "cardId": "ETLPzgGfSzgmhih",
    "status": "APPROVED",
    "cardInf": {
      "id": "ETLPzgGfSzgmhih",
      "pan": "5185453411223010229",
      "panFirst6": "518545",
      "panLast4": "0229",
      "uid": "1877543923242373120",
      "cvv2": "671",
      "expiry": "205001",
      "cardType": "VIRTUAL",
      "status": "awq5s2",
      "createdAt": 1736476429000,
      "updatedAt": 1736476477000,
      "category": "PREPAID_CARD",
      "currency": "USD"
    },
    "cardHolder": {
      "uid": "1877543923242373120",
      "name": "Jack D Joe",
      "firstName": "Jack",
      "midName": "D",
      "lastName": "Joe",
      "birthday": "1990-01-01",
      "phoneNo": "+61-123456789",
      "email": "example@example.com",
      "gender": "MALE",
      "addr": {
        "type": "USER",
        "country": "US",
        "city": "Los Angeles",
        "neighborhood": "The neighborhood",
        "state": "Ls",
        "addressLine1": "Unit 701,Block B,The Victoria Tower.",
        "postcode": "200001"
      },
      "createdAt": 1736476429000,
      "updatedAt": 1736476477000
    },
    "cardProfile": {
      "pinFailCount": 0,
      "reissue": 0,
      "embossedName": "USD PREPAID1",
      "profileName": "test_multiple_profiles",
      "upstreamSeqNum": 1,
      "createdAt": 1736476429000,
      "updatedAt": 1736476477000
    },
    "createdAt": 1736476429000,
    "updatedAt": 1736476477000
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 审核卡

**URL:** /api/v2/build/card/review

**Type:** POST

**Content-Type:** application/json

**Description:** 审核卡

**Body-parameters:**

| Parameter     | Type   | Required | Description                   | Since |
|---------------|--------|----------|-------------------------------|-------|
| applicationId | string | true     | 申请记录id                        | -     |
| reviewStatus  | string | true     | 审核 Allowed: APPROVED,REJECTED | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/review --data '{
  "applicationId": "ETLPzgGfSzDtuPNfeTMQqhonq-",
  "reviewStatus": "APPROVED"
}'
```

**Response-fields:**

| Field           | Type   | Description | Since |
|-----------------|--------|-------------|-------|
| code            | int32  | 响应码         | -     |
| message         | string | 响应描述        | -     |
| data            | object | 响应数据        | -     |
| └─applicationId | string | 申请记录id      | -     |
| └─cardId        | string | 卡id         | -     |
| timestamp       | string | 时间戳毫秒       | -     |
| key             | string | 加密key       | -     |
| sign            | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "applicationId": "ETLPzgGfSzDtuPNfeTMQqhonq-",
    "cardId": "ETLPzgGfSzgmhqs"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 实体卡签发卡

**URL:** /api/v2/build/card/issuePhyCard

**Type:** POST

**Content-Type:** application/json

**Description:** 实体卡签发卡

**Body-parameters:**

| Parameter      | Type   | Required | Description | Since |
|----------------|--------|----------|-------------|-------|
| applicationId  | string | true     | 申请记录id      | -     |
| pan            | string | true     | 卡号          | -     |
| activationCode | string | true     | 激活码         | -     |
| expiry         | string | true     | 有效期         | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/issuePhyCard --data '{
  "applicationId": "ETLPzgGfSzDtuPNfeTMQqhonq-",
  "pan": "08912",
  "activationCode": "4085428279146910725",
  "expiry": "203009"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 卡片列表

**URL:** /api/v2/build/card/list

**Type:** POST

**Content-Type:** application/json

**Description:** 卡片列表

**Body-parameters:**

| Parameter           | Type   | Required | Description                             | Since |
|---------------------|--------|----------|-----------------------------------------|-------|
| pageNo              | int64  | false    | No comments found.                      | -     |
| pageSize            | int64  | false    | No comments found.                      | -     |
| cardHolderName      | string | false    | 卡持有人名                                   | -     |
| cardType            | string | false    | 卡类型 PHY,VIRTUAL                         | -     |
| category            | string | false    | 卡分类 DEBIT_CARD,PREPAID_CARD,CREDIT_CARD | -     |
| currency            | string | false    | 结算币种                                    | -     |
| cardStatus          | string | false    | 卡状态                                     | -     |
| createTimestampFrom | int64  | false    | 创建时间开始时间戳                               | -     |
| createTimestampTo   | int64  | false    | 创建时间开始时间戳                               | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/list --data '{
  "pageNo": 75,
  "pageSize": 807,
  "cardHolderName": "Mike Joe",
  "cardType": "PHY",
  "category": "CREDIT_CARD",
  "currency": "USD",
  "cardStatus": "ACTIVE",
  "createTimestampFrom": 1672056033898,
  "createTimestampTo": 1772056033898
}'
```

**Response-fields:**

| Field                                           | Type   | Description                                                                                         | Since |
|-------------------------------------------------|--------|-----------------------------------------------------------------------------------------------------|-------|
| code                                            | int32  | 响应码                                                                                                 | -     |
| message                                         | string | 响应描述                                                                                                | -     |
| data                                            | object | 响应数据                                                                                                | -     |
| └─totalCount                                    | int64  | Total record count                                                                                  | -     |
| └─pageSize                                      | int64  | Page size                                                                                           | -     |
| └─totalPage                                     | int64  | Total pages                                                                                         | -     |
| └─pageNo                                        | int64  | Current page                                                                                        | -     |
| └─data                                          | array  | data records                                                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid             | string | 用户uid                                                                                               | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardId          | string | 卡id                                                                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cvv2            | string | cvv2                                                                                                | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─expiry          | string | 有效期                                                                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─currency        | string | 结算币种                                                                                                | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardType        | string | 卡类型 PHYSICAL,VIRTUAL                                                                                | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─category        | string | 卡分类 DEBIT_CARD,PREPAID_CARD,CREDIT_CARD                                                             | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardHolderName  | string | 卡持有人名                                                                                               | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─pan             | string | 卡号                                                                                                  | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─panFirst6       | string | 卡号前6位                                                                                               | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─panLast4        | string | 卡号后4位                                                                                               | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardStatus      | string | 卡状态 NEW, CREATED, PRE_ACTIVATION, DISPATCHED, INVALID, ACTIVE, TEMP_BLOCKED, PERM_BLOCKED, REJECTED | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createTimestamp | int64  | 创建时间                                                                                                | -     |
| timestamp                                       | string | 时间戳毫秒                                                                                               | -     |
| key                                             | string | 加密key                                                                                               | -     |
| sign                                            | string | 签名                                                                                                  | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "totalCount": 666,
    "pageSize": 116,
    "totalPage": 12,
    "pageNo": 342,
    "data": [
      {
        "uid": "178129391923",
        "cardId": "ETLPzgGfSzgmhqs",
        "cvv2": "716",
        "expiry": "203009",
        "currency": "USD",
        "cardType": "PHYSICAL",
        "category": "CREDIT_CARD",
        "cardHolderName": "Mike Jeo",
        "pan": "5185453411223010229",
        "panFirst6": "518545",
        "panLast4": "0229",
        "cardStatus": "ACTIVE",
        "createTimestamp": 1672056033898
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 卡详情

**URL:** /api/v2/build/card/detail

**Type:** POST

**Content-Type:** application/json

**Description:** 卡详情

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| id        | string | true     | 记录id        | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/detail --data '{
  "id": "81823"
}'
```

**Response-fields:**

| Field                                                                       | Type   | Description                                                  | Since |
|-----------------------------------------------------------------------------|--------|--------------------------------------------------------------|-------|
| code                                                                        | int32  | 响应码                                                          | -     |
| message                                                                     | string | 响应描述                                                         | -     |
| data                                                                        | object | 响应数据                                                         | -     |
| └─card                                                                      | object | 卡信息                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─id                                          | string | 卡id                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─customerId                                  | string | 卡业务类型                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─pan                                         | string | 卡号                                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─panFirst6                                   | string | 卡号前6位                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─panLast4                                    | string | 卡号后4位                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid                                         | string | 用户uid                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cvv2                                        | string | cvv2                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─expiry                                      | string | 有效期                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardType                                    | string | 卡类型 PHYSICAL,VIRTUAL                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─category                                    | string | 卡分类 DEBIT_CARD,PREPAID_CARD,CREDIT_CARD                      | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─status                                      | string | 卡状态                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createdAt                                   | int64  | 创建时间                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─updatedAt                                   | int64  | 修改时间                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardHolderName                              | string | 卡持有人名                                                        | -     |
| └─cardProfile                                                               | object | 卡片简介                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─logo                                        | string | logo                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardNoRange                                 | string | 卡号范围                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─pinFailCount                                | int32  | 密码错误次数                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─reissue                                     | int32  | 补发次数                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─embossedName                                | string | 图案名称                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─upstreamSeqNum                              | int32  | 上游序列号                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─billingAddress                              | object | No comments found.                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─type          | string | 地址类型 POST,USER,BILLING                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country       | string | 国家IOS码                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city          | string | 城市或地区                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─neighborhood  | string | 最近的建築物                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state         | string | 地址所在州、省、县或地区                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1  | string | 地址第一行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2  | string | 地址第二行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3  | string | 地址第三行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine1 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine2 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine3 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postcode      | string | 邮政编码或邮政编码                                                    | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postAddress                                 | object | No comments found.                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─type          | string | 地址类型 POST,USER,BILLING                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country       | string | 国家IOS码                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city          | string | 城市或地区                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─neighborhood  | string | 最近的建築物                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state         | string | 地址所在州、省、县或地区                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1  | string | 地址第一行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2  | string | 地址第二行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3  | string | 地址第三行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine1 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine2 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine3 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postcode      | string | 邮政编码或邮政编码                                                    | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createdAt                                   | int64  | 创建时间                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─updatedAt                                   | int64  | 修改时间                                                         | -     |
| └─cardApplication                                                           | object | 卡申请信息                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─id                                          | string | 申请记录id                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid                                         | string | 用户id                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─reference                                   | string | 唯一标识防重                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─firstName                                   | string | 名                                                            | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─midName                                     | string | 中间名                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─lastName                                    | string | 姓                                                            | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─birthday                                    | string | 出生日期，格式为YYYY-MM-DD                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneNo                                     | string | 电话号码，格式：{countryCode,0 ... 4 个字符}-{phoneNumber,0 ... 33 个字符} | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─email                                       | string | 邮箱地址                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─gender                                      | string | 性别Allowed: MALE┃FEMALE                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─name                                        | string | 名字                                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─userAddr                                    | object | No comments found.                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─type          | string | 地址类型 POST,USER,BILLING                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country       | string | 国家IOS码                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city          | string | 城市或地区                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─neighborhood  | string | 最近的建築物                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state         | string | 地址所在州、省、县或地区                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1  | string | 地址第一行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2  | string | 地址第二行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3  | string | 地址第三行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine1 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine2 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine3 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postcode      | string | 邮政编码或邮政编码                                                    | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardBillingAddress                          | object | No comments found.                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─type          | string | 地址类型 POST,USER,BILLING                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country       | string | 国家IOS码                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city          | string | 城市或地区                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─neighborhood  | string | 最近的建築物                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state         | string | 地址所在州、省、县或地区                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1  | string | 地址第一行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2  | string | 地址第二行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3  | string | 地址第三行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine1 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine2 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine3 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postcode      | string | 邮政编码或邮政编码                                                    | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardPostAddress                             | object | No comments found.                                           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─type          | string | 地址类型 POST,USER,BILLING                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country       | string | 国家IOS码                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city          | string | 城市或地区                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─neighborhood  | string | 最近的建築物                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state         | string | 地址所在州、省、县或地区                                                 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1  | string | 地址第一行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2  | string | 地址第二行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3  | string | 地址第三行                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine1 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine2 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine3 | string |                                                              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postcode      | string | 邮政编码或邮政编码                                                    | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardType                                    | string | 卡类型 Allowed: VIRTUAL,PHY                                     | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─category                                    | string | 卡类别 Allowed: DEBIT_CARD,PREPAID_CARD,CREDIT_CARD             | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─profile                                     | string | 申请卡简介                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardId                                      | string | 卡id                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─status                                      | string | 卡状态                                                          | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createdAt                                   | int64  | 创建时间                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─updatedAt                                   | int64  | 修改时间                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─limitAmount                                 | string | 消费限额                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─limitCurrency                               | string | 限额币种                                                         | -     |
| └─cardLimit                                                                 | object | 卡消费限额                                                        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─limit                                       | int64  | 消费限额                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─currency                                    | string | 限额币种                                                         | -     |
| └─cardDesign                                                                | object | 卡片设计信息                                                       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─picFrontUrl                                 | string | 卡正面图                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─picBackUrl                                  | string | 卡别面图                                                         | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─name                                        | string | 卡名字                                                          | -     |
| timestamp                                                                   | string | 时间戳毫秒                                                        | -     |
| key                                                                         | string | 加密key                                                        | -     |
| sign                                                                        | string | 签名                                                           | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "card": {
      "id": "ETLPzgGfSzgmhqs",
      "customerId": "ENTERPRISE",
      "pan": "5185453411223010229",
      "panFirst6": "518545",
      "panLast4": "0229",
      "uid": "178129391923",
      "cvv2": "716",
      "expiry": "203009",
      "cardType": "PHYSICAL",
      "category": "CREDIT_CARD",
      "status": "ACTIVE",
      "createdAt": 1694161491593,
      "updatedAt": 1694161491593,
      "cardHolderName": "Mike Jeo"
    },
    "cardProfile": {
      "logo": "https://www.ulec.com.cn/wp-content/uploads/2010/08/1.png",
      "cardNoRange": "string",
      "pinFailCount": 0,
      "reissue": 0,
      "embossedName": "qt",
      "upstreamSeqNum": 0,
      "billingAddress": {
        "type": "POST",
        "country": "US",
        "city": "Hollywood",
        "neighborhood": "The neighborhood",
        "state": "CA",
        "addressLine1": "cheer Building",
        "addressLine2": "Chifley Tower",
        "addressLine3": "2 Square",
        "phoneticLine1": "phoneticLine1",
        "phoneticLine2": "phoneticLine2",
        "phoneticLine3": "phoneticLine3",
        "postcode": "100001"
      },
      "postAddress": {
        "type": "POST",
        "country": "US",
        "city": "Hollywood",
        "neighborhood": "The neighborhood",
        "state": "CA",
        "addressLine1": "cheer Building",
        "addressLine2": "Chifley Tower",
        "addressLine3": "2 Square",
        "phoneticLine1": "phoneticLine1",
        "phoneticLine2": "phoneticLine2",
        "phoneticLine3": "phoneticLine3",
        "postcode": "100001"
      },
      "createdAt": 1694161491593,
      "updatedAt": 1694161491593
    },
    "cardApplication": {
      "id": "Emadae13dsfa.z",
      "uid": "18812391293",
      "reference": "812931023123",
      "firstName": "Bob",
      "midName": "D",
      "lastName": "Josh",
      "birthday": "1970-10-11",
      "phoneNo": "+61-123456789",
      "email": "contactMe@gmail.com",
      "gender": "MALE",
      "name": "Mike Joe",
      "userAddr": {
        "type": "POST",
        "country": "US",
        "city": "Hollywood",
        "neighborhood": "The neighborhood",
        "state": "CA",
        "addressLine1": "cheer Building",
        "addressLine2": "Chifley Tower",
        "addressLine3": "2 Square",
        "phoneticLine1": "phoneticLine1",
        "phoneticLine2": "phoneticLine2",
        "phoneticLine3": "phoneticLine3",
        "postcode": "100001"
      },
      "cardBillingAddress": {
        "type": "POST",
        "country": "US",
        "city": "Hollywood",
        "neighborhood": "The neighborhood",
        "state": "CA",
        "addressLine1": "cheer Building",
        "addressLine2": "Chifley Tower",
        "addressLine3": "2 Square",
        "phoneticLine1": "phoneticLine1",
        "phoneticLine2": "phoneticLine2",
        "phoneticLine3": "phoneticLine3",
        "postcode": "100001"
      },
      "cardPostAddress": {
        "type": "POST",
        "country": "US",
        "city": "Hollywood",
        "neighborhood": "The neighborhood",
        "state": "CA",
        "addressLine1": "cheer Building",
        "addressLine2": "Chifley Tower",
        "addressLine3": "2 Square",
        "phoneticLine1": "phoneticLine1",
        "phoneticLine2": "phoneticLine2",
        "phoneticLine3": "phoneticLine3",
        "postcode": "100001"
      },
      "cardType": "VIRTUAL",
      "category": "CREDIT_CARD",
      "profile": "test profile",
      "cardId": "ETLPzgGfSzgmhqs",
      "status": "ACTIVE",
      "createdAt": 1694161491593,
      "updatedAt": 1694161491593,
      "limitAmount": "500",
      "limitCurrency": "USD"
    },
    "cardLimit": {
      "limit": 500,
      "currency": "USD"
    },
    "cardDesign": {
      "picFrontUrl": "https://test.pic.com/1.png",
      "picBackUrl": "https://test.pic.com/2.png",
      "name": "qt"
    }
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 卡订单列表

**URL:** /api/v2/build/card/order/list

**Type:** POST

**Content-Type:** application/json

**Description:** 卡订单列表

**Body-parameters:**

| Parameter           | Type   | Required | Description                                                | Since |
|---------------------|--------|----------|------------------------------------------------------------|-------|
| pageNo              | int64  | false    | No comments found.                                         | -     |
| pageSize            | int64  | false    | No comments found.                                         | -     |
| holderName          | string | false    | 持卡人名字                                                      | -     |
| cardId              | string | false    | 卡id                                                        | -     |
| uid                 | string | false    | 用户uid                                                      | -     |
| amountStart         | number | false    | 金额from                                                     | -     |
| amountEnd           | number | false    | 金额to                                                       | -     |
| orderStatus         | string | false    | 订单状态 PENDING,WAITING FOR REVIEW,COMPLETED,CANCELLED,FAILED | -     |
| createTimestampFrom | int64  | false    | 创建时间开始时间戳                                                  | -     |
| createTimestampTo   | int64  | false    | 创建时间开始时间戳                                                  | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/order/list --data '{
  "pageNo": 520,
  "pageSize": 521,
  "holderName": "Mike",
  "cardId": "ETLPzgGfSzgmhqs",
  "uid": "1879000304432582656",
  "amountStart": 40,
  "amountEnd": 500,
  "orderStatus": "COMPLETED",
  "createTimestampFrom": 1672056033898,
  "createTimestampTo": 1772056033898
}'
```

**Response-fields:**

| Field                                               | Type   | Description                                                | Since  |
|-----------------------------------------------------|--------|------------------------------------------------------------|--------|
| code                                                | int32  | 响应码                                                        | -      |
| message                                             | string | 响应描述                                                       | -      |
| data                                                | object | 响应数据                                                       | -      |
| └─totalCount                                        | int64  | Total record count                                         | -      |
| └─pageSize                                          | int64  | Page size                                                  | -      |
| └─totalPage                                         | int64  | Total pages                                                | -      |
| └─pageNo                                            | int64  | Current page                                               | -      |
| └─data                                              | array  | data records                                               | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─uid                 | string | 用户uid                                                      | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─orderNo             | string | 订单编号                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardId              | string | 卡id                                                        | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─orderCurrency       | string | 币种                                                         | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─holderName          | string | 持卡人名                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─orderAmount         | number | 订单金额                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─settleAmount        | number | 结算金额                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─reversalAmount      | number | 退款金额                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─transactionType     | string | 交易类型 PURCHASE                                              | REFUND |-|
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─merchantName        | string | 商户名                                                        | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─createTime          | int64  | 用户uid                                                      | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─orderType           | string | 订单类型                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─transactionAmount   | number | 交易金额                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─transactionCurrency | string | 交易币种                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─cardPresent         | int32  | 是否有卡 1-有 0-没                                               | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country             | string | 国家ISO                                                      | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city                | string | 城市                                                         | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─mcc                 | string | 交易mcc                                                      | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─acquiringCurrency   | string | 收单币种                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─acquiringAmount     | number | 收单金额                                                       | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─version             | string | 版本                                                         | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─relatedOrderNo      | string | 关联订单号用逗号分隔                                                 | -      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─orderStatus         | string | 订单状态 PENDING,WAITING FOR REVIEW,COMPLETED,CANCELLED,FAILED | -      |
| timestamp                                           | string | 时间戳毫秒                                                      | -      |
| key                                                 | string | 加密key                                                      | -      |
| sign                                                | string | 签名                                                         | -      |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "totalCount": 11,
    "pageSize": 994,
    "totalPage": 405,
    "pageNo": 162,
    "data": [
      {
        "uid": "1879000304432582656",
        "orderNo": "1834499556319834112",
        "cardId": "ETLPzgGfSzg-ef",
        "currency": "USD",
        "holderName": "Mike Jeo",
        "orderAmount": 50,
        "settleAmount": 50,
        "reversalAmount": 0,
        "transactionType": "PURCHASE",
        "merchantName": "PELICANA",
        "createTime": 1724785053024,
        "orderType": "Standard Order",
        "transactionAmount": 600,
        "transactionCurrency": "HKD",
        "cardPresent": 0,
        "country": "US",
        "city": "Perth",
        "mcc": "5876",
        "acquiringCurrency": "USD",
        "acquiringAmount": 183.12,
        "version": "v1",
        "relatedOrderNo": "1830279551182712835,1830635075516903428",
        "orderStatus": "PENDING"
      }
    ]
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 卡订单详情

**URL:** /api/v2/build/card/order/detail

**Type:** POST

**Content-Type:** application/json

**Description:** 卡订单详情

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| orderNo   | string | true     | 订单号         | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/order/detail --data '{
  "orderNo": "1834499556319834112"
}'
```

**Response-fields:**

| Field                 | Type   | Description                                                | Since |
|-----------------------|--------|------------------------------------------------------------|-------|
| code                  | int32  | 响应码                                                        | -     |
| message               | string | 响应描述                                                       | -     |
| data                  | object | 响应数据                                                       | -     |
| └─uid                 | string | 用户uid                                                      | -     |
| └─orderNo             | string | 订单编号                                                       | -     |
| └─cardId              | string | 卡id                                                        | -     |
| └─currency            | string | 币种                                                         | -     |
| └─holderName          | string | 持卡人名                                                       | -     |
| └─orderAmount         | number | 订单金额                                                       | -     |
| └─settleAmount        | number | 结算金额                                                       | -     |
| └─reversalAmount      | number | 退款金额                                                       | -     |
| └─transactionType     | string | 交易类型 PURCHASE,REFUND                                       | -     |
| └─merchantName        | string | 商户名                                                        | -     |
| └─createTime          | int64  | 创建时间戳                                                      | -     |
| └─orderType           | string | 订单类型                                                       | -     |
| └─transactionAmount   | number | 交易金额                                                       | -     |
| └─transactionCurrency | string | 交易币种                                                       | -     |
| └─cardPresent         | int32  | 是否有卡 1-有 0-没                                               | -     |
| └─country             | string | 国家ISO                                                      | -     |
| └─city                | string | 城市                                                         | -     |
| └─mcc                 | string | 交易mcc                                                      | -     |
| └─relatedOrderNo      | string | 关联订单号用逗号分隔                                                 | -     |
| └─acquiringCurrency   | string | 收单币种                                                       | -     |
| └─acquiringAmount     | number | 收单金额                                                       | -     |
| └─version             | string | 版本                                                         | -     |
| └─orderStatus         | string | 订单状态 PENDING,WAITING FOR REVIEW,COMPLETED,CANCELLED,FAILED | -     |
| timestamp             | string | 时间戳毫秒                                                      | -     |
| key                   | string | 加密key                                                      | -     |
| sign                  | string | 签名                                                         | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "uid": "1879000304432582656",
    "orderNo": "1834499556319834112",
    "cardId": "ETLPzgGfSzg-ef",
    "currency": "USD",
    "holderName": "Mike Jeo",
    "orderAmount": 50,
    "settleAmount": 50,
    "reversalAmount": 0,
    "transactionType": "PURCHASE",
    "merchantName": "PELICANA",
    "createTime": 1724785053024,
    "orderType": "Standard Order",
    "transactionAmount": 600,
    "transactionCurrency": "HKD",
    "cardPresent": 0,
    "country": "US",
    "city": "Perth",
    "mcc": "5876",
    "relatedOrderNo": "1830279551182712835,1830635075516903428",
    "acquiringCurrency": "USD",
    "acquiringAmount": 183.12,
    "version": "v1",
    "orderStatus": "PENDING"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 更新持卡人手机

**URL:** /api/v2/build/card/holder/updatePhone

**Type:** POST

**Content-Type:** application/json

**Description:** 更新持卡人手机

**Body-parameters:**

| Parameter   | Type   | Required | Description | Since |
|-------------|--------|----------|-------------|-------|
| phoneNumber | string | true     | 手机号         | -     |
| countryCode | string | true     | 国家号         | -     |
| uid         | string | true     | 用户id        | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/holder/updatePhone --data '{
  "phoneNumber": "123456",
  "countryCode": "+61",
  "uid": "1881937643756720128"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 更个新持卡人邮箱地址

**URL:** /api/v2/build/card/holder/updateEmail

**Type:** POST

**Content-Type:** application/json

**Description:** 更个新持卡人邮箱地址

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| email     | string | true     | 邮箱地址        | -     |
| uid       | string | true     | 用户id        | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/holder/updateEmail --data '{
  "email": "somewhere@gmail.com",
  "uid": "1881937643756720128"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 更新持卡人信息

**URL:** /api/v2/build/card/holder/updateCardHolder

**Type:** POST

**Content-Type:** application/json

**Description:** 更新持卡人信息

**Body-parameters:**

| Parameter | Type   | Required | Description    | Since |
|-----------|--------|----------|----------------|-------|
| uid       | string | true     | 用户id           | -     |
| firstName | string | false    | 名              | -     |
| midName   | string | false    | 中间名            | -     |
| lastName  | string | false    | 姓              | -     |
| birthday  | string | false    | 生日             | -     |
| gender    | string | false    | 性别 MALE,FEMALE | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/holder/updateCardHolder --data '{
  "uid": "1881937643756720128",
  "firstName": "Jame",
  "midName": "D",
  "lastName": "Joe",
  "birthday": "2000-02-01",
  "gender": "MALE"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 更新持卡人账单地址

**URL:** /api/v2/build/card/holder/updateBillingAddress

**Type:** POST

**Content-Type:** application/json

**Description:** 更新持卡人账单地址

**Body-parameters:**

| Parameter     | Type   | Required | Description  | Since |
|---------------|--------|----------|--------------|-------|
| cardId        | string | true     | 卡id          | -     |
| country       | string | true     | 国家IOS码       | -     |
| city          | string | true     | 城市或地区        | -     |
| neighborhood  | string | false    | 最近的建築物       | -     |
| state         | string | true     | 地址所在州、省、县或地区 | -     |
| addressLine1  | string | true     | 地址第一行        | -     |
| addressLine2  | string | false    | 地址第二行        | -     |
| addressLine3  | string | false    | 地址第三行        | -     |
| phoneticLine1 | string | false    |              | -     |
| phoneticLine2 | string | false    |              | -     |
| phoneticLine3 | string | false    |              | -     |
| postcode      | string | true     | 邮政编码或邮政编码    | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/holder/updateBillingAddress --data '{
  "cardId": "ETLPzgGfSzgnqqi",
  "country": "US",
  "city": "Hollywood",
  "neighborhood": "The neighborhood",
  "state": "CA",
  "addressLine1": "cheer Building",
  "addressLine2": "Chifley Tower",
  "addressLine3": "2 Square",
  "phoneticLine1": "phoneticLine1",
  "phoneticLine2": "phoneticLine2",
  "phoneticLine3": "phoneticLine3",
  "postcode": "100001"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 查询持卡人详情

**URL:** /api/v2/build/card/holder/detail

**Type:** POST

**Content-Type:** application/json

**Description:** 查询持卡人详情

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| id        | string | true     | 记录id        | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/holder/detail --data '{
  "id": "81823"
}'
```

**Response-fields:**

| Field                                         | Type   | Description  | Since |
|-----------------------------------------------|--------|--------------|-------|
| code                                          | int32  | 响应码          | -     |
| message                                       | string | 响应描述         | -     |
| data                                          | object | 响应数据         | -     |
| └─id                                          | string | 持卡人id        | -     |
| └─uid                                         | string | 持卡人用户id      | -     |
| └─name                                        | string | 持卡人全名        | -     |
| └─firstName                                   | string | 持卡人名         | -     |
| └─midName                                     | string | 持卡人中间名       | -     |
| └─lastName                                    | string | 持卡人姓         | -     |
| └─birthday                                    | string | 持卡人生日        | -     |
| └─phoneNo                                     | string | 持卡人手机        | -     |
| └─email                                       | string | 持卡人邮箱        | -     |
| └─gender                                      | string | 持卡人性别        | -     |
| └─addr                                        | object | 地址           | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─country       | string | 国家IOS码       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─city          | string | 城市或地区        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─neighborhood  | string | 最近的建築物       | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─state         | string | 地址所在州、省、县或地区 | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine1  | string | 地址第一行        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine2  | string | 地址第二行        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─addressLine3  | string | 地址第三行        | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine1 | string |              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine2 | string |              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─phoneticLine3 | string |              | -     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└─postcode      | string | 邮政编码或邮政编码    | -     |
| timestamp                                     | string | 时间戳毫秒        | -     |
| key                                           | string | 加密key        | -     |
| sign                                          | string | 签名           | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "id": "181823123",
    "uid": "1698974509547507713",
    "name": "Tommy.Ernest.Emmanuel",
    "firstName": "Tommy",
    "midName": "Ernest",
    "lastName": "Emmanuel",
    "birthday": "1990-01-01",
    "phoneNo": "+61-123456789",
    "email": "contactMe@gmail.com",
    "gender": "MALE",
    "addr": {
      "country": "US",
      "city": "Hollywood",
      "neighborhood": "The neighborhood",
      "state": "CA",
      "addressLine1": "cheer Building",
      "addressLine2": "Chifley Tower",
      "addressLine3": "2 Square",
      "phoneticLine1": "phoneticLine1",
      "phoneticLine2": "phoneticLine2",
      "phoneticLine3": "phoneticLine3",
      "postcode": "100001"
    }
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 更新卡pin

**URL:** /api/v2/build/card/updateCardPin

**Type:** POST

**Content-Type:** application/json

**Description:** 更新卡pin

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| uid       | string | true     | 用户uid       | -     |
| cardId    | string | true     | 卡id         | -     |
| pin       | string | true     | 卡密码         | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/updateCardPin --data '{
  "uid": "18781278312312",
  "cardId": "ETLPzgGfSzgmno",
  "pin": "1234"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 解禁临时禁用卡

**URL:** /api/v2/build/card/releaseBlock

**Type:** POST

**Content-Type:** application/json

**Description:** 解禁临时禁用卡

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| cardId    | string | true     | 卡id         | -     |
| memo      | string | false    | memo        | -     |
| startTime | int64  | false    | 开始时间        | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/releaseBlock --data '{
  "cardId": "ETLPzgGfSzgmno",
  "memo": "memo here",
  "startTime": 0
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 临时禁用卡

**URL:** /api/v2/build/card/block

**Type:** POST

**Content-Type:** application/json

**Description:** 临时禁用卡

**Body-parameters:**

| Parameter | Type   | Required | Description                                                                                                     | Since |
|-----------|--------|----------|-----------------------------------------------------------------------------------------------------------------|-------|
| cardId    | string | true     | 卡id                                                                                                             | -     |
| memo      | string | false    | memo                                                                                                            | -     |
| reason    | string | true     | 原因 REPORTED_LOST_OR_STOLEN,TEMPORARY_SUSPENSION,FRAUD_PREVENTION,SYSTEM_RELATED,ACTIVATION_RELATED,DEACTIVATION | -     |
| startTime | int64  | false    | 开始时间                                                                                                            | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/block --data '{
  "cardId": "ETLPzgGfSzgmno",
  "memo": "memo here",
  "reason": "REPORTED_LOST_OR_STOLEN",
  "startTime": 0
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 激活实体卡

**URL:** /api/v2/build/card/activePhy

**Type:** POST

**Content-Type:** application/json

**Description:** 激活实体卡

**Body-parameters:**

| Parameter      | Type   | Required | Description | Since |
|----------------|--------|----------|-------------|-------|
| activationCode | string | true     | 激活码         | -     |
| expiry         | string | false    | 有效期         | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/activePhy --data '{
  "activationCode": "6675798267016130852",
  "expiry": "203009"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

## 卡业务模拟

### 模拟卡消费退款

**URL:** /api/v2/build/card/simulate/reversal

**Type:** POST

**Content-Type:** application/json

**Description:** 模拟卡消费退款

**Body-parameters:**

| Parameter               | Type   | Required | Description | Since |
|-------------------------|--------|----------|-------------|-------|
| cardId                  | string | true     | 卡id         | -     |
| acceptorName            | string | true     | 接收者名字       | -     |
| transactionSource       | string | true     | 交易来源        | -     |
| originalAuthorizationId | string | false    | 原始验证id      | -     |
| originalTransactionId   | string | false    | 原始交易id      | -     |
| partial                 | int32  | true     | partial     | -     |
| amount                  | number | true     | 金额          | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/simulate/reversal --data '{
  "cardId": "ETLPzgGfSzgmn_",
  "acceptorName": "AcceptorName#2",
  "transactionSource": "online",
  "originalAuthorizationId": "ETLPzgGfSzDTTLQuKEvVlssjqc_",
  "originalTransactionId": "ETLPzgGfSzWQftUdvTMQqhokk9",
  "partial": 0,
  "amount": 10
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| └─id      | string | id          | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "id": "ETLPzgGfSzDTTLQuKEvVlssjp_-e"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 模拟卡消费结算

**URL:** /api/v2/build/card/simulate/transaction

**Type:** POST

**Content-Type:** application/json

**Description:** 模拟卡消费结算

**Body-parameters:**

| Parameter              | Type   | Required | Description                                      | Since |
|------------------------|--------|----------|--------------------------------------------------|-------|
| cardId                 | string | true     | 卡id                                              | -     |
| acceptorName           | string | false    |                                                  | -     |
| transactionSource      | string | false    |                                                  | -     |
| currency               | string | true     | 币种                                               | -     |
| amount                 | number | true     | 金额                                               | -     |
| authorizationId        | string | false    | 验证id                                             | -     |
| type                   | string | true     | 交易类型 PURCHASE,REFUND                             | -     |
| partnerTransactionType | string | true     | 合作方交易类型 LOAD,WITHDRAW LOAD表示向账户扣减资金，WITHDRAW表示增加 | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/simulate/transaction --data '{
  "cardId": "ETLPzgGfSzgmn_",
  "acceptorName": "AcceptorName",
  "transactionSource": "online",
  "currency": "USD",
  "amount": 20,
  "authorizationId": "ETLPzgGfSzDHHLQuKEvVlssjo-gn",
  "type": "PURCHASE",
  "partnerTransactionType": "LOAD"
}'
```

**Response-fields:**

| Field           | Type   | Description | Since |
|-----------------|--------|-------------|-------|
| code            | int32  | 响应码         | -     |
| message         | string | 响应描述        | -     |
| data            | object | 响应数据        | -     |
| └─transactionId | string | 交易记录id      | -     |
| timestamp       | string | 时间戳毫秒       | -     |
| key             | string | 加密key       | -     |
| sign            | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "transactionId": "189192931823"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 模拟卡消费发起验证

**URL:** /api/v2/build/card/simulate/authorization

**Type:** POST

**Content-Type:** application/json

**Description:** 模拟卡消费发起验证

**Body-parameters:**

| Parameter              | Type   | Required | Description                                      | Since |
|------------------------|--------|----------|--------------------------------------------------|-------|
| cardId                 | string | true     | 卡id                                              | -     |
| originalAuthId         | string | false    | 如果 original_auth_id 不为空，则为增量冻结资金                 | -     |
| currency               | string | true     | 币种                                               | -     |
| amount                 | number | true     | 金额                                               | -     |
| acceptorName           | string | false    |                                                  | -     |
| transactionSource      | string | true     |                                                  | -     |
| type                   | string | true     | 交易类型 PURCHASE,REFUND                             | -     |
| partnerTransactionType | string | true     | 合作方交易类型 LOAD,WITHDRAW LOAD表示向账户扣减资金，WITHDRAW表示增加 | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/simulate/authorization --data '{
  "cardId": "ETLPzgGfSzgmn_",
  "originalAuthId": "ETLPzgGfSzDHHLQuKEvVlssjo-gn",
  "currency": "USD",
  "amount": 20,
  "acceptorName": "AcceptorName",
  "transactionSource": "online",
  "type": "PURCHASE",
  "partnerTransactionType": "LOAD"
}'
```

**Response-fields:**

| Field             | Type   | Description | Since |
|-------------------|--------|-------------|-------|
| code              | int32  | 响应码         | -     |
| message           | string | 响应描述        | -     |
| data              | object | 响应数据        | -     |
| └─authorizationId | string | 授权记录id      | -     |
| └─message         | string | message     | -     |
| timestamp         | string | 时间戳毫秒       | -     |
| key               | string | 加密key       | -     |
| sign              | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "data": {
    "authorizationId": "ETLPzgGfSzDTTLQuKEvVlssjp_-e",
    "message": "success"
  },
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 模拟卡绑定电子钱包时验证码发送

**URL:** /api/v2/build/card/simulate/behavior/activationCode

**Type:** POST

**Content-Type:** application/json

**Description:** 模拟卡绑定电子钱包时验证码发送

**Body-parameters:**

| Parameter        | Type   | Required | Description     | Since |
|------------------|--------|----------|-----------------|-------|
| activationCode   | string | true     | 激活码             | -     |
| activationMethod | string | true     | 激活码方式,SMS,EMAIL | -     |
| cardId           | string | true     | 卡id             | -     |
| expirationTime   | int64  | true     | 失效时间戳           | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/simulate/behavior/activationCode --data '{
  "activationCode": "555666",
  "activationMethod": "SMS",
  "cardId": "ETLPzgGfSzg5-ih",
  "expirationTime": 1737525034246
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 模拟发送OTP消息

**URL:** /api/v2/build/card/simulate/behavior/sendOtp

**Type:** POST

**Content-Type:** application/json

**Description:** 模拟发送OTP消息

**Body-parameters:**

| Parameter      | Type   | Required | Description  | Since |
|----------------|--------|----------|--------------|-------|
| uid            | string | false    | 用户id         | -     |
| cardId         | string | false    | 卡id          | -     |
| option         | string | false    | 方式，SMS,EMAIL | -     |
| otp            | string | false    | 一次性验证码       | -     |
| remark         | object | false    | 备注信息         | -     |
| └─amount       | string | false    | 金额           | -     |
| └─currency     | string | false    | 币种           | -     |
| └─merchantName | string | false    | 商户名          | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/simulate/behavior/sendOtp --data '{
  "uid": "1870731091074093056",
  "cardId": "ETLPzgGfSzgmhhj",
  "option": "EMAIL",
  "otp": "881723",
  "remark": {
    "amount": "18.4",
    "currency": "USD",
    "merchantName": "apple"
  }
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 模拟发送验证方式选择消息

**URL:** /api/v2/build/card/simulate/behavior/verificationChoose

**Type:** POST

**Content-Type:** application/json

**Description:** 模拟发送验证方式选择消息

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| uid       | string | false    | 用户id        | -     |
| cardId    | string | false    | 卡id         | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/simulate/behavior/verificationChoose --data '{
  "uid": "1870731091074093056",
  "cardId": "ETLPzgGfSzgmhhj"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```

### 模拟OBB验证

**URL:** /api/v2/build/card/simulate/behavior/obbValidate

**Type:** POST

**Content-Type:** application/json

**Description:** 模拟OBB验证

**Body-parameters:**

| Parameter | Type   | Required | Description | Since |
|-----------|--------|----------|-------------|-------|
| uid       | string | false    | 用户id        | -     |
| cardId    | string | false    | 卡id         | -     |

**Request-example:**

```
curl -X POST -H 'Content-Type: application/json' -i /api/v2/build/card/simulate/behavior/obbValidate --data '{
  "uid": "1870731091074093056",
  "cardId": "ETLPzgGfSzgmhhj"
}'
```

**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| code      | int32  | 响应码         | -     |
| message   | string | 响应描述        | -     |
| data      | object | 响应数据        | -     |
| timestamp | string | 时间戳毫秒       | -     |
| key       | string | 加密key       | -     |
| sign      | string | 签名          | -     |

**Response-example:**

```
{
  "code": 200,
  "message": "Success",
  "timestamp": "1685343278618",
  "key": "tvJ1Um",
  "sign": "LwpZUp"
}
```
## 卡业务回调处理
### 概述
卡业务有部分场景需要接入方确认的场景，接入方收到请求后，需要返回对应的消息进行确认，比如
+ 消费授权
+ 下发验证码
+ ...

**这部分回调功能需要单独配置新的回调URL才能使用**

### 授权
使用配置的cardAuthUrl进行回调接收信息，并且客户需要在2s内及时做出符合规则的响应

**Request payload**

| Field           | Type   | Description        | Since |
|-----------------|--------|--------------------|-------|
| authAmount      | string | 授权金额               | -     |
| currency        | string | 币种                 | -     |
| uid             | string | 用户id               | -     |
| cardId          | string | 卡id                | -     |
| tranTime        | int64  | 交易时间戳              | -     |
| merchantName    | string | 支付商户名              | -     |
| id              | string | 卡交易id              | -     |
| orderNo         | string | 关联的订单号             | -     |
| transactionType | string | 交易类型               | -     |
| authType        | string | 授权类型,AUTH,REVERSAL | -     |

**Request-example:**
```
{
  "authAmount": 128.88,
  "currency": "USD",
  "uid": "1808026787681538048",
  "cardId": "ETLPzgGfSzg-ef",
  "tranTime": 1724783070549,
  "merchantName": "PELICANA.",
  "id": "ETLPzgGfSzDTTLQuKEvVlssjq5mer",
  "orderNo": "1828633042242514944",
  "transactionType": "PURCHASE",
  "authType": "AUTH"
}
```
**Response-fields:**

| Field             | Type   | Description      | Since |
|-------------------|--------|------------------|-------|
| responseCode      | int32  | 响应码,0标识成功，其他标识失败 | -     |
| message           | string | 响应描述             | -     |
| externalReference | string | 附加信息             | -     |

**Response-example:**
```
{
	"responseCode": 0,
	"message": "success",
	"externalReference": ""
}
```
### 卡3DS相关
用于在用户刷卡时验证用户身份
使用配置的cardTdsUrl进行回调接收信息，并做出相关响应

**事件类型**

| Event               | Description |
|---------------------|-------------|
| VERIFICATION_CHOOSE | 验证方式选择      |
| OTP_SEND            | 发送验证吗       |
| OOB_SEND            | 发送App消息     |
| OOB_VALIDATE        | 验证App消息     |

**验证方式**

| Event | Description |
|-------|-------------|
| SMS   | 短信          |
| EMAIL | 邮件          |
| OOB   | App消息       |

**OOB结果**

| Event     | Description |
|-----------|-------------|
| PENDING   | 持卡人未能确认交易请求 |
| CONFIRMED | 持卡人确认交易请求   |
| REJECTED  | 持卡人拒绝交易请求   |

#### VERIFICATION_CHOOSE

**Request payload**

| Field           | Type   | Description | Since |
|-----------------|--------|-------------|-------|
| requestId       | string | 请求id        | -     |
| uid             | string | 用户id        | -     |
| cardId          | string | 卡id         | -     |
| event           | string | 事件          | -     |
| options         | array  | 验证方式        | -     |

**Request-example:**

```
{
  "requestId":"1668184762436685824",
  "uid":"1668184762436685823",
  "cardId":"ETLPzgGfSzg.pq",
  "event":"VERIFICATION_CHOOSE",
  "options":["EMAIL","SMS","OOB"]  
}
```
**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| requestId | string | 请求id        | -     |
| event     | string | 事件          | -     |
| options   | array  | 选择的方式       | -     |
| locale    | string | 语言          | -     |

**Response-example:**
```
{
  "requestId":"1668184762436685824",
  "event":"VERIFICATION_CHOOSE",
  "options":["EMAIL"],
  "locale":"en_US"
}
```

#### OTP_SEND

**Request payload**

| Field          | Type   | Description | Since |
|----------------|--------|-------------|-------|
| requestId      | string | 请求id        | -     |
| uid            | string | 用户id        | -     |
| cardId         | string | 卡id         | -     |
| event          | string | 事件          | -     |
| otp            | string | 一次性验证码      | -     |
| option         | string | 验证方式        | -     |
| remark         | object | 卡购买信息，可以为空  | -     |
| └─merchantName | string | 商户名         | -     |
| └─amount       | string | 金额          | -     |
| └─currency     | string | 币种          | -     |

**Request-example:**

```
{
  "requestId":"1668184762436685824",
  "uid":"1668184762436685823",
  "cardId":"ETLPzgGfSzg.pq",
  "event":"OTP_SEND",
  "option":"EMAIL", 
  "otp":"679023",
  "remark":{
    "merchantName":"apple.com",
    "amount":"10.00",
    "currency":"AUD"
  }
}
```
**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| requestId | string | 请求id        | -     |
| event     | string | 事件          | -     |
| email     | string | 邮箱          | -     |
| mobile    | string | 手机          | -     |

**Response-example:**
```
{
  "requestId":"1668184762436685824",
  "event":"OTP_SEND",
  "email":"abc@gamil.com" 
  "mobile":"+61490789322"  
}
```

#### OOB_SEND

**Request payload**

| Field          | Type   | Description | Since |
|----------------|--------|-------------|-------|
| requestId      | string | 请求id        | -     |
| uid            | string | 用户id        | -     |
| cardId         | string | 卡id         | -     |
| event          | string | 事件          | -     |
| option         | string | 验证方式        | -     |
| remark         | object | 卡购买信息，可以为空  | -     |
| └─merchantName | string | 商户名         | -     |
| └─amount       | string | 金额          | -     |
| └─currency     | string | 币种          | -     |

**Request-example:**

```
{
  "requestId":"1668184762436685824",
  "uid":"1668184762436685823",
  "cardId":"ETLPzgGfSzg.pq",
  "event":"OOB_SEND",
  "option":"OOB", 
  "remark":{
    "merchantName":"apple.com",
    "amount":"10.00",
    "currency":"AUD"
  }
}
```
**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| requestId | string | 请求id        | -     |
| event     | string | 事件          | -     |

**Response-example:**
```
{
  "requestId":"1668184762436685824",
  "event":"OOB_SEND"
}
```

#### OOB_VALIDATE

**Request payload**

| Field          | Type   | Description | Since |
|----------------|--------|-------------|-------|
| requestId      | string | 请求id        | -     |
| uid            | string | 用户id        | -     |
| cardId         | string | 卡id         | -     |
| event          | string | 事件          | -     |
| option         | string | 验证方式        | -     |

**Request-example:**

```
{
  "requestId":"1668184762436685824",
  "uid":"1668184762436685823",
  "cardId":"ETLPzgGfSzg.pq",
  "event":"OOB_VALIDATE",
  "option":"OOB"
}
```
**Response-fields:**

| Field     | Type   | Description | Since |
|-----------|--------|-------------|-------|
| requestId | string | 请求id        | -     |
| event     | string | 事件          | -     |
| result    | string | OOB 确认结果    | -     |

**Response-example:**
```
{
  "requestId":"1668184762436685824",
  "event":"OOB_VALIDATE",
  "result":"PENDING"
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

Webhook 回调时采用与 API鉴权 中相同的数据加密和签名方案，需要您在 ClearOnes 控制台配置您的 Webhook RSA 公钥。您可以参考
API 鉴权章节生成 RSA 公私钥对，在 ClearOnes 控制台可获取 ClearOnes Webhook RSA 公钥，您可以使用该公钥对回调数据进行验签。

### 回调请求描述

| Parameter  | Type   | Description                               |
|------------|--------|-------------------------------------------|
| timestamp  | int64  | 时间戳毫秒                                     |
| bizContent | string | 业务请求参数 AES 加密后数据                          |
| key        | string | 使用 ClearOnes Webhook RSA 私钥对请求参数签名得到的签名数据 |
| sign       | string | 使用您的 Webhook RSA 公钥对随机 AES Key 加密后的数据     |

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

ClearOnes 在收到非200成功状态码以及响应内容非以上成功格式时，会认为此次推送失败，会再次进行重试推送，再次推送的频率为
30s，1m，5m，1h，12h，24h

### 事件格式

| Parameter   | Type   | Description |
|-------------|--------|-------------|
| eventType   | string | 回调事件类型      |
| eventDetail | object | 回调事件详情      |

### 事件类型

| eventType                                 | eventDetail                                                     | Description    |
|-------------------------------------------|-----------------------------------------------------------------|----------------|
| CLIENT_STATUS_CHANGED                     | [clientDetail](#clientDetail)                                   | 创建用户状态变更       |
| FUND_ACCOUNT_CREATED                      | [fundAccountCreated](#fundAccountCreated)                       | 资金账号已创建        |
| CRYPTO_TX_CREATED                         | [transactionDetail](#transactionDetail)                         | 数字货币交易创建       |
| CRYPTO_TX_STATUS_CHANGED                  | [transactionDetail](#transactionDetail)                         | 数字货币交易状态变更     |
| FIAT_RECIPIENT_STATUS_CHANGED             | [fiatRecipientDetail](#fiatRecipientDetail)                     | 法币收款人信息变更      |
| CRYPTO_RECIPIENT_STATUS_CHANGED           | [cryptoRecipientDetail](#cryptoRecipientDetail)                 | 加密货币收款人信息变更    |
| FIAT_TX_CREATED                           | [transactionDetail](#transactionDetail)                         | 法币创建交易         |
| FIAT_TX_STATUS_CHANGED                    | [transactionDetail](#transactionDetail)                         | 法币交易状态变更       |
| CURRENCY_STATUS_CHANGED                   | [currencyStatusDetail](#currencyStatusDetail)                   | 账户币种状态变更       |
| DEPOSIT_INFO_ADD                          | [depositAddressAddDetail](#depositAddressAddDetail)             | 账户币种收款信息添加     |
| DEPOSIT_INFO_CHANGED                      | [depositAddressChangeDetail](#depositAddressChangeDetail)       | 账户币种收款信息状态变更   |
| CONNECT_TX_CREATED                        | [connectTransactionDetail](#connectTransactionDetail)           | 连接账号交易创建       |
| CONNECT_TX_STATUS_CHANGED                 | [connectTransactionDetail](#connectTransactionDetail)           | 连接账号交易状态变更     |
| SUPER_ORG_CRYPTO_RECIPIENT_CREATE         | [superOrgCryptoRecipientDetail](#superOrgCryptoRecipientDetail) | 主机构数字货币收款人信息创建 |
| SUPER_ORG_FIAT_RECIPIENT_CREATE           | [superOrgFiatRecipientDetail](#superOrgFiatRecipientDetail)     | 主机构法币收款人信息创建   |
| SUPER_ORG_CRYPTO_RECIPIENT_STATUS_CHANGED | [superOrgCryptoRecipientDetail](#superOrgCryptoRecipientDetail) | 主机构数字货币收款人信息变更 |
| SUPER_ORG_FIAT_RECIPIENT_STATUS_CHANGED   | [superOrgFiatRecipientDetail](#superOrgFiatRecipientDetail)     | 主机构法币收款人信息变更   |
| FX_PAIR_ADD                               | [fxPairDetail](#fxPairDetail)                                   | FX交易对添加        |
| FX_PAIR_UPDATE                            | [fxPairDetail](#fxPairDetail)                                   | FX交易对更新        |
| FX_PAIR_DELETE                            | [fxPairDetail](#fxPairDetail)                                   | FX交易对删除        |
| FX_TX_CREATED                             | [fxTransactionDetail](#fxTransactionDetail)                     | FX创建交易         |
| FX_TX_STATUS_CHANGED                      | [fxTransactionDetail](#fxTransactionDetail)                     | FX交易状态变更       |
| BUILD_CARD_ORDER                          | [buildCardOrder](#buildCardOrder)                               | 卡订单消息          |
| BUILD_CARD_APPLICATION_STATE              | [buildCardApplicationState](#buildCardApplicationState)         | 卡申请状态变更        |
| BUILD_CARD_STATE                          | [buildCardState](#buildCardState)                               | 卡状态变更          |
| BUILD_CARD_ACTIVATION_CODE                | [buildCardActivationCode](#buildCardActivationCode)             | 卡激活码消息         |

### 事件详情

**<div id="clientDetail"> clientDetail </div>**

| Parameter     | Type   | Description           |
|---------------|--------|-----------------------|
| customerRefId | string | 调用方唯一业务id             |
| clientId      | string | 客户的账户Id               |
| name          | string | 账号名                   |
| email         | string | 创建时的邮箱地址              |
| status        | int32  | 状态 1-审核中 2-已生效 3-审核拒绝 |

**<div id="fundAccountCreated"> fundAccountCreated </div>**

| Parameter          | Type   | Description      |
|--------------------|--------|------------------|
| clientId           | string | 客户的账户Id          |
| currencyList       | array  | 币种列表             | -     |
| └─currencyKey      | string | 币种唯一标识           | -     |
| └─currencyCategory | int32  | 币种分类 1-数字货币 2-法币 | -     |

**<div id="fiatRecipientDetail"> fiatRecipientDetail </div>**

| Field                  | Type   | Description                        | Since |
|------------------------|--------|------------------------------------|-------|
| customerRefId          | string | 调用方唯一业务id                          | -     |
| recipientId            | string | 收款方地址id                            | -     |
| channelKey             | string | 法币-[转账通道](#channelKey)             | -     |
| subChannelKey          | string | 法币-[转账子通道](#channelKey)            | -     |
| status                 | int32  | 收款人状态(1:审批中；2:已生效；3:审批拒绝)          | -     |
| currencyKey            | string | 币种标识                               | -     |
| swiftCode              | string | 收款银行swift码                         | -     |
| bankCode               | string | 收款银行代号                             | -     |
| branchCode             | string | 收款银行分行code                         | -     |
| bankName               | string | 收款银行名称                             | -     |
| bankCountryCode        | string | 收款银行国家ISO code                     | -     |
| bankAddress            | string | 收款银行地址                             | -     |
| sortCode               | string | Sort Code                          | -     |
| beneficiaryRoutingCode | string | Routing Code                       | -     |
| beneficiaryAccountNo   | string | 收款人银行账户号码/IBAN                     | -     |
| beneficiaryName        | string | 银行账号持有者姓名                          | -     |
| beneficiaryEntityType  | string | 收款人实体类型（individual：个人；company：公司；） | -     |
| beneficiaryCompanyName | string | 收款人公司名                             | -     |
| beneficiaryFirstName   | string | 收款人first name                      | -     |
| beneficiaryLastName    | string | 收款人last name                       | -     |
| beneficiaryCountryCode | string | 收款人国家ISO code                      | -     |
| beneficiaryStreet      | string | 收款人街道                              | -     |
| beneficiaryCity        | string | 收款人城市                              | -     |
| beneficiaryState       | string | 收款人州/省                             | -     |
| beneficiaryPostalCode  | string | 收款人邮编                              | -     |
| beneficiaryIdNumber    | string | 收款人证件号                             | -     |
| beneficiaryPhoneNumber | string | 收款人手机号                             | -     |
| conetId                | int64  | conet收款方式对方conetId                 | -     |
| note                   | string | 备注                                 | -     |
| label                  | string | 别称                                 | -     |

**<div id="cryptoRecipientDetail"> cryptoRecipientDetail </div>**

| Field         | Type   | Description               | Since |
|---------------|--------|---------------------------|-------|
| customerRefId | string | 调用方唯一业务id                 | -     |
| recipientId   | string | 收款方地址id                   | -     |
| currencyKey   | string | 币种标识                      | -     |
| address       | string | 加密货币地址                    | -     |
| label         | string | 别称                        | -     |

**<div id="transactionDetail"> transactionDetail </div>**

| Field | Type | Description | Since |
|-------|------|-------------|-------|
|customerRefId|string|调用方唯一业务ID|-|
|transactionNo|string|交易号|-|
|clientId|string|客户的账户ID|-|
|createTimestamp|int64|创建时间，UNIX 时间戳毫秒数|-|
|completedTimestamp|int64|完成时间，UNIX 时间戳毫秒数|-|
|transferCurrencyKey|string|转账币种唯一标识|-|
|currencyCategory|int32|币种类别(1:数字货币;2:法币;)|-|
|transactionType|int32|交易类型（1：接收；2：发送；5：退款；）|-|
|payAccountName|string|付款方名称|-|
|beneficiaryName|string|收款方名称|-|
|transactionAmount|string|交易数量|-|
|transactionStatus|string|交易状态（SUBMITTED:审批中；PROCESSING:处理中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；UPLOADING_PROOF:待上传入账凭证；UPLOADED_PROOF:已上传凭证；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；）|-|
|transactionSubStatus|string|交易子状态|-|
|platformFee|string|平台手续费|-|
|additionalFee|string|附加手续费（转账手续费=平台手续费+附加手续费）|-|
|note|string|备注|-|
|beneficiaryId|int64|收款人ID|-|
|fiatFeeMethod|int32|法币手续费方式（1：支付本地银行服务费；2：支付本地银行服务费与收款行服务费；）|-|
|channelKey|string|[转账通道](#channelKey)|-|
|subChannelKey|string|[转账子通道](#channelKey)，当转账方式为“local”时，需要指定子类型|-|
|proofEn|string|需要上传凭证的英文说明|-|
|proofCn|string|需要上传凭证的中文说明|-|
|cryptoBlockHeight|int64|数字货币区块高度|-|
|cryptoFromAddress|string|数字货币交易来源地址|-|
|cryptoToAddress|string|数字货币交易目标地址|-|
|cryptoTxHash|string|数字货币交易hash|-|
|cryptoTxFee|string|数字货币链上手续费|-|
|hasTransferNotice|boolean|是否可下载转账凭证|-|

**<div id="currencyStatusDetail"> currencyStatusDetail </div>**

| Field            | Type   | Description      | Since |
|------------------|--------|------------------|-------|
| clientId         | string | 客户的账户ID          | -     |
| currencyCategory | int32  | 币种分类 1-数字货币 2-法币 | -     |
| currencyKey      | string | 币种key            | -     |
| status           | int32  | 状态 1:未生效 2:已生效   | -     |

**<div id="depositAddressAddDetail"> depositAddressAddDetail </div>**

| Field                  | Type   | Description                                  | Since |
|------------------------|--------|----------------------------------------------|-------|
| clientId               | string | 客户的账户ID                                      | -     |
| depositAddressList     | array  | 地址列表                                         | -     |
| └─conetId              | int64  | 用于内部转账的收款账号id                                | -     |
| └─currencyCategory     | int32  | 币种分类 1-数字货币 2-法币                             | -     |
| └─currencyKey          | string | 币种标识                                         | -     |
| └─currencyName         | string | 币种名                                          | -     |
| └─channelKey           | string | 币种-[转账通道](#channelKey)                       | -     |
| └─subChannelKey        | string | 法币-[转账子通道](#channelKey),当channelKey=local时有值 | -     |
| └─bankAccountType      | int32  | 法币-银行账号类型 1-CA 2-VA                          | -     |
| └─bankName             | string | 法币-银行名称                                      | -     |
| └─bankAddress          | string | 法币-银行地址                                      | -     |
| └─bankCode             | string | 法币-银行代码                                      | -     |
| └─branchCode           | string | 法币-分行代码                                      | -     |
| └─swiftCode            | string | 法币-SWIFT                                     | -     |
| └─bankCountry          | string | 法币-银行国家                                      | -     |
| └─beneficiaryAccountNo | string | 法币-银行账号                                      | -     |
| └─beneficiaryName      | string | 法币-收款人姓名                                     | -     |
| └─beneficiaryCountry   | string | 法币-收款人国家                                     | -     |
| └─beneficiaryAddress   | string | 法币-收款人地址                                     | -     |
| └─note                 | string | 法币-转入需要的附言                                   | -     |

**<div id="depositAddressChangeDetail"> depositAddressChangeDetail </div>**

| Field                | Type   | Description                                  | Since |
|----------------------|--------|----------------------------------------------|-------|
| clientId             | string | 客户的账户ID                                      | -     |
| conetId              | int64  | 用于内部转账的收款账号id                                | -     |
| currencyCategory     | int32  | 币种分类 1-数字货币 2-法币                             | -     |
| currencyKey          | string | 币种标识                                         | -     |
| currencyName         | string | 币种名                                          | -     |
| channelKey           | string | 币种-[转账通道](#channelKey)                       | -     |
| subChannelKey        | string | 法币-[转账子通道](#channelKey),当channelKey=local时有值 | -     |
| bankAccountType      | int32  | 法币-银行账号类型 1-CA 2-VA                          | -     |
| bankName             | string | 法币-银行名称                                      | -     |
| bankAddress          | string | 法币-银行地址                                      | -     |
| bankCode             | string | 法币-银行代码                                      | -     |
| branchCode           | string | 法币-分行代码                                      | -     |
| swiftCode            | string | 法币-SWIFT                                     | -     |
| bankCountry          | string | 法币-银行国家                                      | -     |
| beneficiaryAccountNo | string | 法币-银行账号                                      | -     |
| beneficiaryName      | string | 法币-收款人姓名                                     | -     |
| beneficiaryCountry   | string | 法币-收款人国家                                     | -     |
| beneficiaryAddress   | string | 法币-收款人地址                                     | -     |
| status               | int32  | 状态 1-关闭 2-开启                                 | -     |
| note                 | string | 法币-转入需要的附言                                   | -     |

**<div id="connectTransactionDetail"> connectTransactionDetail </div>**

| Field                | Type   | Description                                                                                                                   | Since |
|----------------------|--------|-------------------------------------------------------------------------------------------------------------------------------|-------|
| customerRefId        | string | 调用方唯一业务ID                                                                                                                     | -     |
| createTimestamp      | int64  | 创建时间，UNIX 时间戳毫秒数                                                                                                              | -     |
| transactionNo        | string | 交易号                                                                                                                           | -     |
| clientId             | string | 客户的账户ID                                                                                                                       | -     |
| accountNo            | string | 账号编号                                                                                                                          | -     |
| currencyKey          | string | 币种唯一标识                                                                                                                        | -     |
| transactionType      | int8   | 交易类型（1:接收；2:发送；）                                                                                                              | -     |
| txAmount             | string | 交易金额                                                                                                                          | -     |
| transactionStatus    | string | 交易状态（SUBMITTED:审批中；PROCESSING:处理中；SIGNING:签名中；BROADCASTING:广播中；CONFIRMING:确认中；SUCCESS:成功；FAILED:失败；CANCELLED:取消；REJECTED:拒绝；） | -     |
| transactionSubStatus | string | 交易子状态                                                                                                                         | -     |
| note                 | string | 备注                                                                                                                            | -     |
| blockHeight          | int64  | 区块高度                                                                                                                          | -     |
| fromAddress          | string | 交易来源地址                                                                                                                        | -     |
| toAddress            | string | 交易目标地址                                                                                                                        | -     |
| txHash               | string | 交易hash                                                                                                                        | -     |
| txFee                | string | 交易手续费                                                                                                                         | -     |

**<div id="superOrgCryptoRecipientDetail"> superOrgCryptoRecipientDetail </div>**

| Field       | Type   | Description                        | Since |
|-------------|--------|------------------------------------|-------|
| recipientId | string | 收款方地址id                            | -     |
| channelKey  | string | [转账通道](#channelKey)（crypto；conet；） | -     |
| currencyKey | string | 币种标识                               | -     |
| status      | int32  | 收款人状态（2：已生效；3：已删除；）                | -     |
| conetId     | int64  | conet收款方式对方conetId                 | -     |
| address     | string | 加密货币地址                             | -     |
| label       | string | 别称                                 | -     |

**<div id="superOrgFiatRecipientDetail"> superOrgFiatRecipientDetail </div>**

| Field       | Type   | Description                 | Since |
|-------------|--------|-----------------------------|-------|
| recipientId | string | 收款方地址id                     | -     |
| channelKey  | string | [转账通道](#channelKey)（conet；） | -     |
| currencyKey | string | 币种标识                        | -     |
| status      | int32  | 收款人状态（2：已生效；3：已删除；）         | -     |
| conetId     | int64  | conet收款方式对方conetId          | -     |
| label       | string | 别称                          | -     |

**<div id="fxPairDetail"> fxPairDetail </div>**

| Field                                | Type   | Description                                                        | Since |
|--------------------------------------|--------|--------------------------------------------------------------------|-------|
| weeklyLimitUsd                       | number | 周交易限额（单位：USD），统计范围：按照东八区本周周一至周日，toCurrencyKey兑换数量按照兑换时汇率折算为USD进行统计 | -     |
| pairs                                | array  | 交易对列表                                                              | -     |
| &nbsp;&nbsp;&nbsp;└─fromCurrencyKey  | string | 付款币种Key                                                            | -     |
| &nbsp;&nbsp;&nbsp;└─toCurrencyKey    | string | 收款币种Key                                                            | -     |
| &nbsp;&nbsp;&nbsp;└─exchangeRate     | string | 兑换汇率                                                               | -     |
| &nbsp;&nbsp;&nbsp;└─feeRate          | string | 手续费费率                                                              | -     |
| &nbsp;&nbsp;&nbsp;└─minAmount        | string | 最小兑换fromCurrencyKey数量                                              | -     |
| &nbsp;&nbsp;&nbsp;└─maxAmount        | string | 最大兑换fromCurrencyKey数量                                              | -     |
| &nbsp;&nbsp;&nbsp;└─thresholdAmount  | string | 高手续费阈值金额(fromAmount < thresholdAmount时，手续费率使用thresholdFeeRate)     | -     |
| &nbsp;&nbsp;&nbsp;└─thresholdFeeRate | string | 高手续费率                                                              | -     |

**<div id="fxTransactionDetail"> fxTransactionDetail </div>**

| Field              | Type   | Description                                                                                                                                                                                  | Since |
|--------------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------|
| customerRefId      | string | 调用方唯一业务ID                                                                                                                                                                                    | -     |
| createTimestamp    | int64  | 创建时间，UNIX 时间戳毫秒数                                                                                                                                                                             | -     |
| completedTimestamp | int64  | 完成时间，UNIX 时间戳毫秒数                                                                                                                                                                             | -     |
| transactionNo      | string | 交易号                                                                                                                                                                                          | -     |
| clientId           | string | 客户的账户ID                                                                                                                                                                                      | -     |
| transactionStatus  | string | 交易状态<br/><br/><pre><br/>SUBMITTED：已提交，此状态时用户账户已经冻结交易金额<br/>PROCESSING：进行中，此状态时用户账户已发起转账，等待交易完成<br/>SUCCESS：成功，用户转账完成并且已收到对应币种<br/>FAILED：失败，失败原因有用户转账失败、用户收款失败等。如用户转账完成后失败，会退款给用户<br/></pre> | -     |
| fromCurrencyKey    | string | 付款币种Key                                                                                                                                                                                      | -     |
| toCurrencyKey      | string | 收款币种Key                                                                                                                                                                                      | -     |
| exchangeRate       | string | 兑换汇率                                                                                                                                                                                         | -     |
| fromAmount         | string | 付款币种数量                                                                                                                                                                                       | -     |
| toAmount           | string | 收款币种数量                                                                                                                                                                                       | -     |
| feeRate            | string | 总服务费费率（平台服务费费率+附加服务费费率）                                                                                                                                                                      | -     |
| fee                | string | 总服务费                                                                                                                                                                                         | -     |
| feeCurrencyKey     | string | 服务费币种Key                                                                                                                                                                                     | -     |
| additionalFeeRate  | string | 附加服务费费率                                                                                                                                                                                      | -     |
| additionalFee      | string | 附加服务费                                                                                                                                                                                        | -     |
| transferNo         | string | 转账交易号                                                                                                                                                                                        | -     |
| receiveNo          | string | 收款交易号                                                                                                                                                                                        | -     |
| refundNo           | string | 退款交易号                                                                                                                                                                                        | -     |

**<div id="buildCardOrder"> buildCardOrder </div>**

| Field               | Type    | Description | Since |
|---------------------|---------|-------------|-------|
| id                  | string  | 消息id        | -     |
| orderNo             | string  | 订单id        | -     |
| uid                 | string  | 用户id        | -     |
| cardId              | string  | 卡id         | -     |
| orderCurrency       | string  | 结算币种        | -     |
| orderAmount         | string  | 订单金额        | -     |
| settleAmount        | string  | 结算金额        | -     |
| reversalAmount      | string  | 退款金额        | -     |
| orderStatus         | string  | 订单状态        | -     |
| transactionType     | string  | 交易类型        | -     |
| merchantName        | string  | 卡支付商户名称     | -     |
| createTime          | int64   | 创建时间        | -     |
| updateTime          | int64   | 修改时间        | -     |
| orderType           | string  | 订单类型        | -     |
| transactionAmount   | string  | 交易金额        | -     |
| transactionCurrency | string  | 交易币种        | -     |
| cardPresent         | boolean | 是否有卡        | -     |
| country             | string  | 交易国家        | -     |
| city                | string  | 交易城市        | -     |
| mcc                 | string  | 交易mcc       | -     |
| acquiringCurrency   | string  | 收单币种        | -     |
| acquiringAmount     | string  | 收单金额        | -     |
| version             | string  | 版本号         | -     |
| relatedOrderNo      | string  | 关联的订单id     | -     |

**<div id="buildCardApplicationState"> buildCardApplicationState </div>**

| Field         | Type   | Description                                                                  | Since |
|---------------|--------|------------------------------------------------------------------------------|-------|
| applicationId | string | 申请id                                                                         | -     |
| status        | string | 状态,PENDING_INITIALIZATION, UNDER_REVIEW, REJECTED, APPROVED, SUCCESS, FAILED | -     |
| timestamp     | int64  | 时间戳                                                                          | -     |

**<div id="buildCardState"> buildCardState </div>**

| Field         | Type   | Description                                                                                        | Since |
|---------------|--------|----------------------------------------------------------------------------------------------------|-------|
| cardId        | string | 卡id                                                                                                | -     |
| applicationId | string | 申请id                                                                                               | -     |
| status        | string | 状态,NEW, CREATED, PRE_ACTIVATION, DISPATCHED, INVALID, ACTIVE, TEMP_BLOCKED, PERM_BLOCKED, REJECTED | -     |
| timestamp     | int64  | 时间戳                                                                                                | -     |

**<div id="buildCardActivationCode"> buildCardActivationCode </div>**

| Field            | Type   | Description    | Since |
|------------------|--------|----------------|-------|
| cardId           | string | 卡id            | -     |
| activationCode   | string | 激活码            | -     |
| activationMethod | string | 激活方式,SMS,EMAIL | -     |
| expirationTime   | int64  | 失效时间           | -     |

## 错误码列表

| Error code | Description                      |
|------------|----------------------------------|
| 401        | 未登录                              |
| 412        | 参数错误                             |
| 413        | 远程调用参数错误                         |
| 414        | 远程调用业务异常                         |
| 415        | 远程调用超时                           |
| 500        | 系统异常                             |
| 10101      | 验证签名失败                           |
| 10102      | 解密请求失败                           |
| 10103      | 重复提交                             |
| 10104      | 请求频繁，请稍后重试                       |
| 10105      | 幂等调用                             |
| 10106      | IP拒绝                             |
| 10201      | 钱包名称不能超过{0}个字符                   |
| 10202      | 只能删除余额小于10美元的账号                  |
| 10203      | 账号已冻结                            |
| 10204      | 审批人数量大于管理员数量                     |
| 10205      | 钱包账号不支持该币种                       |
| 10206      | 存在进行中的审批                         |
| 10207      | 冻结时间必须是将来时间                      |
| 10208      | 账号不存在                            |
| 10209      | 账号不支持该币种                         |
| 10301      | 输入错误次数过多，请{0}分钟后再试               |
| 10302      | 验证码无效，在您的账户被锁定{1}分钟前，您还有{0}次尝试机会 |
| 10303      | 验证码无效，您还有{0}次尝试机会                |
| 10401      | 用户已存在                            |
| 10402      | 用户不存在                            |
| 10403      | 不能删除审批中的用户                       |
| 10404      | 不能删除自己                           |
| 10405      | 当前钱包存在审批，无法删除钱包管理员               |
| 10406      | 在删除管理员之前需要先减少审批数量                |
| 10501      | 钱包提币白名单标签不能超过20个字符               |
| 10502      | 钱包提币白名单地址不能超过200个字符              |
| 10503      | 该白名单地址已存在                        |
| 10504      | 不能删除审批中的白名单                      |
| 10601      | 提币规则已存在                          |
| 10701      | 接收地址格式错误                         |
| 10702      | 您的收款地址有风险，不能转账                   |
| 10703      | 目标地址不能为合约地址                      |
| 10704      | 钱包资产余额不足，无法提币                    |
| 10705      | 预估网络费用不足，您需要向此钱包至少存入{0}来支付提币网络费用 |
| 10706      | 提币数量小数精度不能超过{0}                  |
| 10707      | 提币数量超过组织限额                       |
| 10708      | 提币数量超过限制                         |
| 10709      | 暂停充币                             |
| 10710      | 暂停提币                             |
| 10711      | 钱包被冻结，无法提币                       |
| 10712      | 更新二次验证码后，24小时内禁止提币               |
| 10713      | 更新登录密码后，24小时内禁止提币                |
| 10714      | 该机构已失效，无法提币                      |
| 10715      | 收款账号未生效，不能收款                     |
| 10801      | 该机构已失效，无法转账                      |
| 10802      | 暂停充值                             |
| 10803      | 暂停转账                             |
| 10804      | 钱包被冻结，无法转账                       |
| 10805      | 更新二次验证码后，24小时内禁止转账               |
| 10806      | 更新登录密码后，24小时内禁止转账                |
| 10807      | 转账数量小数精度不能超过{0}                  |
| 10808      | 单笔转账最低{0} {1}                    |
| 10808      | 单笔转账最大{0} {1}                    |
| 10809      | 托管⾏不同，请选择银行转账                    |
| 10810      | 收款账号被冻结，不能收款                     |
| 10811      | 收款机构已失效，无法收款                     |
| 10812      | 转账手续费已经变更，请重新提交                  |
| 10813      | 准备金不足                            |
| 10814      | 币种不存在                            |
| 10815      | 手续费规则不存在                         |
| 10816      | 转账附加服务费小数精度不能超过{0}                       |
| 10817      | 转账附加服务费最低{0} {1}                       |
| 10901      | 操作授权验证失败                         |
| 10902      | 收款人不存在                           |
| 10903      | 收款人信息错误                          |
| 10904      | 收款人状态不允许删除                       |
| 10905      | 收款人添加通道已关闭                       |
| 11001      | 交易记录不存在                          |
| 11002      | 当前交易状态不允许的操作                     |
| 11101      | 账单自动付款方式已存在                      |
| 11102      | 账单已经支付                           |
| 11103      | 账单已经存在支付请求                       |
| 11104      | 账单支付超时                           |
| 11105      | 超过自动支付限额                         |
| 11106      | 账户余额不足                           |
| 11201      | 只能添加一个CA账号                       |
| 11202      | 银行不存在                            |
| 11203      | 银行不支持创建虚拟账号                      |
| 11204      | 只能添加一种VA账号                       |
| 11301      | 冻结金额大于可用余额                       |
| 11302      | 解冻金额大于冻结金额                       |
| 11401      | 该机构已失效，无法交易                      |
| 11402      | 该机构未开通交易服务                       |
| 11403      | 交易对不可用                           |
| 11404      | 钱包被冻结，无法交易                       |
| 11405      | 更新二次验证码后，24小时内禁止交易               |
| 11406      | 更新登录密码后，24小时内禁止交易                |
| 11407      | 交易数量小数精度不能超过{0}                  |
| 11408      | 交易数量超过限额                         |
| 11409      | 钱包资产余额不足，无法交易                    |
| 11410      | 兑换金额过大，请联系客服处理                   |
| 11411      | 单笔交易最低{0} {1}                    |
| 11412      | 单笔交易最大{0} {1}                    |
| 11413      | 交易出售数字货币gas不足                    |
| 11414      | 总服务费费率小于0                        |
| 11415      | 创建交易汇率失效                         |
| 11417      | 获取汇率异常                           |
| 11413      | Gas不足                            |
| 11501      | 汇率异常                             |
| 11801      | 钱包资金来源存疑                         |
| 11901      |业务已暂停使|


