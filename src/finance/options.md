# Options
My learning experience with options, the contracts giving the owner the right to buy or sell an asset at strike price

## What is secirities
Securities are fungible and tradable financial instruments used to raise capital in public and private markets. There are primarily three types of securities: 
1. equity: which provides ownership rights to holders
2. debt: essentially loans repaid with periodic payments
3. hybrids: combine aspects of debt and equity.

## Basic Option Types
There are two types of option:
- Call Option: The buyer has the right to `buy` a certain stock at `strike price` up until a defined `expiration date`
- Put Option: The buyer has the right to `sell` a certain stock at `strike price` up until a defined `expiration date`

Note: `American option` may be exercised at any time before the expiration date!
(My experience is that In-The-Money American options is frequently exercised)

Small tips: when we need to increase the leverage, use the option might be the good idea if we can control the risk.

## Option Greeks
Option Greeks are financial metrics that can be used to measure the price of options.
- Delta: Measures impact of a change in the price of underlying.
- Gamma: The `delta` of `delta`.
- Theta: Measures impact of a change in time remaining.
- Vega: Measures impact of a change in volatility.
- Rho: Measures impact of interest rates.

## Black-Scholes Model
TODO


## Put-Call Parity
It will help us further the understanding of options. 

Put and Call option can convert in some scenarios.

$$ P + S = C + PV(x) $$

Where:
- C: Price of the `European` call option 
- PV(x): Present value of the strike price (x), discounted from the value on the expiration
date at the risk-free rate 
- P: Price of the European put 
- S: Spot price or the current market value of the underlying asset
