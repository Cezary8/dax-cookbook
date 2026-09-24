# Iteration based on calculatetable
```
VAR Period =
    CALCULATETABLE (
        DATESINPERIOD ( 'Calendar'[Date], MAX ( 'Calendar'[Date] ), -12, MONTH ),
        'Calendar'[DateWithPurchase] = TRUE
    )
VAR Result =
    DIVIDE ( SUMX ( Period, [Invoiced Amount Invoice Date] ), SUMX ( Period, [Invoiced Qty Invoice Date] ) )
RETURN
    Result
```
