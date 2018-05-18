# Term-Structure-for-future-rates
This project is an individual final project of Fixed Income and Quant Trading in NYU Tandon school.

Problem is to use 2 factor Vasicek Model:
r(t) = x1(t) + x2(t)
dx1(t) = (u1 - k1x1(t)) dt + sigma1dW1(t)
dx2(t) = (u2 + k2x2(t)) dt + sigma2dW2(t)
dW1(t)dW2(t) = rou dt
to carry out the estimation with the constant-maturity Eurodollars.

Parameters optimized in following steps:

1. Inner loop: assume parameters, and fit x1(t); x2(t) to a set of futures observed onday t repeated for all days in the Historical Sample.
Compute sigma and rou from time series of estimated x1(t) and x2(t)

2. Outer loop: find p that best fits the whole Historical Sample of Eurodollars. Inner loop is repeated on each iteration 
as solving for optimal p.

3. On each iteration, inspect if the problem becomes collinear. If collinearity detected, implement Ridge Regression to deal with it.

4. Generate time series of the residuals for all 20 interpolated futures, and study in-sample time series properties of the residuals: 
stationarity, mean-reversion (half-life), volatility and shape of the distribution. 

Then we do following analysis on residuals:

1. Use Sample A to compute 5 cointegrated pairs of futures rates: [2y,3y], [3y,4y], [4y,5y], [2y,4y], [3y,5y]

2. Construct the following:
a. AR(1) model fitted to each of the 5 cointegrated vectors (Signal 1)
b. AR(1) model fitted to each {cointegrated vector - EMA(lambda)} (Signal 2)

3. Compute half-lives for all signals. Pick lambda to make sure that half-life of Signal 2 is 5 days

4. Signal 3 is a gaussian mixture of signals 1 & 2. Weight of the mixture is a free parameter determined using Cross-Validation Sample B.

5. Use MSE as a metric of our estimation  to analyze signal quality in Sample C.
