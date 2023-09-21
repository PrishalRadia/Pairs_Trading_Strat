# Pairs_Trading_Strat
This notebook serves as a theoretical model to test out a statistical arbitrage trading strategy which seeks 
to capture profits from the mean reversion of the spread of two co-integrated securities.

Using the YFinance library we can download the prices of the gold (GLD) and gold miners (GDX) ETF's from the 
1st of January 2018 onwards.



Using Ordinary Least Squares regression on the first 5 years of data (up to 31/12/22)  we can determine a constant hedge ratio and then 
construct a spread of the two ETF's using this ratio. We then conduct an Augmented Dickey Fuller test on the the spread which will test 
for stationarity and if this pair is sufficiently mean-reverting for us to proceed.
The result of the test was -3.115916116809538 hence we can reject the null hypothesis at the 5% significance level and use this pair to trade.

We then proceed to deal with the issue of a dynamic hedge ratio to capture the varying dynamics of the spread.
We calulate a Rolling OLS regression of the two securites with a look-back window of 20 days. This lookback window helps reduce look ahed bias as 
we have a rolling window of in-sample data.
The spread with the rolling hedge ratio can be seen below:
![image](https://github.com/PrishalRadia/Pairs_Trading_Strat/assets/140926795/75e2c977-a5f9-4142-8b98-21048fd61eb8)

Using a Bollinger Band strategy similar to the one in the book Algorithmic Trading by Ernest Chan, we 
