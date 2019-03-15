# REST行情接口

## 一、简介
- 本篇所有接口的根路径为:https://api.btcwinex.com/v1 
- 请求头信息中Content-Type统一设置为:application/x-www-form-urlencoded。
- 目前已经实现的功能:市场行情信息查询（K线、深度、实时成交、24小时行情等）
- 请求说明:

    参数：请根据下述接口详情进行参数封装。

    提交请求：将封装好的请求参数通过POST 或GET 方式提交至服务器。
    
    响应：服务器首先对用户请求进行校验，通过校验后根据将响应数据以JSON格式返回给用户。
    
    数据处理：对服务器响应数据进行处理。
    
- GET方法的接口, 参数必须在query string中发送。对参数的顺序不做要求。
 

## 二、访问限制
-   每一个接口按照各自的频率进行访问限制，在规定时间内超过固定次数后访问会被拒绝，并返回相应的错误码。
-   收到错误码后，调用者应适当降低访问频率或停止访问。
-   具体限制频率见接口详情。

## 三、接口详情

### 接口目录
    
|序号 | 接口类型 | 请求路径 |请求类型|描述|
|---|---|---|---|---|
|1.1| 通用接口 |/common/ping	| get |测试服务器连通性	
|1.2| 通用接口 |/common/time	| get |获取服务器时间	
|2.1| 市场行情 |/market/kline | get |获取K线数据	
|2.2| 市场行情 |/market/orderBook| get |获取实时行情深度	
|2.3| 市场行情 |/market/trade| get |获取最近成交记录	
|2.4| 市场行情 |/market/trade/history| get | 获取历史成交记录	
|2.5| 市场行情 |/market/details| get |获取交易行情（24小时最高、最低、交易量等）	
|2.6| 市场行情 |/market/ticker| get |获取最新成交价格	
|2.7| 市场行情 |/market/bookTicker| get |获取最优挂单	

### 详情


#### 1、通用接口
1.1 测试服务器连通性  /common/ping  

 测试能否联通

 访问频率: 10次/1秒

 请求参数：无

 响应数据：     

```
 pong
```
 
1.2 获取时间 /common/time

 获取服务器时间

 访问频率: 10次/1秒

 请求参数:无

 响应数据:     

```
{
  "code": 0000,
  "message": "success",
  "ts": 1499223904680
}

```

#### 2、市场行情 

2.1获取K线数据 /market/kline

 访问频率: 10次/1秒

 请求参数:

参数名称 |是否必须 | 类型 | 描述 | 默认值 | 取值范围
---|---|---|---|---|---
symbol | Y | string |市场||btc_usdt,ltc_usdt,ltc_btc...
period | Y | enum |K线周期	||1m, 1h, 1d...
limit | N | int |查询数量	|150|[1,1000 ]
start | N | long |开始时间||13位
end | N | long |结束时间||13位

响应数据:    

```
{
  "code": 0000,
  "message": "success",
  "ts": 1499223904680,
   "data": [
   {
            "open":"3607.60",//开盘价
            "close":"3607.89",//收盘价,当K线为最晚的一根时，是最新成交价
            "low":"3606.51",//最低价
            "high":"3607.99",//最高价
            "vol":"58022.75326268",//成交额, 即 sum(每一笔成交价 * 该笔的成交量)
            "ts":1549933080000//时间
  },
// more data here
]
}

```

2.2获取实时行情深度 /market/orderBook

 访问频率: 10次/1秒

 请求参数:

参数名称 |是否必须 | 类型 | 描述 | 默认值 | 取值范围
---|---|---|---|---|---
symbol | Y | string |市场||btc_usdt,ltc_usdt,ltc_btc...
limit | N | int |深度|60|可选值:[5, 20, 60, 100, 200]

响应数据:  

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
2.3 获取近期成交记录(60条) /market/trade

获取最新的成交记录

访问频率: 10次/1秒

请求参数：

参数名称 |是否必须 | 类型 | 描述 | 默认值 | 取值范围
---|---|---|---|---|---
symbol | Y | string |市场||btc_usdt,ltc_usdt,ltc_btc...

响应数据：     

```
{
  "code": 0000,
  "message": "success",
  "ts": 1551183045998,
   "data": [{
        "price":"3813.766",//成交价格
        "amount":"3.812422",//成交数量
        "side":1,//主动成交方向 0-卖 1-买
        "ts":1551183045998 //  成交时间
    },
 ...
]
}

```
2.4 获取历史成交记录 /market/trade/history

访问频率: 5次/1秒

请求参数:

参数名称 |是否必须 | 类型 | 描述 | 默认值 | 取值范围
---|---|---|---|---|---
symbol | Y | string |市场||btc_usdt,ltc_usdt,ltc_btc...
limit | N | int |查询数量	|100|[1,1000 ]
start | N | long |开始时间||13位
end | N | long |结束时间||13位

响应数据:
```
{
  "code": 0000,
  "message": "success",
  "ts": 1551183045998,
   data": [{
        "id":1552179166435,//成交id
        "price":"3813.766",//成交价格
        "amount":"3.812422",//成交数量
        "side":1,//主动成交方向 0-卖 1-买
        "ts":1551183045998 //  成交时间
  },
 ...
]
}
```

2.5获取交易行情（24小时最高、最低、交易量等）/market/details

 访问频率: 10次/1秒

 请求参数:
 
参数名称 |是否必须 | 类型 | 描述 | 默认值 | 取值范围
---|---|---|---|---|---
symbol | Y | string |市场| |btc_usdt,ltc_usdt,ltc_btc...

响应数据：     

```
{
  "code": 0000,
  "message": "success",
  "ts": 1551183966480,
    "data":
        { 
            "low":"3786.9709",       //  24小时最低价
            "high":"3867.4835",      // 24小时最高价
            "vol":"130112128.79718",  // 24小时成交金额
            "change":"0.06",           // 24小时涨跌幅
            "price":"3808.3646",        // 最新价
            "symbol":"btc_usdt"     // 市场
        }
     
}

```
2.6 获取最新成交价格 /market/ticker

访问频率: 10次/1秒

请求参数:

参数名称 |是否必须 | 类型 | 描述 | 默认值 | 取值范围
---|---|---|---|---|---
symbol | Y | string |市场||btc_usdt,ltc_usdt,ltc_btc...

响应数据:     

```
{
  "code": 0000,
  "message": "success",
  "ts": 1551184125335,
   "data":
   {
    "symbol":"btc_usdt",//市场
    "price": "3807.6718" // 价格
  }
}
```

2.7 获取最优挂单 /market/bookTicker

访问频率: 10次/1秒

请求参数:

参数名称 |是否必须 | 类型 | 描述 | 默认值 | 取值范围
---|---|---|---|---|---
symbol | Y | string |市场||btc_usdt,ltc_usdt,ltc_btc...

响应数据:     

```
{
  "code": 0000,
  "message": "success",
  "ts": 1499223904680,
   "data": {
    "symbol": "btc_usdt",
    "bidPrice": 3807.7108,//最优买单价
    "bidQty": 0.004447,//挂单量
    "askPrice": 3809.5829,//最优卖单价
    "askQty": 0.602619//挂单量
  }
}
```

#### 3、参数列表
  
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
0|卖
1|买


#### 4、错误代码
如果发生错误，统一的错误的格式是：
```
{
 "code":"9999",//错误编码
 "msg":"...",//错误信息
 "ts":1550632606428 //返回数据的系统时间
}
```
##### 错误代码列表

错误代码 |错误类别 |描述 |
---|---|---
1000|SYSTEM_ERROR|系统错误|
1001|SYSTEM_ERROR|接口不可用|
1002|SYSTEM_ERROR|访问频率超过限制|
2000|PARAMS_ERROR|参数错误|





    
