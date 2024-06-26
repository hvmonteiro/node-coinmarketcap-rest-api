# About

*node-coinmarketcap* is a nodeJs module to get market data from CoinMarketCap.com by means of connection to their REST API and retrieve prices and statistics of cryptocurrencies.

It supports events to get alerts on price status at a predefined interval.

Originally based on the simple yet very well written [node-coinmarketcap](https://github.com/Aex12/node-coinmarketcap).

## Installation

```console
$ npm install node-coinmarketcap-rest-api
```

## Quick Usage Example:
```js
var CoinMarketCap = require('node-coinmarketcap-rest-api');
var coinmarketcap = new CoinMarketCap();
// If you want to check a single coin, use get() (You need to supply the coinmarketcap id of the cryptocurrency, not the symbol)
// If you want to use symbols instead of id, use multi.
coinmarketcap.get('bitcoin', coin => {
  console.log(coin.price_usd); // Prints the price in USD of BTC at the moment.
});
```

## More Complete Usage Example:
```js
var CoinMarketCap = require('.');
var coinmarketcap = new CoinMarketCap();
// If you want to check a single coin, use get() (You need to supply the coinmarketcap id of the cryptocurrency, not the symbol)
// If you want to use symbols instead of id, use multi.
coinmarketcap.getTicker('bitcoin', coin => {
  console.log(coin.price_usd); // Prints the price in USD of BTC at the moment.
});
// If you want to check multiple coins, use multi():
coinmarketcap.getAllTickers(coins => {
  console.log(coins.get('BTC').price_usd); // Prints price of BTC in USD
  console.log(coins.get('ETH').price_usd); // Print price of ETH in USD
  console.log(coins.get('LTC').price_btc); // Print price of LTC in BTC
  console.log(coins.getTop(10)); // Prints information about top 10 cryptocurrencies
});

// Get Global Market Data
coinmarketcap.getGlobalData( (globalData) => {
    console.log(globalData);
});
```

## Usage Example with Events:

```js
var CoinMarketCap = require('.');

var options = {
	events: true, // Enable event system
	refresh: 60, // Refresh time in seconds (Default: 60)
	currency: 'EUR' // Convert price to different currencies. (Default USD)
}
var coinmarketcap = new CoinMarketCap(options);

// Trigger this event when BTC price is above than 4000
coinmarketcap.onAbove('BTC', 4000, (coin) => {
	console.log('BTC price is above than 4000 of your defined currency');
});

// Trigger this event when BTC percent change is above than 20
coinmarketcap.onPercentChange24h('BTC', 20, (coin) => {
	console.log('BTC has a percent change above 20% in the last 24 hours');
});

// Trigger this event every 60 seconds with information about BTC
coinmarketcap.on('BTC', (coin) => {
	console.log(coin);
});


// Trigger this event every 60 seconds to get currency information
coinmarketcap.onMulti((coins, event) => {
	console.log('Refreshing data...');
    console.log(coins.get('BTC').price_usd); // Prints price of BTC in USD
    console.log(coins.get('ETH').price_usd); // Print price of ETH in USD
    console.log(coins.get('LTC').price_btc); // Print price of LTC in BTC
    console.log(coins.getTop(10)); // Prints information about top 10 cryptocurrencies
});

```
For a full list of examples with simple requests, check: https://github.com/hvmonteiro/node-coinmarketcap-rest-api/blob/master/example1.js 
  
For a full list of examples with events, check: https://github.com/hvmonteiro/node-coinmarketcap-rest-api/blob/master/example2.js  
