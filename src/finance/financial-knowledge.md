# Financial Basic

## Terminology

### Secirities

Securities are fungible and tradable financial instruments used to raise capital in public and private markets. There are primarily three types of securities: 
1. `equity`: which provides ownership rights to holders
2. `debt`: essentially loans repaid with periodic payments
3. `hybrids`: combine aspects of debt and equity.

### Quote
A financial quotation refers to specific market data relating to a security or commodity. While the term quote specifically refers to the bid price or ask price of an instrument, it may be more generically used to relate to the last price which this security traded at ("last sale").

### Alpha
Alpha shows how well (or badly) a stock has performed in comparison to a benchmark index.

well-known paper: [alpha101](https://arxiv.org/pdf/1601.00991)

### Beta
Beta is the volatility of a security or portfolio against its benchmark index.

### Speculation
Assumption of unusual business risk in hopes of obtaining commensurate gain

### Bonds
#### Effective Duration
TODO

### Options
Option is a contract between buyer and seller. The buyer has the right to `buy` a certain stock at `strike price` up until a defined `expiration date`. More details will be contained in `Options` Chapter below


## Options
### Basic Option Types
There are two types of option:
- Call Option: The buyer has the right to `buy` a certain stock at `strike price` up until a defined `expiration date`
- Put Option: The buyer has the right to `sell` a certain stock at `strike price` up until a defined `expiration date`

Note: `American option` may be exercised at any time before the expiration date!
(My experience is that In-The-Money American options is frequently exercised)

Small tips: when we need to increase the leverage, use the option might be the good idea if we can control the risk.

### Option Greeks
Option Greeks are financial metrics that can be used to measure the price of options.
- Delta: Measures impact of a change in the price of underlying.
- Gamma: The `delta` of `delta`.
- Theta: Measures impact of a change in time remaining.
- Vega: Measures impact of a change in volatility.
- Rho: Measures impact of interest rates.

### Black-Scholes Model
Black-Schole Model in option is to calculate the price of an option.


### Put-Call Parity
It will help us further the understanding of options. 

Put and Call option can convert in some scenarios.

$$ P + S = C + PV(x) $$

Where:
- C: Price of the `European` call option 
- PV(x): Present value of the strike price (x), discounted from the value on the expiration
date at the risk-free rate 
- P: Price of the European put 
- S: Spot price or the current market value of the underlying asset


## Appendix
https://www.youtube.com/watch?v=PHe0bXAIuk0&t=3s&ab_channel=PrinciplesbyRayDalio