# Python-Efficient-Frontier-Portfolio

## The Efficient Frontier Portfolio is a concept in modern portfolio theory that represents a set of optimal portfolios that offer the highest expected return for a given level of risk, or the lowest risk for a given level of expected return. It's essentially the boundary line outlining the best possible combinations of investments in a portfolio.

## The Efficient Frontier is typically plotted on a graph with expected return on the y-axis and standard deviation (a measure of risk) on the x-axis. Portfolios lying on the Efficient Frontier are considered efficient because they offer the maximum return for a given level of risk, or conversely, the minimum risk for a given level of return.

## Investors can choose a portfolio along the Efficient Frontier based on their risk tolerance and investment goals. Those seeking higher returns may select portfolios further to the right on the Frontier, accepting higher risk, while those prioritizing capital preservation may opt for portfolios closer to the left, with lower risk.

## The Efficient Frontier Portfolio helps investors make informed decisions about asset allocation by providing a clear visualization of the trade-off between risk and return. Modern portfolio management often involves optimizing portfolios to lie on or close to the Efficient Frontier to achieve the best risk-return profile.

### Importing pricing data and calculating daily returns
```
!pip install yfinance

import yfinance as yf
import pandas_datareader.data as web
import datetime as dt
import pandas as pd

start = dt.datetime(2010, 1, 1)
end = dt.datetime.now()

tickers = ['AAPL', 'MSFT', 'GOOG', 'F', 'MS']

df = pd.DataFrame()
for ticker in tickers:
  df2 = yf.download(ticker, start=start, end=end)
  df[ticker] = df2['Adj Close']

import numpy as np
return1 = np.log(df/df.shift(1))
```
### Calculating (Variance of individual securities - Average daily return - STD of individual securities)
```
return1.dropna(inplace=True)
Er = np.array(return1.describe().loc['mean'])
std = np.array(return1.describe().loc['std'])
var = std**2
```
### Creating Var Covar Matrix
```
return1.dropna(inplace=True)
Er = np.array(return1.describe().loc['mean'])
std = np.array(return1.describe().loc['std'])
var = std**2
varcovar1 = return1.cov()
```
### PortfolioEfficient Portfolio A (Using C = 0%) - Get The Weights
### PortfolioEfficient Portfolio B (Using C = 4%) - Get The Weights
### Portfolio Portfolio C using A and B
```
c1 = 0
weightA = np.linalg.inv(varcovar1) @ (Er - c1) / np.sum(np.linalg.inv(varcovar1) @ (Er - c1))
ErA = weightA@Er
VarA = weightA@varcovar1@weightA.T
stdA = np.sqrt(VarA)

c2 = 0.04
weightB = np.linalg.inv(varcovar1) @ (Er - c2) / np.sum(np.linalg.inv(varcovar1) @ (Er - c2))
ErB = weightB@Er
VarB = weightB@varcovar1@weightB.T
stdB = np.sqrt(VarB)
CovAB = weightA@varcovar1@weightB.T

wa = np.linspace(-1.5, 4.5, num = 100)
wb = 1-wa
ErC = wa*ErA + wb*ErB
stdC = np.sqrt((wa*stdA)**2 + (wb*stdB)**2 + 2*wa*wb*CovAB)
```
### IndexingGMVP (Global Minimum Variance Portfolio)
```
UM = np.ones((varcovar1.shape[0]))
GMVP = UM @  np.linalg.inv(varcovar1) / (UM @  np.linalg.inv(varcovar1) @ UM.T)
ErG = GMVP@Er
stdG = np.sqrt(GMVP@varcovar1@GMVP.T)
```
### Plot
```
import matplotlib.pyplot as plt
plt.plot(stdC, ErC)
plt.scatter(stdG, ErG)
plt.show()
```
<a href="#"><img align="left" alt="Portfolio" style="padding-right:10px;" src="https://firebasestorage.googleapis.com/v0/b/mahmoudosweb.appspot.com/o/github%2F05-14-2024.png?alt=media&token=14dd4655-ec4e-4caf-ad8b-131f22f11e9e" /></a>
