# Quant-trading

Most scripts inside this repository are technical indicator automated trading. There are various types of momentum trading, opening range breakout and statistical arbitrage strategies. Yet, quantitative trading is not only about technical analysis. It can refer to computational finance to exploit derivative price mismatch, pattern recognition on alternative datasets to generate alphas or low latency order execution in the market microstructure. Hence, there are a few ongoing projects inside this repository. These projects are mostly strange trading ideas I come up with to beat the market (or so I thought). There is no options strategy or HFT strategy simply because option price data and ultra high frequency data are very expensive to acquire (even consider platforms like Quantopian or Quandl). Additionally, please note that, all scripts are historical data backtesting (basically via Python, not C++, maybe Julia in the near future). The assumption is that all trades are frictionless so we don't have to worry about slippage or liquidity. 

### Table of Contents

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#1-macd-oscillator>1.MACD oscillator</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#2-pair-trading>2.Pair trading</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#3-heikin-ashi-candlestick>3.Heikin-Ashi candlestick</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#4-london-breakout>4.London Breakout</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#5awesome-oscillator>5.Awesome oscillator</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#6oil-money-project>6.Oil Money project</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#7dual-thrust>7.Dual Thrust</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#8parabolic-sar>8.Parabolic SAR</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#9bollinger-bands-pattern-recognition>9.Bollinger Bands Pattern Recognition</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#10-relative-strength-index-pattern-recognition>10.Relative Strength Index Pattern Recognition</a>

<a href=https://github.com/tattooday/quant-trading/blob/master/README.md#11monte-carlo-project>11.Monte Carlo project</a>

# Strategies:

### 1. MACD oscillator

MACD oscillator is trading strategy 101. MACD refers to Moving Average Convergence/Divergence. It is a momentum trading strategy which holds the belief that upward/downward momentum has more impact on short term moving average than long term moving average. It only takes 5 minutes for any bloke with no background in finance to trade with MACD signals. Regarding the simplicity of MACD oscillator, it is the most common strategy among the non-professionals in the market. In behavioral economics, the more people believe in the strategy, the more effective the strategy becomes (not always true, e.g. 2008). Therefore, we should not underestimate the power of MACD oscillator.

For the strategy itself, we compute long term moving average and short term moving average on the close price of a given stock. To generate the trading signal, we implement a comparison between the moving averages of different time horizons. When short term moving average is above long term moving average, we long the given stock accordingly. Vice versa.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/macd%20positions.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/macd%20oscillator.png)

### 2. Pair trading

Pair trading is the basic form of statistics arbitrage. It relies on the assumption that two cointegrated stocks would not drift too far away from each other. First step, we select two stocks and run <a href=https://en.wikipedia.org/wiki/Error_correction_model#Engle_and_Granger_2-step_approach>Engle-Granger two step analysis</a>. Once the criteria of cointegration is met, we standardize the residual and set one sigma away (two tailed) as the threshold. After that, we compute the current standardized residual of the selected stocks accordingly. When the standardized residual exceeds the threshold, it generates the trading signal. The simple rule is we always long the cheap stock and short the expensive stock. 

The core idea of pair trading is <a href=https://en.wikipedia.org/wiki/Cointegration>cointegration</a>. Metaphorically speaking, cointegration is like a couple in a clingy relationship where two parties are crazy-glued together. Yet, most relationships break sooner or later, and only the very few can make it to the marriage (from a statistics perspective, not being pessimistic). Hence, it is important to frequently check on the status quo of cointegration before any pair trading order execution (the same applies to relationships).

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/pair%20trading%20eg%20two%20step.PNG)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/pair%20trading%20z%20stats.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/pair%20trading%20positions.png)

### 3. Heikin-Ashi candlestick

Heikin-Ashi, the exotic name actually referring to 'Average Bar' in Japanese, is an alternative style of candlestick chart. The sophisticated rules of Heiki-Ashi are designed to filter out the noise for momentum trading. Hence, Heikin-Ashi shows more consecutive bars in contrast to the standard candlestick, which makes price momentum and reverse points more distinguishable in figures. Arguably it should outperform the standard candlestick in sideways and choppy markets. 

For the strategy itself, initially we make a few transformations on four vital benchmarks - Open, Close, High, Low. The next step is to apply unique Heikin-Ashi rules on Heikin-Ashi Open, Close, High, Low to generate trading signals. The downside of Heikin-Ashi (or any momentum trading strategies) is the slow response. Thus, we should set up the stop loss position accordingly so that we don't get caught up in any flash crash.

The rules of Heikin-Ashi can be found in <a href=https://quantiacs.com/Blog/Intro-to-Algorithmic-Trading-with-Heikin-Ashi.aspx>Quantiacs</a>.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/heikin-ashi%20positions.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/heikin-ashi%20asset%20value.png)

### 4. London Breakout

To one of my favourite cities in the world! Proud to be a Londoner!

London Breakout is an intra daily opening range breakout strategy. Basically, it is a fascinating information arbitrage across different markets in different time zones. FX market runs 24/7 globally. For instance, you cannot long the stock of Ford in ASX simply because Ford is listed in NYSE. As FX market is decentralised, you can long any currency pair in any market as long as the market is open. That leaves a door to take a peek at the activity in a closed foreign FX market before the opening of domestic FX market.

Back to London Breakout, London and Tokyo are two of the largest FX markets in the world. Tokyo FX trading hour is GMT 0:00 a.m. - GMT 8:59am. London FX trading hour (no summer daylight saving) begins at GMT 8:00 a.m. Even though there is an hour of overlap, the crucial timeframe of London Breakout is GMT 7:00 a.m. - GMT 7:59 a.m. a.k.a. the last trading hour before the opening of London market. The price movement of the crucial timeframe incorporates the information of all the overnight activities of financial market (from the perspective of the current time zone).

For the strategy itself, we establish upper and lower thresholds prior to the high and low of the crucial timeframe. Once London FX market opens, we spend the first couple of minutes to check if the price would breach the preset boundaries. If it is above threshold, we long the currency pair accordingly. Vice versa. Nevertheless, we should set up a limit to prevent us from trading in the case of abnormal opening volatility. Normally, we clear our positions based on our target stop loss or stop profit respectively. By the end of the trading hour (still from the perspective of the current time zone), if there are any open positions, we clear them out.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/london%20breakout%20positions.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/london%20breakout%20thresholds.png)

### 5.Awesome oscillator

Awesome oscillator is similar to MACD oscillator. Both of them are considered as momentum strategies which focus on the game of moving average. Instead of taking simple moving average on close price, awesome moving average is based on the mean of high and low price. Apart from the traditional moving average divergence, there is additional way for awesome oscillator to generate signals, which is called saucer. Saucer is slightly more complex to implement but it has the power to beat the slow response of the traditional divergence. Generally speaking, a faster response doesn't guarantee a more profitable or less risky outcome. 

The rules of awesome oscillator could be found in <a href=https://www.tradingview.com/wiki/Awesome_Oscillator_(AO)>TradingView</a>.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/awesome%20positions.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/awesome%20oscillator.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/awesome%20ma.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/awesome%20asset.png)

### 6.Oil Money project

This project is inspired by an <a href=https://www.bloomberg.com/news/articles/2018-05-20/crude-oil-s-surge-is-putting-the-petro-back-in-petrocurrencies>article</a> on oil-backed foreign exchange. When the oil exits the bear market, the currency exchange of oil producing countries would also bounce back. But, does this statement really hold? With academic analysis and computer simulation on a couple of petrocurrencies, we have found a deeper level of the story. 

For more details, please refer to the <a href=https://github.com/tattooday/quant-trading/blob/master/Oil%20Money%20project/README.md>read me page</a> of a separate directory or <a href=https://tattooday.github.io/quant-trading/oil-money>quant trading section</a> on my personal blog.

![alt text](https://github.com/tattooday/quant-trading/blob/master/Oil%20Money%20project/preview/oil%20production%20bubble%20map.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/Oil%20Money%20project/preview/nok%20trading%20positions.png)

### 7.Dual Thrust

Dual thrust is a very popular intra daily opening range breakout strategy, especially in CTA. Initially we establish upper and lower thresholds based on previous days' open, close, high and low. When the market opens and the price exceeds certain thresholds, we would take long/short positions prior to upper/lower thresholds. The strategy is quite useful in intra daily trading. However, there is no stop loss/profit position in this strategy. We reverse our positions when the price goes from one threshold to the other. We need to clear all positions by the end of the day.

Rules of dual thrust can be found in <a href=https://www.quantconnect.com/tutorials/dual-thrust-trading-algorithm>QuantConnect</a>.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/dual%20thrust%20positions.png)

### 8.Parabolic SAR

Parabolic SAR is an indicator to identify stop and reverse of a trend. When it is an uptrend, SAR curve would sit below the price. When it is downtrend, SAR curve would rise above the price. It is always considered as a symbol of resistance to the price momentum. When SAR curve and the price curve crosses over, it is when trades should be executed. The strategy of divergence is very similar to MACD oscillator (as most momentum trading strategies are). However, the calculation of SAR is extremely painful. Information on Parabolic SAR can be found in <a href=https://en.wikipedia.org/wiki/Parabolic_SAR>Wikipedia</a> but not very well explained. The most straight forward answer is to take a look at the <a href=https://www.box.com/s/gbtrjuoktgyag56j6lv0>spreadsheet</a> made by joeu2004. 

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/parabolic%20sar%20positions.png)

### 9.Bollinger Bands Pattern Recognition

Bollinger Bands is a very simple but powerful indicator. There are three bands of this indicator. The mid band is the moving average on the price series. The upper and lower bands are two moving standard deviations away from the moving average. The indicators can be used to test for various types of strategies. For volatility trading, contraction and expansion of the band width is an key element. For momentum trading, the phenomenon of 'walking the band' indicates the resistance and support level of the underlying asset. For pattern recognition, Bollinger Bands has the capability of testing bottom W, top M, head-shoulder patterns and etc.

For the rules of Bollinger Bands, please refer to <a href=https://www.tradingview.com/wiki/Bollinger_Bands_(BB)>TradingView</a>.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/bollinger%20bands%20positions.png)

### 10. Relative Strength Index Pattern Recognition

RSI (Relative Strength Index) is also a popular indicator. It reflects the current strength/weakness of the stock price momentum. The calculation is pretty straight forward. We use 14 days of smoothed moving average (or other moving average methods) to separately calculate the intra daily uptrend and downtrend. We denote uptrend moving average divided by downtrend moving average as the relative strength. We normalize the relative strength by 100 which becomes an index called RSI. It is commonly believed that RSI above 70 is overbought and RSI below 30 is oversold. Nonetheless, there could be divergence between RSI momentum and price momentum which will not be covered in the script. The effectiveness of any divergence strategy on RSI is rather debatable.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/rsi%20positions.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/rsi%20oscillator.png)

We can apply pattern recognition technique to RSI as well. Unlike strategy No.9 Bollinger Bands, we can directly look at the patterns of RSI itself instead of the price. Since we have tested double bottom pattern in Bollinger Bands, we would test head-shoulder pattern on RSI this time.

For details of head-shoulder pattern, please refer to <a href=https://www.investopedia.com/terms/h/head-shoulders.asp>Investopedia</a>.

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/rsi%20pattern%20positions.png)

![alt text](https://github.com/tattooday/quant-trading/blob/master/preview/rsi%20pattern%20oscillator.png)

### 11.Monte Carlo project

![alt text](https://raw.githubusercontent.com/tattooday/quant-trading/master/Monte%20Carlo%20project/preview/simulation.png)



STAY TUNED
