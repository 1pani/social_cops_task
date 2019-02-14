# social_cops_task
Data Science Internship Task_final

**The problem statement for the given task is as follows:**

Challenge 1: Agriculture Commodities, Prices & Seasons
Aim: Your team is working on building a variety of insight packs to measure key trends in the
Agriculture sector in India. You are presented with a data set around Agriculture and your aim is
to understand trends in APMC (Agricultural produce market committee)/mandi price & quantity
arrival data for different commodities in Maharashtra.
Objective:
1. Test and filter outliers.
2. Understand price fluctuations accounting the seasonal effect
  -  Detect seasonality type (multiplicative or additive) for each cluster of APMC and
commodities
  -  De-seasonalise prices for each commodity and APMC according to the detected
seasonality type
3. Compare prices in APMC/Mandi with MSP(Minimum Support Price)- raw and
deseasonalised
4. Flag set of APMC/mandis and commodities with highest price fluctuation across different
commodities in each relevant season, and year.


**My approach/steps involved-**
1. Simple EDA
  - Checking distribution plots
  - Checking scatter plots, boxplots of modal_price with date, year along with APMC, commodities.

2. Outlier removal
  - Used IQR approach and removed rows beyond (Q1 - 1.5.IQR) and (Q3+1.5.IQR) in modal_price and msprice

3. Again visualised the outlier_removed data

4. Used seasonal_decompose to get trend, seasonal and residual plots
  - Time series model is made up of trend, seasonal and residual portions. It can be termed as either additive or multiplicative type. 
  - y(t) = Trend + Seasonal + Residual - Additive seasonality 
  - y(t) = Trend Seasonal Residual - Multiplicative seasonality

5. Rolling mean approach to calculate seasonality,trend
  - *Trend*  = calculated via rolling mean of modal_price
  - *Detrended_add* = modal_price - trend
  - *Detrended_mult* = modal_price / trend
  - *Seasonality_a* = divided date into 4 quarters and calculated mean of detrended_add for each quarter
  - *Seasonality_m* = divided date into 4 quarters and calculated mean of detrended_mult for each quarter
  - *Residual_a* = detrended_add - seasonal_a
  - *Residual_m* = detrended_mul / seasonal_m
  - To check the seasonality type, I used the autocorrelation of residual which gives the correlation coefficient with the time lag.
  - *Additive seasonality* - It does not depend on the time lag.
  - *Multiplicative seasonality* - It does depend on the time lag.
  - Therefore compared the autocorr values of residual_a and residual_m
  - **Conclusion** - The given modal_price variation is a multiplicative model
  
 6. Performed multiplicative seasonality decomposition
  - deseasonalised = modal_price / seasonal_m
 
 7. Comparing raw/deseasonalised prices with msprice
  - Plots were plotted for each commodity and their modal_price(raw price) and for each commodity and their deseasonalised_modal_price(deseasonalised) and were compared with the corresponding msprice.
  - **Conclusion** - The plots were very similar(raw and deseasonalised) but there were variations compared to msprice.
 
 8. Checking for maximum fluctuations
  - Approach is to check the difference of deseasonalised max_prices and deseasonalised min_prices
  - *fluctuation* = deseasonalised_max - deseasonalised_min
