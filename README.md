CoinMex-official-api-docs
============================================
Official Documentation for the [BIONE]

<!-- TOC -->

- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Encrypted Verification of API](#encrypted-verification-of-api)
    - [Generate an API Key](#generate-an-api-key)
    - [Initiate a Request](#initiate-a-request)
    - [Signature](#signature)
    - [Select timestamp](#select-timestamp)
    - [Request Process](#request-process)
        - [Request](#request)
        - [Pagination](#pagination)
    - [Standards and Specification](#standards-and-specification)
        - [Timestamp](#timestamp)
        - [For example,](#for-example)
        - [Numbers](#numbers)
        - [Rate Limits](#rate-limits)
            - [REST API](#rest-api)
- [Spot API Reference](#spot-api-reference)
    - [Spot Market API](#spot-market-api)
        - [1. Access the list of all trading pairs](#1-access-the-list-of-all-trading-pairs)
        - [2. Access the depth table of trading pairs](#2-access-the-depth-table-of-trading-pairs)
        - [3. Access the ticker of a trading pair](#3-access-the-ticker-of-a-trading-pair)
        - [4. Access the market trading records of a trading pair](#4-access-the-market-trading-records-of-a-trading-pair)
        - [5. Access Candlestick chart](#5-access-candlestick-chart)
        - [6. Access Server Time](#6-access-server-time)
    - [Spot Account API](#spot-account-api)
        - [1. Access account information](#1-access-account-information)
        - [2. Order Placement](#2-order-placement)
        - [3. Cancel all orders](#3-cancel-all-orders)
        - [4. Cancel a specified order](#4-cancel-a-specified-order)
        - [5. Search orders](#5-search-orders)
        - [6. Order inquiry by Order ID](#6-order-inquiry-by-order-id)
        - [7. Access the account statement](#7-access-the-account-statement)
        - [8. Withdrawal](#8-withdrawal)

<!-- /TOC -->
# Introduction
Welcome to [BIONE][] API document for developers.

This document provides instructions on how to use APIs related to account management, market information, trading functions among others in spot trading.

Market API provides market data that are accessible to the public. Account APIs and trading APIs, which provide functions such as order placement, order cancellation, order inquiry and account information, need identity authentication.

# Getting Started

REST, a.k.a Respresntational State Transfer, is an architectural style that defines a set of constraints and properties based on HTTP. REST is known for its clear structure, readability, standardization and scalability. Its advantages are as follows:

+ Each URL represents one web resource in RESTful architecture;
+ Acting as a representation of resources between client and server;
+ Client is enabled to operate server-side resources with 4 HTTP requests - representational state transfer.

Developers are recommended to use REST API to proceed spot trading and withdrawals.

# Encrypted Verification of API
## Generate an API Key

Before signing any request, you must apply an access key via BIONE’s official Email ceo@bione.cc. BIONE officials will send the access key and secret after auditing.  
After gotten the key, there are two things you must bear in mind:
* Access Key

* Secret Key

API access Key and Secret are randomly generated and provided. Please make sure not send secret key to anybody.

## Signature

User submitted parameters must be signed except for sign. First, the string to be signed is ordered according to the parameter name (first compare the first letter of all parameter names, in alphabetic order, if you encounter the same first letter, then you move to the second letter, and so on).

**sign process**

1. sort all parameters with alphabet order； 
2. join all parameters like this，key1value1key2value2key3value3...keyNvalueN； 
3. use app secret as prefix and suffix like appsecret + keyvalue_result + appsecret
4. hmac with sha256, like hash_hmac('sha256', message, appSecret, true);
5. get the sign string with md5(appSecret + hmac_hash_value + appSecret)


## Request Process 
  
The root URL for REST access：`https://www.bione.cc`

### Request
All requests are based on Https protocol.
It is required to append accessKey=xxx&sign=xxx for the api required authentication.

**Request Process Descriptions**

1. Request parameter: parameter encapsulation based on the port request.

2. Submitting request parameter: submit the encapsulated parameter request to the server via POST/GET/ or other methods.

3. Server response: the server will first perform a security validation, then send back the requested data to the client in JSON format.

4. Data processing: processing server response data.

**Success**

HTTP status code 200 indicates a successful response and may contain content. If the response contains content, it will appear in the corresponding returned content.

all the response content will be a json text like below.
```
{
    "code": 1,
    "msg": "success",
    "data": xxx,
}
```

**Common Error Code**

* 1,  success

* 0 or others, failed



if failed, your can get the error message from field "msg"


### Pagination

use pagenumber and pagesize.

**For example:**

`GET /orders?pagenumber=1&pagesize=20`

## Standards and Specification

### Timestamp

Unless otherwise specified, all timestamps in APIs are returned in microseconds.

The ACCESS-TIMESTAMP header must be the number of seconds since UTC's time [Unix Epoch][]. Decimal values are allowed. 
Your timestamp must be within 30 seconds of the API service time, otherwise your request will be considered expired and rejected. If you think there is a large time difference between your server and the API server, then we recommend that you use the time point to check the API server time.

### For example,

1524801032573

### Numbers

In order to maintain the accuracy of cross-platform, decimal numbers are returned as strings. We suggest that you might be better to convert the number to string when issuing the request to avoid truncation and precision errors. Integers (such as transaction number and sequence) do not need quotation marks.

### Rate Limits

When a rate limit is exceeded, a status of 429 Too Many Requests will be returned.

#### REST API

* Public interface: We limit the invocation of public interface via IP: up to 6 requests every 2s.

* Private interface: We limit the invocation of private interface via user ID: up to 6 requests every 2s.

* Special restrictions on specified interfaces are specified.

# Spot API Reference

## Spot Market API

### 1. Access the list of all trading pairs

**HTTP Request**

```http
    # Request
    GET /api/v2/markets
```
```javascript
    # Response
    [
        {
            "market": "eth_usdt",
            "coin_pre": "eth",
            "coin_suf": "usdt",
            "vol": "1417.64830900",
            "change": "4.86000000",
            "quotePrecision": 6,
            "buy_min": "",
            "buy_max": "",
            "sell_min": "",
            "sell_max": ""
        },
        {
            "market": "bcd_usdt",
            "coin_pre": "bcd",
            "coin_suf": "usdt",
            "vol": "388.82555700",
            "change": "1.46000000",
            "quotePrecision": 6,
            "buy_min": "",
            "buy_max": "",
            "sell_min": "",
            "sell_max": ""
        },
        ...
    ]
```

**Response Details**

| Field | Descirption |
| ----------|:-------:|
| code        | Trading pair code |
| baseCurrency   | Base currency |
| quoteCurrency  | Quote currency |
| baseMinSize   | Minimum Transaction Volume |
| baseMaxSize   | Maximum Transaction Volume |
| quoteIncrement | Ticker Size |
| quotePrecision | Quote Precision |
| volumeIncrement | Volume Increase Size |

### 2. Access the depth table of trading pairs

**HTTP Request**

```http
    # Request
    GET /api/v2/depth?market=xxxx
```
```javascript
    # Response
    {
        "asks":[
            [
                "10463.3399",
                "0.0025"
            ],
            ...
        ],
        "bids":[
            [
                "7300.2456",
                "0.0022"
            ],
            ...
        ]
    }
```
**Response Details**  


|Field|Description|  
|---- |------------|
| asks | depth of sellers |
| bids | depth of buyers |

**Request Paramters**

| Name | Type  | Requited | Description |
| ------------- |-----|-----|-----|
| market | String | Y | Trading Pair, e.g. ltc_btc |

### 3. Access the ticker of a trading pair

**HTTP Request**

    The snapshot of the latest price, the highest bid price, the lowest ask price and 24-hour trading volume.

```http
    # Request
    GET /api/v2/ticker/:market
```

```javascript
    # Response
    [
        "symbol": "eth_usdt",
        "buy": "0.00000000",
        "sell": "0.00000000",
        "high": "145.01000000",
        "low": "129.19000000",
        "last": "145.01000000",
        "vol": "1418.15673100",
        "change": "4.87000000"
    ]
```

**Response Details (from the top down)**

|Field|Description|
|--------| :-------: |
| symbol | trade pair |
| buy|buy|
| sell|sell|
| high|24hr Highest|
| low |24hr Lowest|
| last|latest price|
| vol|24h Vol|
| change|24h change|
| 24h Close|7845.2459|

**Request Parameter**

|Name|Type|Required|Description| 
|------|-----|-----|-----|
|market|String|Y|Trading Pair, e.g. btc_usdt|

### 4. Access the market trading records of a trading pair

    The request supports pagination.

**HTTP Request**
```http
    # Request
    GET /api/v2/deal/:market?pagenumber=1&pagesize=10
```
```javascript
    # Response
    [
        [
           "amount": "0.11590600",
           "date": 1550544363,
           "price": "145.00000000",
           "type": 1
        ],
        [
            "amount": "0.11590600",
            "date": 1550544363,
            "price": "145.00000000",
            "type": 2
        ]
    ]
```
**Response Description (In order)**

|Field|Description|
|--------|-----|
|Execution Price |0.00329999|
|Volume |10.99999999|
|Maker Side|Buy|
|Timestamp| 1524801032573|

**Request Paramters**

|Name|Type|Required|Description| 
|-----|-----|-----|-----| 
|code|String|Y|Trading pair, e.g. btc_usdt|

    **Explanation**

    + Side indicates that the direction of the order the maker places. Maker refers to a trader who places orders in the market, a marker is a passive transaction party

    + Buy suggests price fall, because the maker places a buy order and the order is executed, the price falls; in contrary, sell suggests price rise, because the maker places a sell order and the order is executed, the price rises.

### 5. Access Candlestick chart

**HTTP Request**

```http
    # Request
    GET  /api/v2/kline/:market?type=1min&start=start_time&end=end_time
```
```javascript
    # Response
    {
        [ 1415398768, 0.32, 0.42, 0.36, 0.41, 12.3 ]
        ...
    }
```

**Response Details (in order)**
    
|Field|Description|
|-----|-----|
|Start timestamp|1415398768|
|The lowest price|0.32|
|The highest price|0.42|
|Opening price|0.36|
|Closing price|0.41|

**Request parameters**
    
|Name|Type|Required|Description|
|-----|-----|-----|-----|
|code|String|Y|Trading pair, e.g.btc_usdt|
|type|String|Y|Candlestick chart period type, e.g.1min/1hour/day/week/month|
|start|String|Y|Opening time based on ISO 8601|
|end|String|Y|Closing time based on ISO 8601|

### 6. Access Server Time

    Access API server time. This interface does not require ID authentication.

**HTTP Request**
```http
    # Request
    
    GET /api/v2/public/time
```
    
```javascript
    # Reponse

    {
        "timestamp": 1524801032
    }
```
    
**Response Description**
    
|Field|Description|  
|------|-----|  
|epoch|server time expressed in second|
|iso|server time expressed in time string by ISO 8061|
|timestamp|server time expressed in millisecond|

       iso: Response is returned in time string by ISO 8061
       epoch: Response is retured in timestamp

    It is an API for accessing all the available trading pairs and their trading parameters.

## Spot Account API

### 1. Access account information

    Access the list of balance, inquiry of coin balances, freezing status and available fund in spot account.

**HTTP Request**
```
    # Request
    GET /api/v2/account/assets
```
```
    # Response
    [
        {
            "coin": "btc",
            "available": "0.02809200",
            "freeze": "0.00000000",
            "balance": 0.028092
        },
        {
           "coin": "ltc",
           "available": "0.00000000",
           "freeze": "0.00000000",
           "balance": 0
        }
    ]
```

**Response Details**

|Field|Description|
|-----|-----|
|available|Avaliable Fund|
|balance|Number of coins in balance|
|currencyCode|Coin symbol|
|frozen|Frozen fund|
|id|Account ID|

### 2. Order Placement

    There are two categrories of orders that can be placed on CoinMex -- limit order and market order.

**HTTP Request**
```
    # Request

    POST /api/v2/order/create?accessKey=your_access_key&sign=your_sign_str&market=fund_usdt&unitPrice=0.000300&type=1&number=100
```

```javascript
    # Response

    {
        "orderSn": "HB1234567",
        "order_id": 123456
    }
```
    
    **Response Details**

    + orderId: Order ID
    + result: the result of the order placed

**Request Paramters**

|Name| Type | Required | Description |
|----|----|-----|-----|
|code|String|Y|Trading pair, e.g.btc_usdt|
|side|String|N|buy or sell|
|type|String|Y|limit order or market order|
|size|String|N|delivered when a limit order or selling market order if placed,representing the number of coins for trading|
|price|String|N|delivered when a limit order is placed, representing the price of the pair
|funds|String|N|delievered then a market order is placed, representing the number of quote currencies


### 3. Cancel all orders

    Cancel all unfilled orders of the target trading pair.

**HTTP Request**
```
    # Request
    DELETE /api/v2/order/batchcancel?market=eth_usdt&orderids=1,2,3&accessKey=your_access_key&sign=your_sign_str
```
```javascript
    # Response

    { ...}
```

**Request Paramters**

|Name|Paramters|Type|Description|
|----|-----| -----| -----|
|code|String|Y|Trading pairs, e.g. btc_usdt|

### 4. Cancel a specified order

    Cancel a specified order by order ID

**HTTP Request**

```http
    # Request
    GET /api/v2/order/cancel
```
```javascript
    # Response
    {...}
```

**Request Paramters**

|Name|Type|Required|Description|
|-----|-----|-----|-----|
|code|String|Y|Trading Pair, e.g. btc_usdt|
|orderId|String|Y|The ID of an unfilled order specified need to be cancelled|

### 5. Search orders

    Check all the orders by order status.
    
**HTTP Request**

```http   
    # Request
    GET /api/v2/orders?market=fund_usdt&status=xxx&accessKey=your_access_key&sign=your_sign_str
```
```javascript
    # Response
    {
        "id": 6188977,
        "userid": 12,
        "order_number": "I218573604654579",
        "market": "fund_usdt",
        "market_type": 1,
        "type": 1,
        "price": "0.00030000",
        "num": "100.00000000",
        "deal": "0.00000000",
        "mum": "0.03000000",
        "fee": null,
        "nature": "",
        "sort": null,
        "addtime": 1550457360,
        "endtime": null,
        "status": 3
    }
```

**Response Details**

|Field|Description|
|-----|-----|
|averagePrice|average price for the filled orders; 0 for the unfilled orders|
|code|Trading pair, e.g.btc_usdt|
|createDate|Timestamp upon the placement of the order|
|filledVolume|the volume of the filled orders|
|funds|the amount of the filled|
|orderId|Order ID|
|price|Price set for the order|
|side|Order direction|
|status|Order Status|
|volume|Volume of coins in the order placed|

**请求参数**

|Name | Type | Required | Description |
|------|-----|-----|-----|
|code|String|Y|Trading pair, e.g.btc_usdt|
|status|String|Y| Order Status:open,filled,canceled,cancel,partially-filled|

### 6. Order inquiry by Order ID

    Inquiry of a specified order by order ID

**HTTP Request**
```http
    # Request
    GET /api/v2/order/query?orderid=your_order_id&market=your_market&accessKey=your_access_key&sign=your_sign_str
```
```javascript
    # Response 
    {
       "id": 6188675,
       "userid": 12,
       "order_number": "I218564861103484",
       "market": "fund_usdt",
       "market_type": 1,
       "type": 1,
       "price": "0.00030000",
       "num": "100.00000000",
       "deal": "0.00000000",
       "mum": "0.03000000",
       "fee": null,
       "nature": "",
       "sort": null,
       "addtime": 1550456486,
       "endtime": null,
       "status": 3
    }
```

**Response Details**
    
|Field|Description|
|------|-----|
|averagePrice|average price for the filled orders; 0 for the unfilled orders|
|code|Trading pair, e.g.btc_usdt|
|createDate|Timestamp upon the placement of the order|
|filledVolume|the volume of the filled orders|
|funds|the amount of the filled|
|orderId|Order ID|
|price|Price set for the order|
|side|Order direction|
|status|Order Status|
|volume|Volume of coins in the order placed|

**Request Paramters**
    
|Name|Type|Required|Description
|-----|-----|-----|-----
|code|String|Y|Trading pair, e.g.btc_usdt|
|orderId|String|Y|Order Id|

### 7. Access the account statement

    Access the statement of a spot account

**HTTP Request**
```http
    # Request
    GET /api/v2/account/bill/:currencyCode?accessKey=your_access_key&sign=your_sign_str
```
```javascript
    # Response
    {
       "id": 19010805,
       "userid": 12,
       "order_number": "I111863328528718",
       "market": "cjz_eth",
       "operation": 2,
       "type": 2,
       "currency_abbr": "eth",
       "currency_total": "0.01490000",
       "c_before_normal": "2.41457859",
       "c_before_bfreeze": "0.00000000",
       "c_before_total": "2.41457859",
       "c_after_normal": "2.39967859",
       "c_after_bfreeze": "0.01490000",
       "c_after_total": "2.41457859",
       "fee": "0.00000000",
       "log_id": "4996256",
       "move": "",
       "addtime": 1547186332,
       "status": 1
    }
```

**Response Details**

|Field | Description |
|-----|-----|
|amount|Volume of coins traded on the statement|
|balance|Statement balance|
|createdDate|Timestamp on the statement taking place|
|details|Statement Details|
|orderId|Order ID|
|productId|Product ID|
|id|Statement ID|
|type|Transaction Type|

**Request Paramters**

|Name|Type|Required|Description|
|----|-----|-----|-----|
|code|String|Y| Trading pair, e.g.btc_usdt|

### 8. Withdrawal

    Withdraw to your wallet address.

**HTTP Request**

```http
    # Request
    GET /api/v2/account/withdraw?coin=your_coin_name&num=your_withdraw_num&addr=your_coin_address&accessKey=your_access_key&sign=your_sign_str
```
```javascript
    # Response
    { ... }
```

**Request Parameters**

|Name|Type|Required|Description
|-----|-----|-----|-----|
|currencyCode|String|Y|Name of coin to be withdrawn, e.g.BTC|
|amount|String|Y|Withdraw amount|
|address|String|Y| Withdraw address|


[BIONE]: https://www.bione.cc 
