# 🧮 DAX Measures – Global Superstore Analysis Dashboard

This file contains the key DAX calculations used in building the Global Superstore Power BI dashboard. The measures are grouped based on their analytical purpose: KPIs, profit and sales calculations, discount analysis, and shipping cost insights.

---

## 📊 1. KPI Measures


Total Sales = SUM('Orders'[Sales])

Total Profit = SUM('Orders'[Profit])

Total Orders = DISTINCTCOUNT('Orders'[Order ID])

QTY sold = SUM(Orders[Quantity])

Total Customers = DISTINCTCOUNT('Orders'[Customer ID])

## Other Measures Created

1. Shipping Cost
Shipping Cost = SUM(Orders[Shipping Cost])

2. Avg Discount
Avg Discount = AVERAGE(Orders[Discount])

3. Avg Profit
Avg Profit = AVERAGE(Orders[Profit])

4. Avg Quantity 
Avg Quantity = AVERAGE(Orders[Quantity])

5. Avg Shipping Cost
Avg Shipping Cost = AVERAGE(Orders[Shipping Cost])

6. Customers % Var 
Customers % Var = 
DIVIDE( [Total Customers] - [Customers PY], [Customers PY] )

7. Customers PY
Customers PY = 
CALCULATE( [Total Customers], SAMEPERIODLASTYEAR( 'Calendar Table'[Date] ))

8. Customers var Label
Customers var Label = 
VAR pct = [Customers % Var]
VAR arrow = IF( pct >= 0, "▲", "▼")
RETURN
    arrow & " " & FORMAT( ABS( pct ), "0.0%")

9. Orders % Var
Orders % Var = 
DIVIDE( [Total Orders] - [Orders PY], [Orders PY] )

10. Orders PY
Orders PY = 
CALCULATE( [Total Orders], SAMEPERIODLASTYEAR( 'Calendar Table'[Date] ) )

11. Orders var Label
Orders var Label = 
VAR pct = [Orders % Var]
VAR arrow = IF( pct >= 0, "▲", "▼")
RETURN
    arrow & " " & FORMAT( ABS( pct ), "0.0%")

12. Profit % Var
Profit % Var = 
DIVIDE( [Total Profit] - [Profit PY], [Profit PY])

13. Profit PY
Profit PY = 
CALCULATE(
    [Total Profit],
    SAMEPERIODLASTYEAR('Calendar Table'[Date])
)

14. Profit SE Asia
Profit SE Asia = 
CALCULATE(
    [Total Profit],
    'Orders'[Country] IN {
        "Cambodia", "Indonesia", "Malaysia", "Myanmar (Burma)",
        "Philippines", "Singapore", "Thailand", "Vietnam"
    }
)

15. Profit var Label
Profit var Label = 
VAR pct = [Profit % Var]
VAR arrow = IF( pct >= 0, "▲", "▼")
VAR color = "" 
RETURN
    arrow & " " & FORMAT( ABS( pct ), "0.0%")

16. Quantity % Var
Quantity % Var = 
DIVIDE( [QTY sold] - [Quantity PY], [Quantity PY])
 
17. Quantity PY
Quantity PY = 
CALCULATE( [QTY sold], SAMEPERIODLASTYEAR('Calendar Table'[Date])) 

18. Quantity var Label
Quantity var Label = 
VAR pct = [Quantity % Var]
VAR arrow = IF( pct >= 0, "▲", "▼")
RETURN
    arrow & " " & FORMAT( ABS( pct ), "0.0%")

19. Sales % Change
Sales % Change = 
DIVIDE([Total Sales] - [Previous Sales], [Previous Sales])

20. Sales % Var = 
DIVIDE( [Total Sales] - [Sales PY], [Sales PY])

21. Sales PY
Sales PY = 
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR('Calendar Table'[Date])
)

22. Sales Trend KPI
Sales Trend KPI = 
VAR Change = [Sales % Change]
VAR Arrow = 
    IF(Change > 0, "▲", IF(Change < 0, "▼", "▬"))
VAR Color = 
    IF(Change > 0, "Green", IF(Change < 0, "Red", "Yellow"))
RETURN 
    Arrow & " " & FORMAT(Change, "0.0%")

23. Sales var Label
Sales var Label = 
VAR pct = [Sales % Var]
VAR arrow = IF( pct >= 0, "▲", "▼")
VAR color = "" // the Card with States visual will color for us
RETURN
    arrow & " " & FORMAT( ABS( pct ), "0.0%")

 Previous Sales = 
CALCULATE([Total Sales], 
    DATEADD(Orders[Order Date], -1, YEAR)
)


## Calendar 

Calendar Table = 
ADDCOLUMNS (
    CALENDAR (MIN(Orders[Order Date]), MAX(Orders[Order Date])),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Month Number", MONTH([Date]),
    "Year-Month", FORMAT([Date], "YYYY-MM")
)
