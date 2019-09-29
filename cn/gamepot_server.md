---
search:
  keyword: ['gamepot']
---

#GAMEPOT Server> Game Server

## Purchase(required)

支付完成后服务器会对游戏服务器请求发放物品。游戏服务会按照以下形式传达，依照此信息发放用户相关物品即可。

###Request

```web-idl
https://{domain}?
userId={uuid}&orderId={orderId}&projectId={projectId}&platform={platform}&productId={productId}&store={store}&payment={payment}&transactionId={transactionId}&gamepotOrderId={gamepotOrderId}&uniqueId={uniqueId}
```

| Attribute      | Type   | Description                                                  |
| -------------- | ------ | ------------------------------------------------------------ |
| userId         | String | 用户 UID                                                     |
| transactionId  | String | transaction id of Google/Apple/Onestore<br/>(ex. Google is “GPA.0000-0000-0000-00000”) |
| store          | String | 渠道信息(ios, google, one)                                   |
| projectId      | String | ProjectID                                                    |
| productId      | String | Google/Apple/Onestore 的商品 ID                              |
| platform       | String | 平台 Platform 信息 (android, ios)                            |
| payment        | String | 储值方式 (apple, google, one, mycard, mol)                   |
| uniqueId       | String | Unique ID（称为 purchase API 时的 unique id）                |
| gamepotOrderId | String | GAMEPOT Order id                                             |

###Response

```json
{
  "status": 1,
  "message": ""
}
```

| Attribute | Type   | Description                 |
| --------- | ------ | --------------------------- |
| status    | Int    | 结果值 ( 0: 失败, 1: 成功 ) |
| message   | String | 错误内容                    |

## Item(required)

使用优惠卷时会对游戏服务器请求发放物品
会依照以下形式来请求，游戏服务器就依照传达过来的内容发放给玩家相关物品即可。

###Request

```web-idl
https://{domain}?
userId={userId}&projectId={projectId}&platform={platform}&store={store}&userData={userData}&itemId=[{itemData}, {itemData}, ...]
```

| Attribute | Type            | Description                                                                                                        |
| --------- | --------------- | ------------------------------------------------------------------------------------------------------------------ |
| userId    | String          | 用户 UID                                                                                                           |
| projectId | String          | ProjectID                                                                                                          |
| platform  | String          | 平台 Platform 信息 (Android, IOS)                                                                                  |
| store     | String          | 渠道信息(ios, google, one)                                                                                         |
| userData  | String          | User data                                                                                                          |
| itemID    | Array<itemData> | itemData Array<br /><br />- itemData(JSON) <br /> {"item_id" : String, "store_item_id" : String, "count" : Number} |

ex)
https://{domain}?itemId=[{"item_id":"d892ee43-d516-43c2-b16f-3ca5672e8166","store_item_id":"000","count":1},{"item_id":"989caae1-5f70-41d9-b797-2e27cc838cb0","store_item_id":"rrr","count":2}]&platform=android&projectId=f1df9464-40a8-4a66-8421-196c7c661002&store=google&userData=abcdefg&userId=25dcea66-0719-4d18-8dcd-9b7f638f85e4

###Response

```json
{
  "status": 1,
  "message": ""
}
```

| Attribute | Type   | Description                 |
| --------- | ------ | --------------------------- |
| status    | Int    | 结果值 ( 0: 失败, 1: 成功 ) |
| message   | String | 错误内容                    |

#Game Server > GAMEPOT Server

## Authentication check(optional)

请求验证用户 ID

请求验证时，GamePot 服务器会实际的进行验证，按照验证结果会执行用户 Block。

###Request

```web-idl
POST
url : https://{GamePot API URL}/loginauth
Header : 'content-type: application/json'
data:
{
	"projectId": {GamePot SDK的projectId},
	"memberId": {GamePot SDK的memberId},
	"token": {GamePot SDK的Token}
}
```

| Attribute | Type   | Description              |
| --------- | ------ | ------------------------ |
| projectId | String | GamePot SDK 的 projectId |
| memberId  | String | GamePot SDK 的 memberId  |
| token     | String | GamePot SDK 的 Token     |

GamePot API URL : GamePot Dashboard 网址的 :8080 之前的网址。

Dashboard 网址为 https://xxxxxxx.gamepot.ntruss.com:8080/aaaaaaa-aaaaaaa-aaaaaa/dashboard/analysis 的时候 [xxxxxxx.gamepot.ntruss.com](https://0bmnkzvhzd.gamepot.ntruss.com:8080/d1516c57-f7b5-41cc-a22c-8c0aaff56259/dashboard/analysis)就指这部分。

###Response

```json
{
  "status": 1,
  "message": ""
}
```

| Attribute | Type   | Description                                                                      |
| --------- | ------ | -------------------------------------------------------------------------------- |
| status    | Int    | 结果值 (1: 成功, 请参阅下面的错误代码以了解错误。)<br />如果不是 1，请限制登录。 |
| message   | String | 错误内容                                                                         |

###错误代码

| Code | Description                                                         |
| ---- | ------------------------------------------------------------------- |
| 0    | 缺少数据<br />确保包含 projectId，memberId 和 token。               |
| -1   | 令牌验证失败<br />令牌已被篡改。                                    |
| -2   | MemberId 验证失败<br />如果令牌的 MemberId 与正文的 MemberId 不匹配 |
| -3   | 过期令牌<br />SDK 登录 api 通话时间和认证检查请求时间差超过 10 分钟 |

