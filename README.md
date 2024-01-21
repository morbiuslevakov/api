# Explore APIs

### URL -> https://api.deaslide.com

The app defines following APIs.

### Auth

| Method | Url                     | Decription    | Sample Valid Request Body | 
| ------ |-------------------------|---------------|---------------------------|
| POST   | /api/auth/signup        | Sign up       | [JSON](#signup)           |
| POST   | /api/auth/signin        | Log in        | [JSON](#signin)           |
| POST   | /api/auth/refresh-token | Refresh Token | [JSON](#refresh-token)    |

### P2P

| Method | Url                             | Description                             | Sample Valid Request Body |
|--------|---------------------------------|-----------------------------------------|---------------------------|
| POST   | /api/p2p/create-order           | Create order                            | [JSON](#create-order)     |
| GET    | /api/p2p/cancel-order/{orderId} | Cancel order by id                      |                           |
| GET    | /api/p2p/get-banks              | Get provided currencies for display banks |                           |
| GET    | /api/p2p/get-banks/{currency}   | Get provided banks by currency          |                           |
| GET    | /api/p2p/get-orders?page=1      | Get orders              | [JSON](#get-orders)       |


### Deals

| Method | Url                             | Description                      | Sample Valid Request Body |
|--------|---------------------------------|----------------------------------|---------------------------|
| POST   | /api/p2p/create-deal            | Create deal                      | [JSON](#create-deal)      |
| GET    | /api/p2p/cancel-deal/{dealId}   | Cancel deal by id                |                           |
| GET    | /api/p2p/confirm-deal/{dealId)  | Confirm deal                     |                           |
| GET    | /api/p2p/proof-payment/{dealId} | Proof payment by buyer or seller |                           |



### User

| Method | Url                                    | Description                 | Sample Valid Request Body |
|--------|----------------------------------------|-----------------------------|--------------------------|
| GET    | /api/user/details                      | Get user details            |                          |
| POST   | /api/user/payment-methods              | Add payment method          | [JSON](#add-payment)     |
| DELETE | /api/user/payment-methods?id=paymentId | Delete payment method by id |      |
| GET    | /api/user/payment-methods              | Get all user payments       |      |

### Wallet

| Method | Url                  | Description                 | Sample Valid Request Body |
|--------|----------------------|-----------------------------|---------------------------|
| GET    | /api/wallet/balances | Get all provided currencies |                           |
| GET    | /api/wallet/balances/{currency} | Get balances by currency    |                           |
| POST   | /api/wallet/send     | Send tokens                 | [JSON](#send-tokens)      |


## Sample Valid JSON Request Bodies

##### <a id="signup">Sign Up -> /api/auth/signup</a>
```json
{
	"username": "deaslide",
	"email": "user@deaslide.com",
	"password": "password"
}
```

##### <a id="signin">Log In -> /api/auth/signin</a>
```json
{
	"email": "user@deaslide.com",
	"password": "password"
}
```
##### <a id="refresh-token">Refresh token -> /api/auth/refresh-token</a>
```json
{
	"refreshToken": "728c3301-5ec3-44eb-9b00-617f6fbfd62e"
}
```

##### <a id="create-order">Create order -> /api/p2p/create-order</a>
```json
{
	"type": "SELL",
	"assetId": "659b080aa8cc125d77d4e380",
	"currency": "RUB",
	"priceType": "FIXED",
	"price": 1000,
	"amount": 1000,
	"fee": 2.5641,
	"minSum": 100,
	"paymentTime": 15,
  	"comment": "comment",
	"paymentMethods": [
      	"659d82f2da858d4c0ebe02e3"
	]
}
```

##### <a id="get-orders">Get orders -> /api/p2p/get-orders</a>
```json
{
  	"type": "SELL",
  	"currency": "RUB",
  	"asset": "YUSRA",
  	"paymentMethods": [
      	"bankId",
      	"bankId"
    ]
}
```

##### <a id="create-order">Create deal -> /api/p2p/create-deal</a>
```json
{
	"type": "SELL",
	"orderId": "659b080aa8cc125d77d4e380",
	"price": 18.24,
	"amount": 274.12,
	"sum": 5000,
	"payments": [
		"659d82f2da858d4c0ebe02e3"
	]
}
```

##### <a id="add-payment">Add payment method -> /api/user/payment-methods</a>
```json
{
	"bankId": "659edfb2405a863a5edd51b6",
	"account": "1111222233334444"
}
```

##### <a id="send-tokens">Send tokens -> /api/wallet/send</a>
```json
{
	"address": "3PJHfVz5obiTaBgK6Q83GMB9vrFHKbL1Yab",
	"amount": 8,
	"assetId": "659efb33acf9fe34333b6e20",
	"feeAssetId": "659efb33acf9fe34333b6e20",
	"totpCode": ""
}
```
