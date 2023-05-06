# CAPM-Model-by-Python


- Import libraries/datasets and visualize stocks data
```py
from pandas_datareader import data as web
import pandas as pd
import numpy as np
from datetime import datetime
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
from copy import copy
from scipy import stats
import matplotlib.pyplot as plt
import plotly.figure_factory as ff
import plotly.graph_objects as go
plt.style.use('fivethirtyeight')
assets = ['IBM','PFE','META','AMZN','TSLA','NFLX','GOOG','MOD','SPY']
weights = np.array([0.2, 0.2, 0.2, 0.2, 0.2])
stocksStartDate = '2015-01-01'
today = datetime.today().strftime('%Y-%m-%d')

import yfinance as yf
df= pd.DataFrame()
for stock in assets:
    df[stock] = yf.download(stock, data_source = 'yahoo', start = stocksStartDate,
                              end = today )['Adj Close']
```

-- img of dataset
However, date now is index not a column, I reset dataset 
img of data reset

Next I visualize 9 stocks 

```py
def interactive_plot(data, title):
  fig = px.line(title = title)
  for i in data.columns[1:]:
    fig.add_scatter(x = data['Date'], y = data[i], name = i)
  fig.show()
# Plot interactive chart
interactive_plot(df_reset, 'Prices')
```

- Capital Asset Pricing Model

The formula for calculating the expected return of an asset, given its risk, is as follows:
-- img model
Investors expect to be compensated for risk and the time value of money. The risk-free rate in the CAPM formula accounts for the time value of money. The other components of the CAPM formula account for the investor taking on additional risk.

The goal of the CAPM formula is to evaluate whether a stock is fairly valued when its risk and the time value of money are compared with its expected return. In other words, by knowing the individual parts of the CAPM, it is possible to gauge whether the current price of a stock is consistent with its likely return.
 - Risk Free Asset (rf): A risk-free asset is one that has a certain future return—and virtually no possibility of loss. Debt obligations issued by the U.S. Department of the Treasury (bonds, notes, and especially Treasury bills) are considered to be risk-free because the "full faith and credit" of the U.S. government backs them. 
- Market Portfolio(rm) A market portfolio is a theoretical bundle of investments that includes every type of asset available in the investment universe, with each asset weighted in proportion to its total presence in the market. The expected return of a market portfolio is identical to the expected return of the market as a whole. Market portfolios are a key part of the capital asset pricing model, a commonly used foundation for choosing which investments to add to a diversified portfolio. The market portfolio is an essential component of the capital asset pricing model (CAPM). Widely used for pricing assets, especially equities, the CAPM shows what an asset's expected return should be based on its amount of systematic risk. The relationship between these two items is expressed in an equation called the security market line.
 - Beta
Beta (β), primarily used in the capital asset pricing model (CAPM), is a measure of the volatility–or systematic risk–of a security or portfolio compared to the market as a whole.
Beta data about an individual stock can only provide an investor with an approximation of how much risk the stock will add to a (presumably) diversified portfolio.

For beta to be meaningful, the stock should be related to the benchmark that is used in the calculation.

The S&P 500 has a beta of 1.0. Stocks with betas above 1 will tend to move with more momentum than the S&P 500; stocks with betas less than 1 with less momentum.
-- img of beta
Beta Value Equal to 1.0

If a stock has a beta of 1.0, it indicates that its price activity is strongly correlated with the market. A stock with a beta of 1.0 has systematic risk. However, the beta calculation can’t detect any unsystematic risk. Adding a stock to a portfolio with a beta of 1.0 doesn’t add any risk to the portfolio, but it also doesn’t increase the likelihood that the portfolio will provide an excess return.

Beta Value Less Than One

A beta value that is less than 1.0 means that the security is theoretically less volatile than the market. Including this stock in a portfolio makes it less risky than the same portfolio without the stock. For example, utility stocks often have low betas because they tend to move more slowly than market averages.

Beta Value Greater Than One

A beta that is greater than 1.0 indicates that the security's price is theoretically more volatile than the market. For example, if a stock's beta is 1.2, it is assumed to be 20% more volatile than the market. Technology stocks and small cap stocks tend to have higher betas than the market benchmark. This indicates that adding the stock to a portfolio will increase the portfolio’s risk, but may also increase its expected return.

Negative Beta Value

Some stocks have negative betas. A beta of -1.0 means that the stock is inversely correlated to the market benchmark on a 1:1 basis. This stock could be thought of as an opposite, mirror image of the benchmark’s trends. Put options and inverse ETFs are designed to have negative betas. There are also a few industry groups, like gold miners, where a negative beta is also common.

# Calculate the daily returns
```py
def daily_return(data):

  df_daily_return = data.copy()
  
  # Loop through each stock
  for i in data.columns[1:]:
    
    # Loop through each row belonging to the stock
    for j in range(1, len(data)):
      
      # Calculate the percentage of change from the previous day
      df_daily_return[i][j] = ((data[i][j]- data[i][j-1])/data[i][j-1]) * 100
    
    # set the value of first row to zero, as previous value is not available
    df_daily_return[i][0] = 0
  return df_daily_return
```
# Calculate beta for a single stock
```py
stocks_daily_return['IBM']
stocks_daily_return['SPY'].head()

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['IBM'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('IBM', round(beta,3), round(alpha,3)))

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['PFE'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('PFE', round(beta,3), round(alpha,3)))

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['META'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('META', round(beta,3), round(alpha,3))) 

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['AMZN'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('AMZN', round(beta,3), round(alpha,3)))

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['TSLA'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('TSLA', round(beta,3), round(alpha,3)))

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['NFLX'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('NFLX', round(beta,3), round(alpha,3)))

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['GOOG'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('GOOG', round(beta,3), round(alpha,3)))

beta, alpha = np.polyfit(stocks_daily_return['SPY'], stocks_daily_return['MOD'], 1)
print('Beta for {} stock is = {} and alpha is = {}'.format('MOD', round(beta,3), round(alpha,3)))
```

# 
Assume risk free rate is zero
Also you can use the yield of a 10-years U.S. Government bond as a risk free rate
rf = 0.66 

# Calculate rm
```py
# Define the expected return dictionary
ER = {}

rf = 0.66
rm = round(stocks_daily_return['SPY'].mean() * 252,3) # this is the expected return of the market 
rm
```

```py
for i in keys:
  # Calculate return for every security using CAPM  
  ER[i] = rf + ( beta[i] * (rm-rf) ) 
for i in keys:
  print('Expected Return Based on CAPM for {} is {}%'.format(i, round(ER[i],3)))
```
Expected Return Based on CAPM for IBM is 10.326%
Expected Return Based on CAPM for PFE is 7.912%
Expected Return Based on CAPM for META is 14.857%
Expected Return Based on CAPM for AMZN is 13.406%
Expected Return Based on CAPM for TSLA is 16.829%
Expected Return Based on CAPM for NFLX is 13.931%
Expected Return Based on CAPM for GOOG is 13.575%
Expected Return Based on CAPM for MOD is 15.125%

# Calculate the portfolio return
```py
ER_portfolio_all = round(sum(list(ER.values()) * portfolio_weights),3)
print('Expected Return Based on CAPM for the portfolio  is {}%\n'.format(ER_portfolio_all))
```


```py
# Calculate the portfolio return 
ER_portfolio_ConsumerServices = round(0.50 * ER['PFE'] +  0.50 * ER['TSLA'],3)
print('Expected Return Based on CAPM for the portfolio (Consumer Services) is {}%\n'
      .format(ER_portfolio_ConsumerServices))
```
Expected Return Based on CAPM for the portfolio  is 15.137%

```py
# Calculate the portfolio return 
ER_portfolio_ConsumerServices = round(0.50 * ER['PFE'] +  0.50 * ER['TSLA'],3)
print('Expected Return Based on CAPM for the portfolio (Consumer Services) is {}%\n'
      .format(ER_portfolio_ConsumerServices))
```
Expected Return Based on CAPM for the portfolio (Consumer Services) is 12.371%


```py
# Calculate the portfolio return 
ER_portfolio_PersonalDevices = round(0.25 * ER['META'] +  0.25 * ER['IBM']+0.25 * ER['GOOG'] +  0.25 * ER['AMZN'],3)
print('Expected Return Based on CAPM for the portfolio (Personal Devices Sector) is {}%\n'
      .format(ER_portfolio_PersonalDevices))
```
Expected Return Based on CAPM for the portfolio (Personal Devices Sector) is 13.041%

```py
# Calculate the portfolio return 
ER_portfolio_Manufacturing = round(0.50 * ER['TSLA'] +  0.50 * ER['MOD'],3)
print('Expected Return Based on CAPM for the portfolio (Manufacturing Sector) is {}%\n'
      .format(ER_portfolio_Manufacturing))
```
Expected Return Based on CAPM for the portfolio (Manufacturing Sector) is 15.977%


```py
ER_portfolio_bm = round(0.5 * ER['PFE'] +  0.5 * ER['IBM'],3)
print('Expected Return Based on CAPM for the portfolio (Less than Market Return) is {}%\n'
      .format(ER_portfolio_bm))
```
Expected Return Based on CAPM for the portfolio (Less than Market Return) is 9.119%


Expected Return Based on CAPM for the portfolio (Above than  Market Return) is 14.622%

Contribution on CAPM for IBM is 0.1
Contribution on CAPM for PFE is 0.07
Contribution on CAPM for META is 0.14
Contribution on CAPM for AMZN is 0.13
Contribution on CAPM for TSLA is 0.16
Contribution on CAPM for NFLX is 0.13
Contribution on CAPM for GOOG is 0.13
Contribution on CAPM for MOD is 0.14

```py
# Calculate the portfolio return 
ER_portfolio_ed = round(0.50 * ER['PFE'] +  0.50 * ER['TSLA'],3)
print('Expected Return Based on CAPM for the portfolio (Extremes) is {}%\n'
      .format(ER_portfolio_ed))
```
Expected Return Based on CAPM for the portfolio (Extremes) is 12.371%

`
# Calculate beta for all stocks
- Suggest investing
After calculating the expected return based on CAPM, I would recommend investing in stocks as well as combinations

















