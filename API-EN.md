# Public Rest API

## General Information
- The base endpoint is: https://api.btcwinex.com/v1 
- The Content-Type in the request header is usually set to: application/x-www-form-urlencoded.
- Currently implemented functions: market data query (Kline, depth, real-time trades, 24h market, etc.).
- Request description: 

    Parameters: encapsulate the parameters according to the details below. 

    Submit request: POST or GET the encapsulated request parameters to the server. 

    Response: server first verifies the user request, then returns the response data to the user in JSON format. 
    
    Data processing: process the server response data.
    
- For endpoints by GET,  parameters must be sent in query string, while order is not required. 
 

## Limits
-   Each endpoint is limited to its own frequency. More than a fixed number of times in a specified period of time access will be denied and corresponding error code will be returned.
-   When received error code, caller should reduce access frequency or stop access.
-   See endpoint details for specific limits.

## Endpoint Details

### Catalogue
    
|No | Type | Path |Request|Description|
|---|---|---|---|---|
|1.1| General endpoints |/common/ping	| get |Test connectivity	
|1.2| General endpoints |/common/time	| get |Check server time	
|2.1| Market Data |/market/kline | get |Get Kline data 	
|2.2| Market Data |/market/orderBook| get |Get market depth	
|2.3| Market Data |/market/trade| get |Recent trades list	
|2.4| Market Data |/market/trade/history| get | Old trade lookup	
|2.5| Market Data |/market/details| get |Get trading data (24h High, 24h Low, volume, etc.) 
|2.6| Market Data |/market/ticker| get |Get last price	
|2.7| Market Data |/market/bookTicker| get |Symbol order book ticker	

### Details


#### 1、General endpoints
1.1 Test connectivity /common/ping  

 Test whether it can be connected 

 Request frequency: 10 times/1s

 Parameter: none

 Response:     

```
 pong
```
 
1.2 Check time /common/time

 Check server time 

 Request frequency: 10 times/1s

 Parameter: none

 Response:     

```
{
  "code": 0000,
  "message": "success",
  "ts": 1499223904680
}

```

#### 2、Market data 

2.1Get Kline data /market/kline

 Request frequency: 10 times/1s

 Parameter:

Name |Mandatory | Type | Description | Default | Range
---|---|---|---|---|---
symbol | Y | string |market||btc_usdt,ltc_usdt,ltc_btc...
period | Y | enum |interval	||1m, 1h, 1d...
limit | N | int |Number of results to fetch	|150|[1,1000 ]
start | N | long |startTime||
end | N | long |endTime||

Response：

```
{
  "code": 0000,
  "message": "success",
  "ts": 1499223904680,
  "data": [
   {
            "open":"3607.60",//Open
            "close":"3607.89",//Close, the latest Kline represents last price
            "low":"3606.51",//Low
            "high":"3607.99",//High
            "vol":"58022.75326268",//Quote asset volume, as sum（price * amount）
            "ts":1549933080000//Time
  },
// more data here
]
}

```

2.2Get market depth /market/orderBook

 Request frequency: 10 times/1s

 Parameter:
 
Name |Mandatory | Type | Description | Default | Range
---|---|---|---|---|---
symbol | Y | string |market||btc_usdt,ltc_usdt,ltc_btc...
limit | N | int |depth|60|valid limits:[5, 20, 60, 100, 200]

Response：

```
{
    "msg":"success",
    "code":"0000",
    "ts":1550820832317,
    "data":{
        "asks":[
            [
                "42.6548", //price
                "11.647" // quantity
            ],
            [
                "42.6748",
                "71.516"
            ],
            [
                "42.6798",
                "19.507"
            ],
            // more data here
        ],
        "bids":[
            [
                "42.6398",
                "9.9992"
            ],
            [
                "42.1696",
                "18.1384"
            ],
            [
                "42.1275",
                "47.0342"
            ],
            // more data here
        ]
    }
}


```
2.3 Recent trades list (60 pieces) /market/trade

Get recent trades

 Request frequency: 10 times/1s

 Parameter:
 
Name |Mandatory | Type | Description | Default | Range
---|---|---|---|---|---
symbol | Y | string |market||btc_usdt,ltc_usdt,ltc_btc...

Response：     

```
{
  "code": 0000,
  "message": "success",
  "ts": 1551183045998,
   "data": [{
        "price":"3813.766",//Price
        "amount":"3.812422",//Amount
        "side":1,//Side 0-Sell 1-Buy
        "ts":1551183045998 //  Time
    },
 ...
]
}

```
2.4 Old trade lookup  /market/trade/history

Request frequency: 5 times/1s

 Parameter:
 
Name |Mandatory | Type | Description | Default | Range
---|---|---|---|---|---
symbol | Y | string |market||btc_usdt,ltc_usdt,ltc_btc...
limit | N | int |Number of results to fetch	|100|[1,1000 ]
start | N | long |startTime||13digit
end | N | long |endTime||13digit

Response：
```
{
  "code": 0000,
  "message": "success",
  "ts": 1551183045998,
   data": [{
        "id":1552179166435,//Trade id
        "price":"3813.766",//Price
        "amount":"3.812422",//Amount
        "side":1,//Side 0-Sell 1-Buy
        "ts":1551183045998 //  Time
  },
 ...
]
}
```

2.5Get trading data (24h High, 24h Low, volume, etc.) /market/details

 Request frequency: 10 times/1s

 Parameter:
 
Name |Mandatory | Type | Description | Default | Range
---|---|---|---|---|---
symbol | Y | string |market| |btc_usdt,ltc_usdt,ltc_btc...

Response：

```
{
  "code": 0000,
  "message": "success",
  "ts": 1551183966480,
    "data":
        { 
            "low":"3786.9709",       //  24h Low
            "high":"3867.4835",      // 24h High
            "vol":"130112128.79718",  // 24h Volume
            "change":"0.06",           // 24h Change
            "price":"3808.3646",        // Last Price
            "symbol":"btc_usdt"     // Market
        }
     
}

```
2.6 Get last price /market/ticker

 Request frequency: 10 times/1s

 Parameter:
 
Name |Mandatory | Type | Description | Default | Range
---|---|---|---|---|---
symbol | Y | string |market||btc_usdt,ltc_usdt,ltc_btc...

Response：

```
{
  "code": 0000,
  "message": "success",
  "ts": 1551184125335,
   "data":
   {
    "symbol":"btc_usdt",//Market
    "price": "3807.6718" // Price
  }
}
```

2.7 Symbol order book ticker /market/bookTicker

 Request frequency: 10 times/1s

 Parameter:
 
Name |Mandatory | Type | Description | Default | Range
---|---|---|---|---|---
symbol | Y | string |market||btc_usdt,ltc_usdt,ltc_btc...

Response：

```
{
  "code": 0000,
  "message": "success",
  "ts": 1499223904680,
   "data": {
    "symbol": "btc_usdt",
    "bidPrice": 3807.7108,//bidPrice
    "bidQty": 0.004447,//bidQty
    "askPrice": 3809.5829,//askPrice
    "askQty": 0.602619//askQty
  }
}
```

#### 3、Parameters list
  
 symbol |
---|
btc_usdt|
eth_usdt|
eos_usdt|
ltc_usdt|
etc_usdt|
qtum_usdt|
snt_usdt|
elf_usdt|
knc_usdt|
eth_btc|
eos_btc|
ltc_btc|
etc_btc|
dash_btc|
omg_btc|
link_btc|
zrx_btc|

DepthLimit |
---|
5|
20|
60|
100|
200|

KLinePeriod
---|
1m|
5m|
15m|
30m|
1h|
4h|
1d|
5d|
1w|
1M|

SideType |Name
---|---
0|sell
1|buy


#### 4、Error code
If an error occurs, the uniform error format is:
```
{
 "code":"9999",//Error code
 "msg":"...",//Error message
 "ts":1550632606428 //System time on return
}
```
##### Error code list

Code |Categories |Description |
---|---|---
1000|SYSTEM_ERROR|System error|
1001|SYSTEM_ERROR|Endpoint unavailable|
1002|SYSTEM_ERROR|Access frequency exceeds the limit|
2000|PARAMS_ERROR|Parameter error|





    
