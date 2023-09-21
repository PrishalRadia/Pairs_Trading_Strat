# Pairs_Trading_Strat
This notebook serves as a theoretical model to test out a statistical arbitrage trading strategy which seeks 
to capture profits from the mean reversion of the spread of two co-integrated securities.

Using the YFinance library we can download the prices of the gold (GLD) and gold miners (GDX) ETF's from the 
1st of January 2018 onwards.

![image](https://github.com/PrishalRadia/Pairs_Trading_Strat/assets/140926795/682addb1-a9f9-4b4e-834d-394904747462)


Using Ordinary Least Squares regression on the first 5 years of data (up to 31/12/22), so as to avoid look-ahead bias,  we can determine a constant hedge ratio, which is the slope of the regression, and then 
construct a spread of the two ETF's using this constant ratio. Below is the spread of GDX and GLD with constant hedge ratio:

![image](https://github.com/PrishalRadia/Pairs_Trading_Strat/assets/140926795/dc111af4-a245-4658-a1a1-f60cef4aa6cd)


We then conduct an Augmented Dickey Fuller test on the the spread which will test 
for stationarity and if this pair is sufficiently mean-reverting for us to proceed.
The result of the test was -3.115916116809538 hence we can reject the null hypothesis at the 5% significance level and use this pair to trade.

We then proceed to deal with the issue of a dynamic hedge ratio to capture the varying dynamics of the spread.
We calulate a Rolling OLS regression of the two securites with a look-back window of 20 days. This lookback window helps reduce look ahed bias as 
we have a rolling window of in-sample data.
The spread with the rolling hedge ratio can be seen below:
![image](https://github.com/PrishalRadia/Pairs_Trading_Strat/assets/140926795/75e2c977-a5f9-4142-8b98-21048fd61eb8)

Using a Bollinger Band strategy similar to the one in the book Algorithmic Trading by Ernest Chan, we short the spread (short one unit of GLD and buy beta units of GDX) when
our spread is 2 standard deviations above the rolling mean and we long the spread (buy one unit of GLD and short beta units of GDX) when the spread is 2 standard deviations 
below the rolling mean. Exiting the positions when the spread reverts back to the rolling mean or hits the stop losses at 3 standard deviations above and below.

We backtest this strategy by implementing a vectorised backtest 
