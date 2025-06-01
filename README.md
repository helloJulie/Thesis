
###  _A fragment of my thesis that somehow became this... thing._


### ThesisThing  
- Testing ideas from Modern Portfolio Theory using real stock data.
- Checking risk and expected returns of stocks.
- Showing how diversification works.
- Finding the best mix of stocks (Efficient Frontier).
- Adding a risk-free asset and calculating the Sharpe Ratio.
- Testing APT and CAPM models.

### Tools
- SQL

Joining two tables where 
- s refers to the https://github.com/helloJulie/Thesis/blob/main/Prices/stock%20prices.csv table
- w refers to the https://github.com/helloJulie/Thesis/blob/main/Prices/WIG%20prices.csv table

  
SELECT s.Date, s.CIECH, s.GINOROSSI, s.JUTRZENKA, s.KRUK, s.TAURONPE, s.WILBO, w.WIG
FROM stock_prices s
JOIN wig_prices w ON s.Date = w.Date

Create separate table with daily changes in price

WITH CombinedTable AS (
    SELECT 
        TO_CHAR(TO_DATE(s.Date::TEXT, 'YYYYMMDD'), 'YYYY-MM-DD') AS Readable_Date,
        s.CIECH, s.GINOROSSI, s.JUTRZENKA, s.KRUK, s.TAURONPE, s.WILBO, w.WIG
    FROM stock_prices s
    JOIN wig_prices w ON s.Date = w.Date
),
DailyChanges AS (
    SELECT 
        Readable_Date,
        ROUND(((CIECH - LAG(CIECH) OVER (ORDER BY Readable_Date)) / LAG(CIECH) OVER (ORDER BY Readable_Date)), 5) AS CIECH_Change,
        ROUND(((GINOROSSI - LAG(GINOROSSI) OVER (ORDER BY Readable_Date)) / LAG(GINOROSSI) OVER (ORDER BY Readable_Date)), 5) AS GINOROSSI_Change,
        ROUND(((JUTRZENKA - LAG(JUTRZENKA) OVER (ORDER BY Readable_Date)) / LAG(JUTRZENKA) OVER (ORDER BY Readable_Date)), 5) AS JUTRZENKA_Change,
        ROUND(((KRUK - LAG(KRUK) OVER (ORDER BY Readable_Date)) / LAG(KRUK) OVER (ORDER BY Readable_Date)), 5) AS KRUK_Change,
        ROUND(((TAURONPE - LAG(TAURONPE) OVER (ORDER BY Readable_Date)) / LAG(TAURONPE) OVER (ORDER BY Readable_Date)), 5) AS TAURONPE_Change,
        ROUND(((WILBO - LAG(WILBO) OVER (ORDER BY Readable_Date)) / LAG(WILBO) OVER (ORDER BY Readable_Date)), 5) AS WILBO_Change,
        ROUND(((WIG - LAG(WIG) OVER (ORDER BY Readable_Date)) / LAG(WIG) OVER (ORDER BY Readable_Date)), 5) AS WIG_Change
    FROM CombinedTable
)
SELECT * FROM DailyChanges WHERE CIECH_Change IS NOT NULL;



## 🎓 Thesis Chapter: Building a Stock Portfolio Based on H. Markowitz's Model

### 3.0 Building a Stock Portfolio Using the Markowitz Model
In the empirical section of this thesis, a series of portfolios were constructed: a Minimum Variance Portfolio (MVP), an efficient portfolio with a target return, and a comparison with the market portfolio, represented by the WIG index. The analysis was conducted over the first quarter of 2012. The stocks selected for the portfolios are all listed on the Warsaw Stock Exchange: Ciech S.A., Gino Rossi S.A., Jutrzenka S.A., Kruk S.A., Tauron Polska Energia S.A., and Wilbo S.A.

The study period spans from 02 January to 30 March 2012, during which daily prices of the selected stocks and the WIG index were collected.

### 3.1 Construction of a Portfolio with a Target Return
To construct the portfolios, daily returns of the selected stocks and the WIG index were calculated using formula (1.3). The results are shown in Appendix 2. From this dataset, the average returns (formula 1.5) and standard deviations (formula 1.9) were calculated for each stock and for the market portfolio.

Table 1 presents the average returns and standard deviations of the companies.

Based on the data in Appendix 2, correlation and covariance matrices were built for every pair of stocks. These were used in portfolio construction. Correlations were calculated using formula (1.10), and covariances with formula (1.12).

Table 2: Correlation matrix of stock returns
Table 3: Covariance matrix of stock returns

Construction of the Minimum Variance Portfolio (MVP)
To calculate the weights of each asset in the MVP, matrix algebra was applied.
Matrix C (from formula 2.5) was created and is presented in:

Table 4: Matrix C

After inverting matrix C and applying it in formula (2.5), the resulting vector provided the weights of each stock in the MVP.

Table 5: Asset weights in the MVP

Using these weights, the portfolio's expected return and standard deviation were calculated using formulas (2.1) and (2.3):

Expected Return (rp): 0.04%
Standard Deviation (σ): 1.86%
This confirms that the MVP significantly reduces risk, lowering the portfolio's standard deviation to 1.86%.

The key conclusion is that the portfolio’s return depends not only on the individual returns of the assets but also on their respective weights in the portfolio. Similarly, portfolio risk is influenced not just by the individual asset risks but also by the correlation between them.

Construction of the Efficient Portfolio with a Target Return
The next step was to construct an efficient portfolio assuming a target expected return of 10%. This portfolio is designed to minimize risk while achieving the desired return.
To determine the weights, Matrix D (from formula 2.6) was built:

Table 6: Matrix D

After inverting the matrix and multiplying it by the vector from formula (2.6), the asset weights were obtained:

Table 7: Asset weights in the portfolio with target return

Substituting these values into formulas (2.1) and (2.3) gave the following results:

Expected Return: 10%
Standard Deviation: 6.1%
This shows that achieving the desired return increases the portfolio's risk compared to the MVP.

### 3.2 Empirical Verification of Portfolios
The calculated asset weights and returns were used to compute the average portfolio returns and standard deviations. The WIG index was used as the benchmark market portfolio.

Table 8 compares the average returns and risks of the three portfolios:

Efficient Portfolio (target return): 10% return, 6.1% risk
Market Portfolio (WIG): 0.11% return, 0.84% risk
Minimum Variance Portfolio (MVP): 0.04% return, 1.86% risk
Despite achieving higher returns, the efficient portfolio also bears significantly higher risk—over 3 times that of the MVP, and 7 times that of the WIG.

# Conclusion...



