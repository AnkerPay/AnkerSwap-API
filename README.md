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


