# Public Rest API for AnkerSwap

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
* HTTP `5XX` return codes are used for internal errors; 
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
###  Current Base Coins List. 
```
GET /api/v1/base
```
This endpoint allows anyone to get a list of all base digital assets

**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": {
    "BTC": {
        "minimum": "0.002", 
        "symbol": "BTC", 
        "limit": "2.0", 
        "image": "https://ankerpay.com/images/coins/bitcoin.png", 
        "minerFee": "0.0005", 
        "name": "Bitcoin", 
        "status": "available", 
        "maxLimit": "2.0", 
        "imageSmall": "https://ankerpay.com/images/coins-sm/bitcoin.png"
        }
  ...
}
```
    `minimum` is the lowest amount of the input coin you can swap
    `symbol` is the 3-5 digit symbol for the coin listed.
    `limit` is the highest amount of this coin you can send
    `name` is the full name of coin listed.
    `image` is a URL to an image of the icon for the coin listed.
    `imageSmall` is a URL to an image of the smaller icon for the coin listed
    `status` will read available or unavailable depending on if the coin is currentlt online.
    `minerFee` is an estimation of the proper miner fee to send this con successfully.
    `maxLimit` is the highest amount of the input coin you can


###  Current Target Coins List. 
```
GET /api/v1/target
```
This endpoint allows anyone to get a list of all base digital assets

**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": {
    "BTC": {
        "minimum": "0.002", 
        "symbol": "BTC", 
        "limit": "2.0", 
        "image": "https://ankerpay.com/images/coins/bitcoin.png", 
        "minerFee": "0.0005", 
        "name": "Bitcoin", 
        "status": "available", 
        "maxLimit": "2.0", 
        "imageSmall": "https://ankerpay.com/images/coins-sm/bitcoin.png"
        }
  ...
}
```
`minimum` is the lowest amount of the input coin you can swap
`symbol` is the 3-5 digit symbol for the coin listed.
`limit` is the highest amount of this coin you can send
`name` is the full name of coin listed.
`image` is a URL to an image of the icon for the coin listed.
`imageSmall` is a URL to an image of the smaller icon for the coin listed
`status` will read available or unavailable depending on if the coin is currentlt online.
`minerFee` is an estimation of the proper miner fee to send this con successfully.
`maxLimit` is the highest amount of the input coin you can
    
### Details on cryptoassets traded on an exchange.
```
GET /api/v1/pairs
```
The /pairs endpoint provides a summary on cryptoasset trading pairs available on the exchange.

**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [
    {
      "ticker_id": "BTC_ETH",
      "base": "BTC",
      "target": "ETH"
    }
  ]
}
```

`ticker_id` Identifier of a ticker with delimiter to separate base/target, eg. BTC_ETH
`base` Symbol/currency code of a the base cryptoasset, eg. BTC
`target` Symbol/currency code of the target cryptoasset, eg. ETH

### Market Info.
```
GET /api/v1/tickers
```
The /tickers endpoint provides 24-hour pricing and volume information on each market pair available on an exchange.

**Parameters:**
NONE

**Response:**
```javascript
{
  "time": 1596477571, 
  "data": [
    {
      "ticker_id": "DASH_BTC",
      "base_currency": "DASH", 
      "target_currency": "BTC", 
      "ask": "0.007597"
    },
    ...
  ]
}
```

`ticker_id` Identifier of a ticker with delimiter to separate base/target, eg. BTC_ETH
`base_currency` Symbol/currency code of a the base cryptoasset, eg. BTC
`target_currency` Symbol/currency code of the target cryptoasset, eg. ETH
`ask` Current lowest ask price


### Order book depth details.
```
GET /api/v1/orderbook/ticker_id
```
The /orderbook/ticker_id endpoint is to provide order book information with at least depth = 100 (50 each side) returned for a given market pair/ticker. 

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ticker_id | STRING | YES | A ticker such as "BTC_ETH", with delimiter between different cryptoassets
depth | INT | NO | Orders depth quantity: [0, 100, 200, 500...]. 0 returns full depth. Depth = 100 means 50 for each bid/ask side.

**Example query:**
`.../api/v1/orderbook/BTC_ETH/?depth=200`


**Response:**
```javascript
{
  "ticker_id":	"BTC_ETH"
  "timestamp":	1596475472
  "asks": [
            	"2.0"
            	"28.433819"
          ]
}
```

`ticker_id` Identifier of a ticker with delimiter to separate base/target, eg. BTC_ETH
`asks` Current lowest ask prices. An array containing 2 elements. The offer price and quantity for each bid order


### Historical Data.
```
GET /api/v1/historical_trades/ticker_id
```
The /historical_trades/ticker_id is used to return data on historical completed trades for a given market pair.

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ticker_id | STRING | YES | A ticker such as "BTC_ETH", with delimiter between different cryptoassets
type | STRING | NO | To indicate nature of trade - "buy" or "sell"

**Example query:**
`.../api/v1/historical_trades/BTC_ETH


**Response:**
```javascript
{
  "ticker_id":	"BTC_ETH"
  "timestamp":	1596475472
  "buy": [    {
            	"trade_id":1234567,
              "price":"50.1",
              "base_volume":"0.1",
              "target_volume":"1",
              "trade_timestamp":"1700050000",
              "type":"buy"
              },
              ...
         ],
  "sell": [    {
            	"trade_id":1234567,
              "price":"52.1",
              "base_volume":"0.1",
              "target_volume":"1",
              "trade_timestamp":"1700050000",
              "type":"sell"
              },
              ...
         ]
}
```

`ticker_id` Identifier of a ticker with delimiter to separate base/target, eg. BTC_ETH
`trade_id` A unique ID associated with the trade for the currency pair transaction
`price` Transaction price in base pair volume.
`base_volume` Transaction amount in base pair volume.
`target_volume` Transaction amount in target pair volume.
`trade_timestamp` Unix timestamp in milliseconds for when the transaction occurred.
`type` Used to determine the type of the transaction that was completed. "buy" – Identifies an ask that was removed from the order book. "sell" – Identifies a bid that was removed from the order book.



## Trading endpoints
### Fixed Amount Transaction.
```
POST /api/v1/sendamount
```
This call allows you to request a fixed amount to be sent to the withdrawal address. You provide a withdrawal address and the amount you want sent to it. We return the amount to deposit and the address to deposit to. 

**Parameters:**
Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
withdrawal | STRING | YES | the address for coin to be sent to
pair | STRING | YES | what coins are being exchanged in the form [input coin]_[output coin] ie LTC_BTC
returnAddress | STRING | YES | address to return deposit to if anything goes wrong with exchange


**Response:**
```javascript
{
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


###  Status of deposit to address.
```
GET /api/v1/txstat/[address]
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
  "address": "0xb80469d01912bd3c58673faeee4310c07550828e",
  "withdraw": "19Eek1mnffFF7ecmWHxp2pDcrALmR4Nkon",
  "incomingCoin": 0.08937546,
  "incomingType": "ETH",
  "outgoingCoin": "0.00251506",
  "outgoingType": "BTC",
  "transaction": "e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a",
  "transactionURL": "https://blockchain.info/tx/e760822a528a181dc78bae3fca47a37a4f098d2397d68c4a6279799520fbb99a"
}
```








