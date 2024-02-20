# Explore APIs

#### URL -> https://api.deaslide.com


<p align="left">
  <img src="https://www.svgrepo.com/show/402882/warning.svg" width="20" alt="alt"> Обрати внимание на то, что теперь пути запросов не имеют приставки <a>/api</a> вместо нее используется приставка с версией конечных точек
</p>


<p>Дам пояснения по эндпойнтам для оперирования сделками. 
Ответ возвращают только точки <a>/get-deal</a> и <a>/init-deal</a>, 
возвращают они ответ типа <a href="#deal-pojo">Deal POJO</a>, 
остальные эндпойнты лишь принимают данные. 
Они принимают и обрабатывают данные, 
далее посылают их на нужный топик веб-сокета, 
куда нужно подписать клиента после инициализации сделки. 
Веб сокет доступен по пути -> <a>ws://api.deaslide/ws</a>, 
подключение через SockJS осуществляется в формате пути -> <a>https://**</a><br/>
Думаю дальше разберешся, ниже пример подключения к сокету</p>

```javascript
var socket = new SockJS('https://api.deaslide.com/ws');
var stompClient = Stomp.over(socket);

stompClient.connect({}, function(frame) {
    console.log('Connected: ' + frame);
    stompClient.subscribe('/topic/deal/{dealId}', function(greeting){
        console.log(JSON.parse(greeting.body).content);
    });
});

```

## Auth

| Method | URL                    | Description          | Request                | Auth Header |
|--------|------------------------|----------------------|------------------------|-------------|
| POST   | /v1/auth/signup        | Sign up              | [JSON](#signup)        | false       |
| POST   | /v1/auth/signin        | Log in               | [JSON](#signin)        | false       |
| POST   | /v1/auth/refresh-token | Refresh access token | [JSON](#refresh-token) | false       |

## P2P

| Method | URL                          | Description                               | Request | Auth Header  |
|--------|------------------------------|-------------------------------------------|---------|--------------|
| GET    | /v1/p2p/get-banks            | Get provided currencies for display banks |         | true         |
| GET    | /v1/p2p/get-banks/{currency} | Get provided banks by currency            |         | true         |
| GET    | /v1/p2p/rates/{currency}     | Get crypto rates                          |         | false        |

## Orders
| Method | URL                            | Description        | Request               | Auth Header  |
|--------|--------------------------------|--------------------|-----------------------|--------------|
| GET    | /v1/p2p/get-orders             | Get user orders    |                       | true         |
| GET    | /v1/p2p/get-order/{orderId}    | Get order by id    |                       | true         |
| POST   | /v1/p2p/create-order           | Create order       | [JSON](#create-order) | true         |
| GET    | /v1/p2p/cancel-order/{orderId} | Cancel order by id |                       | true         |
| POST   | /v1/p2p/get-orders?page=1      | Get orders         | [JSON](#get-orders)   | true         |


## Deals

| Method | URL                              | Description         | Request            | Auth Header |
|--------|----------------------------------|---------------------|--------------------|-------------|
| GET    | /v1/p2p/get-deals                | Get user deals      |                    | true        |
| GET    | /v1/p2p/get-deal/{dealId}        | Get info by deal    |                    | true        |
| POST   | /v1/p2p/init-deal                | Deal initialization | [JSON](#init-deal) | true        |
| GET    | /v1/p2p/cancel-deal/{dealId)     | Cancel deal         |                    | true        |
| GET    | /v1/p2p/accept-deal/{dealId)     | Accept deal         |                    | true        |
| GET    | /v1/p2p/reject-deal/{dealId}     | Reject deal         |                    | true        |
| GET    | /v1/p2p/make-payment/{dealId}    | Make payment        |                    | true        |
| GET    | /v1/p2p/confirm-payment/{dealId} | Confirm payment     |                    | true        |
| GET    | /v1/p2p/proof-payment/{dealId}   | Proof payment       |                    | true        |



## User

| Method | URL                                   | Description                 | Request               | Auth Header |
|--------|---------------------------------------|-----------------------------|-----------------------|-------------|
| GET    | /v1/user/details                      | Get user details            |                       | true        |
| POST   | /v1/user/payment-methods              | Add payment method          | [JSON](#add-payment)  | true        |
| DELETE | /v1/user/payment-methods?id=paymentId | Delete payment method by id |                       | true        |
| GET    | /v1/user/payment-methods              | Get all user payments       |                       | true        |

## Wallet

| Method | URL                            | Description                 | Request              | Auth Header |
|--------|--------------------------------|-----------------------------|----------------------|-------------|
| GET    | /v1/wallet/balances            | Get all provided currencies |                      | true        |
| GET    | /v1/wallet/balances/{currency} | Get balances by currency    |                      | true        |
| POST   | /v1/wallet/send                | Send tokens                 | [JSON](#send-tokens) | true        |


## JSON Request Bodies

##### <a id="signup">Sign Up -> /v1/auth/signup</a>
```json
{
  "username": "deaslide",
  "email": "user@deaslide.com",
  "password": "password"
}
```

##### <a id="signin">Log In -> /v1/auth/signin</a>
```json
{
  "email": "user@deaslide.com",
  "password": "password"
}
```
##### <a id="refresh-token">Refresh token -> /v1/auth/refresh-token</a>
```json
{
  "refreshToken": "728c3301-5ec3-44eb-9b00-617f6fbfd62e"
}
```

##### <a id="create-order">Create order -> /v1/p2p/create-order</a>
```json
{
  "type": "SELL",
  "assetId": "659b080aa8cc125d77d4e380",
  "currency": "RUB",
  "priceType": "FIXED",
  "price": 1000,
  "amount": 1000,
  "minSum": 100,
  "paymentTime": 15,
  "comment": "comment",
  "payments": [
    "659d82f2da858d4c0ebe02e3"
  ]
}
```

##### <a id="get-orders">Get orders -> /v1/p2p/get-orders</a>
###### Request params:
    page: Integer
```json
{
  "type": "SELL",
  "currency": "RUB",
  "assetId": "659b080aa8cc125d77d4e380",
  "bankIds": [ // для всех методов оплаты оставить пустым
    "659b080aa8cc125d77d4e380",
    "659b080aa8cc125d77d4e380"
  ]
}
```

##### <a id="init-deal">Deal initialization -> /v1/p2p/init-deal</a>
```json
{
  "type": "SELL",
  "orderId": "659b080aa8cc125d77d4e380",
  "amount": 274.12,
  "paymentId": "659d82f2da858d4c0ebe02e3"
}
```

##### <a id="add-payment">Add payment method -> /v1/user/payment-methods</a>
```json
{
  "bankId": "659edfb2405a863a5edd51b6",
  "account": "1111222233334444"
}
```

##### <a id="send-tokens">Send tokens -> /v1/wallet/send</a>
```json
{
  "address": "3PJHfVz5obiTaBgK6Q83GMB9vrFHKbL1Yab",
  "amount": 30.24,
  "assetId": "659efb33acf9fe34333b6e20",
  "feeAssetId": "659efb33acf9fe34333b6e20",
  "totpCode": ""
}
```

#### <a id="deal-pojo">Deal POJO</a>
```json
{
  "dealId": "659efb33acf9fe34333b6e20",
  "status": "INITIALIZED",
  "assetId": "659efb33acf9fe34333b6e20",
  "assetAlias": "YUSRA",
  "amount": 1245,
  "sum": 22708.8,
  "price": 18.24,
  "currency" : "RUB",
  "paymentTime" : 15,
  "chatAvailable" : true,
  "payment" : { // payment также может быть null, он доступен если status не равен INITIALIZED или OPENED
    "id": "65d3258f97ba9a0aac8b07a9", 
    "type": "BANK",
    "account": "79999999999",
    "bank": {
      "id": "65d324fd97ba9a0aac8b07a6",
      "name": "Тинькофф"
    }
  },
  "maker": {
    "username": "username", 
    "role": "SELLER",
    "deals": "159", 
    "completedPercent": "98"
  },
  "taker": {
    "username": "username",
    "role": "BUYER",
    "deals": "159",
    "completedPercent": "98"
  },
  "createdAt" : 1708336769881
}
```
