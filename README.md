
### 🎓 _A fragment of my thesis that somehow became this... thing._











<br>
<br>
<br>
<h2 align="center">
     Building a Stock Portfolio Using the Markowitz Model 
    </h2>
     

Minimum Variance Portfolio (MVP), an efficient portfolio with a target return and a comparison with the market portfolio, represented by the WIG index. 
The analysis is conducted over the first quarter of 2012. The stocks selected for the portfolios are: Ciech S.A., Gino Rossi S.A., Jutrzenka S.A., Kruk S.A., Tauron Polska Energia S.A., Wilbo S.A.


<br>
<br>
<br>


#### -> Get the historical price data of the selected equities [stock_prices.csv](https://github.com/helloJulie/Thesis/blob/main/stock%20prices.csv) and its corresponding index (WIG) [WIG_prices.csv](https://github.com/helloJulie/Thesis/blob/main/WIG%20prices.csv)




#### -> Create separate table with daily changes in price and combine the tables.
To construct the table of portfolio daily returns we used formula

<img src="https://github.com/helloJulie/Thesis/blob/main/formulas/rate%20of%20return%20for%20a%20given%20period/%E2%80%8Erate%20of%20return%20for%20a%20given%20period.%E2%80%8E1.jpeg" width="400">  
The below syntax is a sample how we can do it using SQL
<br>
<br>

```sql
WITH CombinedTable AS (
    SELECT TO_CHAR(TO_DATE(s.Date::TEXT, 'YYYYMMDD'), 'DD/MM/YYYY') AS Date,
           s.CIECH, s.GINOROSSI, s.JUTRZENKA, s.KRUK, s.TAURONPE, s.WILBO, w.WIG
    FROM stock_prices s 
    JOIN wig_prices w ON s.Date = w.Date
),
DailyChanges AS (
    SELECT Date,
           ROUND(((CIECH - LAG(CIECH) OVER (ORDER BY Date)) / LAG(CIECH) OVER (ORDER BY Date)) * 100, 2) AS CIECH_Change,
           ROUND(((GINOROSSI - LAG(GINOROSSI) OVER (ORDER BY Date)) / LAG(GINOROSSI) OVER (ORDER BY Date)) * 100, 2) AS GINOROSSI_Change,
           ROUND(((JUTRZENKA - LAG(JUTRZENKA) OVER (ORDER BY Date)) / LAG(JUTRZENKA) OVER (ORDER BY Date)) * 100, 2) AS JUTRZENKA_Change,
           ROUND(((KRUK - LAG(KRUK) OVER (ORDER BY Date)) / LAG(KRUK) OVER (ORDER BY Date)) * 100, 2) AS KRUK_Change,
           ROUND(((TAURONPE - LAG(TAURONPE) OVER (ORDER BY Date)) / LAG(TAURONPE) OVER (ORDER BY Date)) * 100, 2) AS TAURONPE_Change,
           ROUND(((WILBO - LAG(WILBO) OVER (ORDER BY Date)) / LAG(WILBO) OVER (ORDER BY Date)) * 100, 2) AS WILBO_Change,
           ROUND(((WIG - LAG(WIG) OVER (ORDER BY Date)) / LAG(WIG) OVER (ORDER BY Date)) * 100, 2) AS WIG_Change
    FROM CombinedTable
)
SELECT * FROM DailyChanges WHERE CIECH_Change IS NOT NULL;


## Daily Changes Table

| Date       | CIECH_Change | GINOROSSI_Change | JUTRZENKA_Change | KRUK_Change | TAURONPE_Change | WILBO_Change | WIG_Change |
|------------|--------------|------------------|------------------|-------------|-----------------|--------------|-------------|
| 03/01/2012 | -1.38%       | 0.00%            | 3.75%            | 0.51%       | 1.68%           | 3.03%        | 0.38%       |
| 04/01/2012 | -0.87%       | -5.10%           | 1.20%            | -0.46%      | 0.74%           | 0.00%        | -0.73%      |
| 05/01/2012 | -1.18%       | 0.00%            | -2.78%           | -0.46%      | -0.55%          | 2.94%        | -1.16%      |
| 09/01/2012 | -2.86%       | 0.67%            | -1.22%           | 0.93%       | -0.92%          | 0.00%        | -1.05%      |
| 10/01/2012 | 7.54%        | 0.00%            | -0.83%           | -1.61%      | 0.00%           | -5.71%       | 0.68%       |


## We can now download the .csv
```
<br>
<br>

#### -> Upload .csv to MS Excel and adjust formats.

<img src="https://github.com/user-attachments/assets/d0a86eda-74e9-4a4e-8797-34c6dedbfe2c" width="1000">

<img src="https://github.com/user-attachments/assets/7498c24f-d990-43ee-ad0a-a448317870a6" width="1000">

<br>
<br>

#### -> Build the portfolio with a target return

There are several methods to calculate the average of daily return and standard deviation of each company.

The sample standard deviation **(S)** is a statistical measure used to estimate the standard deviation of a population based on sample drawn from that population. It is not the standard deviation value of the sample itself. We consider selected data for three months as a part of a large population.


<img src="https://github.com/user-attachments/assets/01d83ca7-0e62-4a58-869f-42db40c99909" width="700"> 

<br>
<br>

We use MS Excel formula =AVARAGE() and =STDEV()

<img src="https://github.com/user-attachments/assets/a525b7e3-8e0b-43c2-951d-47b5fdd5eb46" width="1000">

<img src="https://github.com/user-attachments/assets/e3d54b66-a777-4d2d-a31e-01911209ae7c" width="1000">



Based on the data in Appendix 2, correlation and covariance matrices were built for every pair of stocks. These were used in portfolio construction. Correlations were calculated using formula (1.10) and covariances with formula (1.12).

Table 2: Correlation matrix of stock returns
Table 3: Covariance matrix of stock returns




<h2 align="center">
 Construction of the Minimum Variance Portfolio (MVP)
</h2> 
</div>

To calculate the weights of each asset in the MVP, matrix algebra is applied.
Matrix C (from formula 2.5) is created and is presented below:

Table 4: Matrix C

After inverting matrix C and applying it in formula (2.5), the resulting vector provided the weights of each stock in the MVP.

Table 5: Asset weights in the MVP

Using these weights, the portfolio's expected return and standard deviation were calculated using formulas (2.1) and (2.3):

Expected Return (rp): 0.04%
Standard Deviation (σ): 1.86%
This confirms that the MVP significantly reduces risk, lowering the portfolio's standard deviation to 1.86%.

The key conclusion is that the portfolio’s return depends not only on the individual returns of the assets but also on their respective weights in the portfolio. Similarly, portfolio risk is influenced not just by the individual asset risks but also by the correlation between them.

Construction of the Efficient Portfolio with a target return




<h2 align="center">
    Construction of an efficient portfolio
    </h2> 

The next step is to build an efficient portfolio assuming a target expected return of 10%. This portfolio is designed to minimize risk while achieving the desired return.
To determine the weights, Matrix D (from formula 2.6) is built:

Table 6: Matrix D

After inverting the matrix and multiplying it by the vector from formula (2.6), the asset weights were obtained:

Table 7: Asset weights in the portfolio with target return

Substituting these values into formulas (2.1) and (2.3) gave the following results:

Expected Return: 10%
Standard Deviation: 6.1%
This shows that achieving the desired return increases the portfolio's risk compared to the MVP.


#### -> Verify
Asset weights and returns were used to compute the average portfolio returns and standard deviations. The WIG index is used as the benchmark market portfolio.

Table 8 compares the average returns and risks of the three portfolios:

Efficient Portfolio (target return): 10% return, 6.1% risk
Market Portfolio (WIG): 0.11% return, 0.84% risk
Minimum Variance Portfolio (MVP): 0.04% return, 1.86% risk
Despite achieving higher returns, the efficient portfolio also bears significantly higher risk—over 3 times that of the MVP and 7 times that of the WIG.



