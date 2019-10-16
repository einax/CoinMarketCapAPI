The service was developed according to the Coin Market Cap specifications https://docs.google.com/document/d/1S4urpzUnO2t7DmS_1dc4EL4tgnnbTObPYXvDeBnukCg. 

# Assets

Details on crypto currencies available on the exchange.

Available at url: **https://einax.com/api/coinmarketcap/assets/**

Response codes:

| Code | Description |
| --- | --- |
| 200 | OK. |
| 500 | Internal server error. |

Example valid request:

```
GET https://einax.com/api/coinmarketcap/assets/
```

Example response:
```
{
  "BTC": {
    "name": "Bitcoin",
    "unified_cryptoasset_id": 1,
    "can_withdraw": true,
    "can_deposit": true
  },
  "DCA": {
    "name": "Decentralized Currency Asset",
    "can_withdraw": true,
    "can_deposit": true
  },
  ...
}
```
Json object fields description

| Name | Json type | Real type | Market Cap status | Einax status | Description |
| ---- | ---- | --------- | ----------------- | ------------ | ----------- |
| name | String | String | Recommended | Returned | Name of cryptocurrency |
| unified_cryptoasset_id | Integer | Integer | Recommended | Returned | Available for currency present on [Unified Cryptoasset ID](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active) only |
| can_withdraw | Boolean | Boolean | Recommended | Returned |Indicates whether withdrawals are enabled|
| can_deposit | Boolean | Boolean | Recommended | Returned |Identifies whether deposits are enabled or disabled.|
| min_withdraw | String  | Decimal | Recommended | Returned | Minimum withdraw amount for a cryptocurrency.| 
| max_withdraw | String  | Decimal | Recommended |Not applicable|We don't have withdraw limits|
| maker_fee | String  | Decimal | Recommended |Not applicable|Fees are set on per-market basis|
| taker_fee | String  | Decimal | Recommended |Not applicable|Fees are set on per-market basis|

# Ticker

24-hour rolling window price change statistics for all markets.

Available at url: **https://einax.com/api/coinmarketcap/ticker/**

Response codes:

| Code | Description |
| --- | --- |
| 200 | OK |
| 500 | Internal server error |

Example valid request:
```
GET https://einax.com/api/coinmarketcap/ticker/
```
Example response:
```
{
  "AFTA_BTC": {
    "quote_id": 1,
    "last_price": "0.00000001000000000000",
    "base_volume": "0.00000000000000000000",
    "quote_volume": "0.00000000000000000000",
    "isFrozen": "0"
  },
  "ETH_BTC": {
    "base_id": 1027,
    "quote_id": 1,
    "last_price": "0.02138808000000000000",
    "base_volume": "31.10585428893101204035",
    "quote_volume": "0.66529449999999960000",
    "isFrozen": "0"
  },
  ...
}
```
Json object fields description

| Name | Json type | Real type | Market Cap status | Einax status | Description |
| ---- | ---- | --------- | ----------------- | ------------ | ----------- |
| base_id | Integer | Integer | Recommended | Returned | Return for currency present on [Unified Cryptoasset ID](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active) only |
| quote_id | Integer | Integer | Recommended | Returned | Return for currency present on [Unified Cryptoasset ID](https://pro-api.coinmarketcap.com/v1/cryptocurrency/map?CMC_PRO_API_KEY=UNIFIED-CRYPTOASSET-INDEX&listing_status=active) only |
| last_price | String  | Decimal | Mandatory | Returned |The price of the last executed order| 
| base_volume | String  | Decimal | Mandatory | Returned |24 hour trading volume in base pair volume| 
| quote_volume | String  | Decimal | Mandatory | Returned |24 hour trading volume in quote pair volume| 
| isFrozen | String  | Integer | Mandatory | Returned |Indicates if the market is currently enabled (1) or disabled (0).|

# Orderbook

The order book endpoint provides a complete level 2 order book (arranged by best asks/bids) with full depth returned for a given market pair.

Available at url: **https://einax.com/api/coinmarketcap/orderbook/**

| Parameter | Status | Description |
| --- | --- | --- |
| Market pair | Mandatory | A pair such as “ETH_BTC”. |
| depth | Recommended | Orders depth quantity:[0 .. 500].<br>Not defined or 0 = full order book. |
| level | Recommended | Level 1 – Only the best bid and ask.<br>Level 2 – Arranged by best bids and asks.<br>Level 3 – not support. |

Response codes:

| Code | Description |
| --- | --- |
| 200 | OK. |
| 204 | Market is inactive. |
| 400 | Bad request if inputed bad recommended parameters. |
| 404 | Inputed market name does not exist, also if an empty market name is entered. |
| 500 | Internal server error. |

Example requests: 
```
GET https://einax.com/api/coinmarketcap/orderbook/ETH_BTC
```
Example response:
```
{
  "timestamp": 1570002253,
  "bids": [
    [
      "0.02138058000000000000",
      "0.03052420000000000000"
    ],
    [
      "0.02133926000000000000",
      "0.02337300000000000000"
    ],
    ...
  ],
  "asks": [
    [
      "0.02142696000000000000",
      "0.15389510000000000000"
    ],
    [
      "0.02142701000000000000",
      "0.01562720000000000000"
    ],
    ...
  ]
}
```
Example request:
```
GET https://einax.com/api/coinmarketcap/orderbook/ETH_BTC?level=1
```
Example response:by
```
{
  "timestamp": 1570024074,
  "bids": [
    "0.02143916000000000000",
    "0.03108660000000000000"
  ],
  "asks": [
    "0.02147655000000000000",
    "0.11828060000000000000"
  ]
}
```
Json object fields description

| Name | Type | Real type | Market Cap status | Einax status | Description |
| ---- | ---- | --------- | ----------------- | ------------ | ----------- |
| timestamp | Integer  | Integer | Mandatory | Returned |Unix timestamp of the last transaction (seconds)|
| bids | Array  | Array | Mandatory | Returned | Return Array of Decimal or Array of Array of Decimal by url parameter 'level' |
| asks | Array  | Array | Mandatory | Returned | Return Array of Decimal or Array of Array of Decimal by url parameter 'level' | 

# Trades

Recently completed trades for a given market (up to 500).

Available at url: **https://einax.com/api/coinmarketcap/trades/**

| Parameter | Status | Description |
| --- | --- | --- |
| Market pair | Mandatory | A pair such as “ETH_BTC” |
| type | Recommended |  Query 'buy' side or 'sell' side only. |

Response codes:

| Code | Description |
| --- | --- |
| 200 | OK |
| 204 | Market is inactive |
| 400 | Bad request |
| 404 | Market name does not exists |
| 500 | Internal server error |

Example valid requests:
```
GET https://einax.com/api/coinmarketcap/trades/ETH_BTC
```
Example response:
```
[
  {
    "tradeID": 1426001,
    "price": "0.02136701000000000000",
    "base_volume": "0.00016920000000000000",
    "quote_volume": "0.00000361529809200000",
    "trade_timestamp": 1570007474,
    "type": "buy"
  },
  {
    "tradeID": 1426000,
    "price": "0.02136704000000000000",
    "base_volume": "0.00010610000000000000",
    "quote_volume": "0.00000226704294400000",
    "trade_timestamp": 1570002121,
    "type": "sell"
  },
  
  ...
]
```
Json object fields description

| Name | Type | Real type | Market Cap status | Einax status | Description |
| ---- | ---- | --------- | ----------------- | ------------ | ----------- |
| tradeID | Integer  | Integer | Mandatory | Returned |A unique ID associated with the trade for the currency pair transaction.|
| price | String  | Decimal | Mandatory | Returned |Transaction price in base pair volume.| 
| base_volume | String  | Decimal | Mandatory | Returned |Transaction amount in base pair volume.| 
| quote_volume | String  | Decimal | Mandatory | Returned |Transaction amount in quote pair volume.|
| trade_timestamp | Integer  | Integer | Mandatory | Returned |Unix timestamp of the last transaction in seconds.|
| type | String  | String | Mandatory | Returned | Used to determine whether the transaction originated as a buy or sell.<br>buy – An ask was removed from the order book.<br>sell – A bid was removed from the order book.|

# Summary

Overview of market data for all tickers.

Available at url: **https://einax.com/api/coinmarketcap/summary/**

Response codes:

| Code | Description |
| --- | --- |
| 200 | OK |
| 500 | Internal server error |

Example valid request:
```
GET https://einax.com/api/coinmarketcap/summary/
```
Example response:
```
{
  "AFTA_BTC": {
    "last": "0.00000001000000000000",
    "lowerstAsk": "0.00000000000000000000",
    "highestBid": "0.00000000000000000000",
    "percentChange": "0.00000000000000000000",
    "baseVolume": "0.00000000000000000000",
    "quoteVolume": "0.00000000000000000000",
    "isFrozen": "0",
    "high24hr": "0.00000001000000000000",
    "low24hr": "0.00000001000000000000"
  },
  "ETH_BTC": {
    "id": 1426808,
    "last": "0.02139177000000000000",
    "lowerstAsk": "0.02139752000000000000",
    "highestBid": "0.02139177000000000000",
    "percentChange": "0.03432403856450698008",
    "baseVolume": "31.04248549792750670001",
    "quoteVolume": "0.66405371000000070000",
    "isFrozen": "0",
    "high24hr": "0.02169582000000000000",
    "low24hr": "0.02104299000000000000"
  },
  ...
}
```
Json object fields description

| Name | Type | Real type | Market Cap status | Einax status | Description |
| ---- | ---- | --------- | ----------------- | ------------ | ----------- |
| id | Integer  | Integer | Mandatory | Returned | Last market trade id. This field is absent if market is inactive or no trades was executed on the market|
| last | String  | Decimal | Mandatory | Returned | Last trade price |
| lowerstAsk | String  | Decimal | Mandatory | Returned | The best ask. Returns zero, in case there are no asks present on the market. |
| highestBid | String  | Decimal | Mandatory | Returned | The best bid. Returns zero,in case there are no bids present on the market. |
| percentChange | String  | Decimal | Mandatory | Returned | Calculated by the formula:<br><br>percentChange = (last price - start price) / start price * 100.|
| baseVolume | String  | Decimal | Mandatory | Returned | Currently this value is calculated using the formula:<br><br>baseVolume = quote volume / last price<br><br> In future updates this will be correctly calculated based on actual trades. |
| quoteVolume | String  | Decimal | Mandatory | Returned ||
| isFrozen | String  | Integer | Mandatory | Returned |Indicates if the market is currently enabled (1) or disabled (0).|
| high24hr | String  | Decimal | Mandatory | Returned ||
| low24hr | String  | Decimal | Mandatory | Returned ||









