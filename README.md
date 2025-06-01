
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



