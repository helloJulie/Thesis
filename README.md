
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

$$\Huge R_t = \frac{P_t - P_{t-1}}{P_{t-1}}
$$

Where:
- $R_t$ is the rate of return at time $t$
- $P_t$ is the price at time $t$
- $P_{t-1}$ is the price at time $t-1$

```sql
WITH CombinedTable AS (
    SELECT TO_CHAR(TO_DATE(s.Date::TEXT, 'YYYYMMDD'), 'DD/MM/YYYY') AS Date,
           s.CIECH, s.GINOROSSI, s.JUTRZENKA, s.KRUK, s.TAURONPE, s.WILBO, w.WIG
    FROM stock_prices s 
    JOIN wig_prices w ON s.Date = w.Date
),
DailyChanges AS (
    SELECT Date,
           ROUND(((CIECH - LAG(CIECH) OVER (ORDER BY Date)) / LAG(CIECH) OVER (ORDER BY Date)), 3) AS CIECH_Change,
           ROUND(((GINOROSSI - LAG(GINOROSSI) OVER (ORDER BY Date)) / LAG(GINOROSSI) OVER (ORDER BY Date)), 3) AS GINOROSSI_Change,
           ROUND(((JUTRZENKA - LAG(JUTRZENKA) OVER (ORDER BY Date)) / LAG(JUTRZENKA) OVER (ORDER BY Date)), 3) AS JUTRZENKA_Change,
           ROUND(((KRUK - LAG(KRUK) OVER (ORDER BY Date)) / LAG(KRUK) OVER (ORDER BY Date)), 3) AS KRUK_Change,
           ROUND(((TAURONPE - LAG(TAURONPE) OVER (ORDER BY Date)) / LAG(TAURONPE) OVER (ORDER BY Date)), 3) AS TAURONPE_Change,
           ROUND(((WILBO - LAG(WILBO) OVER (ORDER BY Date)) / LAG(WILBO) OVER (ORDER BY Date)), 3) AS WILBO_Change,
           ROUND(((WIG - LAG(WIG) OVER (ORDER BY Date)) / LAG(WIG) OVER (ORDER BY Date)), 3) AS WIG_Change
    FROM CombinedTable
)
SELECT * FROM DailyChanges;


## Daily Changes Table

| Date       | CIECH_Change | GINOROSSI_Change | JUTRZENKA_Change | KRUK_Change | TAURONPE_Change | WILBO_Change | WIG_Change |
|------------|--------------|------------------|------------------|-------------|-----------------|--------------|-------------|
| 03/01/2012 | -0.014       | 0.000            | 0.038            | 0.005       | 0.017           | 0.030        | 0.004       |
| 04/01/2012 | -0.009       | -0.051           | 0.012            | -0.005      | 0.007           | 0.000        | -0.007      |
| 05/01/2012 | -0.012       | 0.000            | -0.028           | -0.005      | -0.006          | 0.029        | -0.012      |
| 09/01/2012 | -0.029       | 0.007            | -0.012           | 0.009       | -0.009          | 0.000        | -0.011      |
| 10/01/2012 | 0.075        | 0.000            | -0.008           | -0.016      | 0.000           | -0.057       | 0.007       |


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

The sample standard deviation **(s)** is a statistical measure used to estimate the standard deviation of a population based on sample drawn from that population. It is not the standard deviation value of the sample itself. We consider selected data for three months as a part of a large population.

$$\Huge s = \sqrt{\frac{\sum_{i=1}^N (x_i - \bar{x})^2}{N - 1}}
$$

Where:
- $s$ = sample standard deviation
- $N$ = the number of observations
- $x_i$ = the observed values of a sample item
- $\bar{x}$ = the mean value of the observations

We use MS Excel formula =AVARAGE() and =STDEV.S()

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



