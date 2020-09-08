# Public Rest API for AnkerSwap POS

## General API Information
* The base endpoint is: **https://api.ankerswap.com**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in **seconds**.

## HTTP Return Codes

* HTTP `4XX` return codes are used for malformed requests;
  the issue is on the sender's side.
* HTTP `403` return code is used when the WAF Limit (Web Application Firewall) has been violated.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  Binance's side.
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.


## Error Codes
* Any endpoint can return an ERROR

Sample Payload below:
```javascript
{
  "code": -1121,
  "msg": "Invalid symbol."
}
```

## General Information on Endpoints
* For `GET` endpoints, parameters must be sent as a `query string`.


# Public API Endpoints

## General endpoints
### Test connectivity
```
GET /api/v1/ping
```
Test connectivity to the Rest API.

**Parameters:**
NONE

**Response:**
```javascript
{"time": 1596473946}
```

## Market Data endpoints
###  Current BTC/ZAR rate. 
```
GET /api/pos/v1/rate
```
This endpoint allows anyone to get a rate of BTC/ZAR pair

**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1596477571, 
  "rate": "181000.00", 
}
```


## POS endpoints
### Fixed Amount Invoice.
```
POST /api/v1/pos/invoice/new
```
This call allows you to create a new fixed amount invoice. You provide a StoreId and TerminalId keys and the amount in ZAR you want get to it. We return the amount in BTC to deposit and the address to deposit to. 

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
StoreId | STRING | YES | the StoreId
TerminalId | STRING | YES | the TerminalId
amount | STRING | YES | amount in ZAR


**Response:**
```javascript
{
  "time": 1596477571, 
  "success": {
    "orderId": "11111b11-000a-111a-a11a-11a1a11a1a11",
    "pair": "BTC_ETH",
    "withdrawal": "0x133459264443e3c56ef7d4e227c26822880b2744",
    "withdrawalAmount": "0.01",
    "deposit": "34qvKdkfSh85Kkct6tmCxLfLjJff73Sim4",
    "depositAmount": "0.00007641",
    "expiration": 1544463034153,
    "quotedRate": "140.69217998",
    "maxLimit": 2.83270872,
    "returnAddress": "18N3RDhjBMf8q13clR4yen3XKaMC8G7no3",
    "minerFee": "0.00075",
    "userId": "111111a-b1e1-11af-11111-1111111d1b11"
  }
}
```

###  Invoice Status.
```
POST /api/v1/pos/invoice/status
```
This endpoint returns the status of the invoice.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
invoiceId | STRING | YES |  invoice Id to look up.

**Response:**
```javascript
{
  "status": "complete",
  "address": "19Eek1mnffFF7ecmWHxp2pDcrALmR4Nkon",
  "incomingCoin": 1.8936,
  "incomingType": "BTC",
  "transaction": "e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a",
  "transactionURL": "https://blockchain.info/tx/e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a"
}
```



###  Status of deposit to address.
```
GET /api/v1/pos/status/[address]
```
This endpoint returns the status of the most recent deposit transaction to the address.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
address | STRING | YES |  is the deposit address to look up.

**Response:**
```javascript
{
  "status": "complete",
  "address": "19Eek1mnffFF7ecmWHxp2pDcrALmR4Nkon",
  "incomingCoin": 1.8936,
  "incomingType": "BTC",
  "transaction": "e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a",
  "transactionURL": "https://blockchain.info/tx/e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a"
}
```

###  Add New terminal.
```
POST /api/v1/pos/terminal/new
```
This endpoint add new terminal to the Store.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.

**Response:**
```javascript
{
  "time": 1596477571, 
  "terminalId": "Eek1mnffFF7ecmWHxp2pDc",
}
```

###  Terminal status.
```
POST /api/v1/pos/terminal/status
```
This endpoint returns the status of terminal.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
terminalId | STRING | YES | Terminal Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "terminalId": "Eek1mnffFF7ecmWHxp2pDc",
}
```

###  Terminal balance.
```
POST /api/v1/pos/terminal/balance
```
This endpoint returns the balance of terminal in ZAR.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
terminalId | STRING | YES | Terminal Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "balance": "500.00",
}
```

###  Terminal's invoice list.
```
POST /api/v1/pos/terminal/invoices
```
This endpoint returns the list of invoices.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
terminalId | STRING | YES | Terminal Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [
  {
  "status": "complete",
  "address": "19Eek1mnffFF7ecmWHxp2pDcrALmR4Nkon",
  "incomingCoin": 1.8936,
  "incomingType": "BTC",
  "transaction": "e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a",
  "transactionURL": "https://blockchain.info/tx/e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a"
  },
  {
  "status": "no_deposit",
  "address": "19Eek1mnffFF7ecmWHxp2pDcrALmR4Nkon",
  "incomingCoin": 1.001,
  "incomingType": "BTC",
  "transaction": "e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a",
  "transactionURL": "https://blockchain.info/tx/e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a"
  }
  ]
}
```

###  Store balance.
```
POST /api/v1/pos/store/balance
```
This endpoint returns the balance of the store in ZAR.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
storeId | STRING | YES | Store Id.
key | STRING | YES | key.

**Response:**
```javascript
{
  "time": 1596477571, 
  "balance": "5000.00",
}
```

###  QR code image generation.
```
GET /qr?img=[address]
```
This endpoint returns PNG image with the address QR code.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
address | STRING | YES |  is the deposit address.

**Response:**
```javascript
PNG image
```







