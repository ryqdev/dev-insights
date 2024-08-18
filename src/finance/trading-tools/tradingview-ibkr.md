# TradingView - Interactive Brokers

## Operation System Information
Linux 6.5.3-1-MANJARO x86_64 GNU/Linux

## Basic Architecture of Trading System

![basic-arch](https://github.com/ryqdev/missing/assets/50010920/6edbf6a1-8485-4729-91f3-6288483e9912)

1. Data Source: There are so many data source in the market and Internet. 
2. Strategy: That's the strategy developed by us. 
3. Order: That's the part to make orders to brokers. 
4. Brokers: e.g. IBKR 
5. Exchanges: e.g. Nasdaq

The orange part is what we should do.

### The process of developing a strategy


## Python Architecture

![python-arch](https://github.com/ryqdev/missing/assets/50010920/273fd0a5-faba-473a-8a44-0af4e4c9cdc6)

Why MQ? 
1. Increase throughput.
2. decouple the components, e.g. I can use Python programs as producers and C++ programs as consumers.


Redis subscribe/publish model, MQ

Flask

Webhook

IBKR

websocket

websocket vs socket

which kind of SQL to use

Compare with Golang

streamlit

what is web socket

[ib_insync](https://github.com/erdewit/ib_insync)


## Golang Architecture

TODO

![golang-arch](https://github.com/ryqdev/missing/assets/50010920/6df7bfd6-11d5-49c6-8f81-99b2f52db1e9)

## TradingView

## Backend
### Flask

### Redis MQ

### WebUI


## IBKR API
### What is IBKR
Interactive Brokers (Nasdaq: IBKR) is an American multinational brokerage firm founded in 1977. 

### Advantages and disadvantages of IBKR
#### Advantages
- Invest globally in stocks, options, futures, foreign exchange and so on.
- Great order execution in speed and price.


#### Disadvantages
- The bad User Interface. However, programmers tend to use API rather than UI, so the hard-to-use UI doesn't count too much


### [IBKR API](https://www.interactivebrokers.com/en/trading/ib-api.php)
There are three kind of APIs from IBK: Web API, FIX API and TWS API

![IBKR-api](https://github.com/ryqdev/missing/assets/50010920/f52a9ae2-0503-47e1-a58c-632f9e8993d5)

I choose TWS API

[Installation Instructions](https://www.interactivebrokers.com/en/index.php?f=16042)

Login with paper trading account

![IBKR-paper-trading](https://github.com/ryqdev/missing/assets/50010920/165c3b9a-78ba-453f-bc28-c988f65a8e93)

[TWS API Documentation](https://ibkrcampus.com/ibkr-api-page/twsapi-doc/)

The important part should be:

![IBKR-api-settings](https://github.com/ryqdev/missing/assets/50010920/68505494-4442-4b9f-8341-6d73e6800d30)

### Connect IBKR TWS with Python 
I will use ib_insync.

[ib_insync](https://github.com/erdewit/ib_insync): Unfortunately, the author Ewald de Wit passed away in 2024. R.I.P

