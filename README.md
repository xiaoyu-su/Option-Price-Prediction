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

- ContractSymbol: Unique identifier for the option contract

- LastTradeDate: Timestamp of the most recent trade

- Strike: Strike price of the option

- LastPrice: Most recent transaction price (market price)

- Bid: Highest price buyers are willing to pay

- Ask: Lowest price sellers are willing to accept

- Change: Price change since previous close

- PercentChange: Percent price change since previous close

- Volume: Number of contracts traded on the current day

- OpenInterest: Total number of outstanding contracts

- ImpliedVolatility: Market’s forecast of future volatility implied by option price

- InTheMoney: Boolean indicating if the option is currently profitable to exercise

- OptionType: Type of option, either call or put

- ExpirationDate: Date when the option expires

- Ticker: Underlying stock symbol

- Spot Price

Our dataset covers multiple companies(Facebook(Meta), Apple, Amazon, Netflix, Google) and all available expiration dates, providing a comprehensive snapshot of the options market over time. This allows us to analyze real market prices alongside theoretical benchmarks like the Black-Scholes model.

Though our current focus is on a selected group of companies, the data collection code is flexible and can be extended to include any publicly traded company with options data available on Yahoo Finance.

This dataset serves as the foundation for building predictive models that aim to capture market realities beyond classical option pricing theory.

### Data Cleansing

In the data cleansing process, we first removed entries with missing values and ensured that all numerical fields (e.g., prices, volume) were positive as a basic sanity check. We then standardized units for consistency and ease of use. To improve data quality, we applied several filters: we kept only options with moneyness between 0.8 and 1.2, bid-ask spread less than 50% of the mid-price, and more than one day remaining until expiration. These steps help ensure that the dataset reflects liquid, relevant options with reliable pricing signals.

## Modeling pipeline

In the belowed subsections, we outlined our strategy for building models, comparing models, and determining the best modeling with respect to our scoring.

### Interested features

Our project implements a comprehensive machine learning pipeline to predict option prices more accurately than the theoretical Black-Scholes (BS) benchmark. As mentioned in the introduction, the features of our interests are

- Bs_price: The theoretical Black-Scholes price

- ImpliedVolatility: The market’s expectation of future volatility

- Log_moneyness: The logarithm of strike price relative to the spot price, a normalized measure of moneyness

- Ask_bid_spread: A liquidity indicator calculated as the difference between ask and bid prices

These features were selected for their financial relevance. On the powerset of these four features, we will perform the following modelings and analysis.

### Analysis and Scoring

The models included in our pipeline are:

- BS-Model (baseline)

- Multiple Linear Regression (MLR)

- k-Nearest Neighbors (KNN)

- Ridge and Lasso Regression

- Bagging Regressor

- Random Forest

- XGBoost

- Neural Network

- Polynomial Regression Methods

We use GridSearchCV to systematically tune hyperparameters for each model, ensuring optimal performance within a cross-validated framework. Model evaluation is primarily based on Mean Squared Error (MSE), allowing consistent grading of model accuracy across varying levels of complexity.

Each model is trained using cleaned and filtered option price data, and we compare predictions against observed market prices (lastPrice), and grade the training using MSE. This modeling pipeline enables us to assess the ability of both linear and nonlinear methods to capture the deviations between theoretical and actual market behavior.

### Visualizations
After the analysis is performed for each firm, we will generate a scatter plot of the best model's predicted price against lastPrice. The closer our data is to the y=x line implies that it offers a better prediction towards the market's price. 

We also generate an aggregate bar chart for all companies. It shows the mean squared error (MSE) between the predicted option prices from each model and the observed lastPrice. Note that the MSEs in the bar chart are not the actual values — we applied a scaling factor to make the Black-Scholes model's price the same across all companies and scaled the predictions from all other models by the same factor. This allows for a more intuitive visual comparison of the relative improvement each model achieves, as reflected by the reduction in bar height.











