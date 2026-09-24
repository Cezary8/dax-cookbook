# Average for last 6 months for KPI

```
VAR MaxDate = EOMONTH ( MAX ( 'Calendar'[Date] ), -2 )

VAR L6M = DATESINPERIOD(
                'Calendar'[Date],
                MaxDate,
                -6,
                MONTH
            )

VAR Result = CALCULATE(
                AVERAGEX(
                    VALUES('Calendar'[Month Short]),
                    CALCULATE([Demand Realization Qty Delivery Date])
                )
                ,L6M
            )

RETURN Result
```
