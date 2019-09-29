---
search:
  keyword: ['gamepot']
---

# GAMEPOT 서버 > 게임 서버

## Purchase Webhook(required)

결제 완료 후에 GAMEPOT서버에서 게임 서버로 아이템 지급을 위해 HTTP 요청을 합니다.

> GAMEPOT은 클라이언트로부터 전달받은 결제 정보를 토대로 스토어 영수증 검증까지 진행하여 불법 결제 시도를 막고 있습니다.

###Request

HTTP 요청 시 정보는 아래와 같은 내용으로 전달드리고 해당 정보를 바탕으로 유저에게 아이템을 지급하시면 됩니다.

> `{domain}` 항목은 개발사에서 작업 완료 후 알려주어야 하며, 이 값은 GAMEPOT 대시보드 - 프로젝트 설정 - 일반 - Webhook 항목에 추가해야 합니다.

```java
https://{domain}?
userId={uuid}&orderId={orderId}&projectId={projectId}&platform={platform}&productId={productId}&store={store}&payment={payment}&transactionId={transactionId}&gamepotOrderId={gamepotOrderId}&uniqueId={uniqueId}
```

| Attribute      | Type   | Description                                              |
| -------------- | ------ | -------------------------------------------------------- |
| userId         | String | 사용자 UID                                               |
| transactionId  | String | 주문번호(GPA-xxxx-xxxx-)                                 |
| store          | String | 스토어 정보(ios, google, one)                            |
| projectId      | String | 프로젝트ID                                               |
| productId      | String | 구글/애플/원스토어에 등록된 상품ID                       |
| platform       | String | 운영 Platform 정보 (android, ios)                        |
| payment        | String | 결제 방식 ( apple, google, one, danal, mycard, mol ... ) |
| uniqueId       | String | Unique id (purchase api 호출 시 넣은 unique id)          |
| gamepotOrderId | String | GAMEPOT Order id                                         |

###Response

응답은 아래와 같은 형식으로 해주세요.

```json
{
    "status": 1,
    "message" : ""
}
```

| Attribute | Type   | Description                 |
| --------- | ------ | --------------------------- |
| status    | Int    | 결과값 ( 0: 실패, 1: 성공 ) |
| message   | String | 오류 내용                   |

## Item Webhook(required)

쿠폰 사용 후 GAMEPOT서버에서 게임 서버로 아이템 지급을 위해 HTTP 요청을 합니다.

###Request

HTTP 요청 시 정보는 아래와 같은 내용으로 전달드리고 해당 정보를 바탕으로 유저에게 아이템을 지급하시면 됩니다.

> `{domain}` 항목은 개발사에서 작업 완료 후 알려주어야 하며, 이 값은 GAMEPOT 대시보드 - 프로젝트 설정 - 일반 - Webhook 항목에 추가해야 합니다.

```web-idl
https://{domain}?
userId={userId}&projectId={projectId}&platform={platform}&store={store}&userData={userData}&itemId=[{itemData}, {itemData}, ...]
```

| Attribute | Type            | Description                                                  |
| --------- | --------------- | ------------------------------------------------------------ |
| userId    | String          | 사용자 ID                                                    |
| projectId | String          | Project ID                                                   |
| platform  | String          | 운영 Platform 정보 (Android, IOS)                            |
| store     | String          | 스토어 정보(ios, google, one)                                |
| userData  | String          | coupon api 호출 시 두 번째 파라미터에 넣은 값                |
| itemId    | Array<itemData> | itemData Array<br /><br />- itemData(JSON) <br /> {"item_id" : String, "store_item_id" : String, "count" : Number} |

> ex) 
>
> https://{domain}?itemId=[{"item_id":"d0781c4e-df52-465b-ab93-0ee16fbf445d","store_item_id":"ttt","count":1}]&platform=android&projectId=f1df9464-40a8-4a66-8421-196c7c661002&store=google&userId=2d485044-06c2-48c4-a6ed-4ab53dea88bb

###Response

응답은 아래와 같은 형식으로 해주세요.

```json
{
	"status": 1,
	"message" : ""
}
```

| Attribute | Type   | Description                 |
| --------- | ------ | --------------------------- |
| status    | Int    | 결과값 ( 0: 실패, 1: 성공 ) |
| message   | String | 오류 내용                   |

# GAMEPOT 서버 > 게임 서버

## Token Authentication(optional)

게임에서 로그인 완료 후 얻은 정보로 GAMEPOT 서버에 로그인 검증을 할 수 있습니다.

토큰 검증은 2 단계로 진행되며, 1단계 GAMEPOT 토큰 검증 후 2단계 소셜 토큰 검증이 진행됩니다.

> 2단계 검증은 GAMEPOT 대시보드 - 프로젝트 설정 - Auth key에 값이 입력되어 있어야 합니다.

1단계 GAMEPOT 토큰 검증은 로그인 후 얻은 MemberID, Token으로 동기 토큰 검증을 진행하며,

2단계 소셜 토큰 검증은 구글 로그인이나 페북 로그인을 통해 얻은 소셜 계정의 토큰으로 소셜 비동기 토큰 검증을 진행합니다.

2단계에서 검증 실패한 경우에는 GAMEPOT 이용정지 기능이 동작되어 이후 재 로그인 시 로그인이 불가합니다.

> 검증 실패에 따른 이용정지는 GAMEPOT 대시보드 - 프로젝트 설정 - Auth key 항목에서 스위치가 ON되어 있는 경우에만 동작하며, 이용정지 대상자는 GAMEPOT 대시보드 - 회원 - 이용정지 메뉴에서 확인이 가능합니다.

###Request

데이터 무결성을 위해 클라이언트가 아닌 개발사 서버에서 진행 해주세요.

> {API URL} 항목은 GAMEPOT 대시보드 주소에서 `:8080` 를 제외한 주소입니다.

```web-idl
POST
url : https://{API URL}/loginauth
Header : 'content-type: application/json'
data: 
{
	"projectId": {GamePot SDK의 projectId},
	"memberId": {GamePot SDK의 memberId},
	"token": {GamePot SDK의 Token}
}
```

| Attribute | Type   | Description             |
| --------- | ------ | ----------------------- |
| projectId | String | GamePot SDK의 projectId |
| memberId  | String | GamePot SDK의 memberId  |
| token     | String | GamePot SDK의 Token     |

###Response

1단계 검증에서 검증 실패하는 경우 status가 1이 아닌 값이 입력됩니다.

이 경우는 게임 진입을 제한 해주세요.

```json
{
	"status": 1,
	"message" : ""
}
```

| Attribute | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| status    | Int    | 결과값 (1: 성공, 실패는 아래 Error code 참고) |
| message   | String | 오류 내용                                     |

###Error code

| Code | Description                                                  |
| ---- | ------------------------------------------------------------ |
| 0    | Body에 누락된 데이터가 있는 경우<br />HTTP 요청 시 projectId, memberId, token을 모두 넣었는지 확인해 주세요. |
| -1   | Token 검증 실패<br />Token이 조작된 경우                     |
| -2   | MemberId 검증 실패<br />Token의 MemberId 정보와 body의 MemberId가 일치하지 않은 경우 |
| -3   | Token 만료<br />SDK login api가 성공한 시각과 해당 Authentication check를 요청한 시각이 10분 이상 차이가 발생한 경우 |
