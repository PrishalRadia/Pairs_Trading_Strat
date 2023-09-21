# Pairs_Trading_Strat
This notebook serves as a theoretical model to test out a statistical arbitrage trading strategy which seeks 
to capture profits from the mean reversion of the spread of two co-integrated securities.

Using the YFinance library we can download the prices of the gold (GLD) and gold miners (GDX) ETF's from the 
1st of January 2018 onwards.
Using Ordinary Least Squares regression on the first 5 years of data (up to 31/12/22)  we can determine a constant hedge ratio and then 
construct a spread of the two ETF's using this ratio. We then conduct an Augmented Dickey Fuller test on the the spread which will test 
for stationarity and if this pair is sufficiently mean-reverting for us to proceed.
The result of the test 
