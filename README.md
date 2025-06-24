# A Prediction of Option Price for FAANG
## Introduction
In financial markets, an option’s lastPrice represents the most recent transaction price—capturing the true behavior of market participants, including factors like supply-demand imbalance, sentiment, and liquidity. The Black-Scholes model, as a well-known model for European style option price prediction, offers a theoretical estimate of what the option should be worth under a set of simplifying assumptions such as frictionless markets, constant volatility, and no arbitrage.

While Black-Scholes remains a foundational tool in option pricing, it often fails to reflect the nuanced realities of actual trading. The gap between the Black-Scholes price and the lastPrice reveals valuable information about the imperfections and behaviors in real markets.

This project aims to build an improved predictive model that captures those complexities by allowing some freedom from the theoretical assumptions of the Black-Scholes Model. By directly modeling lastPrice using data science techniques, we attempt to understand how options are truly priced in practice, and what features matters. Our model incorporates more volatilities from the market that the Black-Scholes formula overlooks. The ultimate goal is to produce a more accurate, data-informed model that better reflects real-world option pricing dynamics, both theoretically and practically.

## Data Gethering
### Data Source
Our data comes from Yahoo Finance Data sets. Description of the data can be found below

### Option Data Description
We collect option price data using the yfinance Python library, which interfaces with Yahoo Finance’s publicly available API. For each company and option expiration date, we download detailed information on both call and put options, including strike prices, last traded prices, bid-ask quotes, volume, open interest, implied volatility, and moneyness indicators. Most of the data are critical for computing the Black-Scholes Model price, while the rest are used in the features used for the models.

Here are the attributes we pulled from the raw data:

-contractSymbol: Unique identifier for the option contract

-lastTradeDate: Timestamp of the most recent trade

-strike: Strike price of the option

-lastPrice: Most recent transaction price (market price)

-bid: Highest price buyers are willing to pay

-ask: Lowest price sellers are willing to accept

-change: Price change since previous close

-percentChange: Percent price change since previous close

-volume: Number of contracts traded on the current day

-openInterest: Total number of outstanding contracts

-impliedVolatility: Market’s forecast of future volatility implied by option price

-inTheMoney: Boolean indicating if the option is currently profitable to exercise

-optionType: Type of option, either call or put

-expirationDate: Date when the option expires

-ticker: Underlying stock symbol

-Spot Price

Our dataset covers multiple companies(Facebook(Meta), Apple, Amazon, Netflix, Google) and all available expiration dates, providing a comprehensive snapshot of the options market over time. This allows us to analyze real market prices alongside theoretical benchmarks like the Black-Scholes model.

Though our current focus is on a selected group of companies, the data collection code is flexible and can be extended to include any publicly traded company with options data available on Yahoo Finance.

This dataset serves as the foundation for building predictive models that aim to capture market realities beyond classical option pricing theory.

### Data Cleansing

In the data cleansing process, we first removed entries with missing values and ensured that all numerical fields (e.g., prices, volume) were positive as a basic sanity check. We then standardized units for consistency and ease of use. To improve data quality, we applied several filters: we kept only options with moneyness between 0.8 and 1.2, bid-ask spread less than 50% of the mid-price, and more than one day remaining until expiration. These steps help ensure that the dataset reflects liquid, relevant options with reliable pricing signals.

## Modeling pipeline

In the belowed subsections, we outlined our strategy for building models, comparing models, and determining 

### Interested features

As mentioned in the introduction, we're interested in producing 









