# Improved calculation based on Group Purchase

```
Cm Per Mp L3M + Current (fast) = 
VAR _CurrentMonth = SELECTEDVALUE( Calendar[CalendarYearMonthKey] ) 
VAR _BeginPeriod = EOMONTH(MIN(Calendar[Date]), -4)
var _FilteredMonths = 
        CALCULATETABLE(
            VALUES('Calendar'[CalendarYearMonthKey]),
            CALCULATETABLE(
                VALUES('Calendar'[Date]),
                FILTER(
                    ALL('Calendar'[Date]),
                    [CM] > 0
                )
            ),
            'Calendar'[CalendarYearMonthKey] <= _CurrentMonth
        )
VAR CurrentValue =
    CALCULATE(
       DIVIDE([CM],[Invoiced Qty Invoice Date],0),
          FILTER(
            ALL('Calendar'),
            Calendar[Date] > _BeginPeriod
          ),
          _FilteredMonths
    )
RETURN
CurrentValue
```

```
! Projected CM per MP (fast) = 
VAR HasInvoiceQty =
    NOT ISBLANK ( [Invoiced Qty Invoice Date] )

VAR PrevMonthValue =
    CALCULATE (
        [Cm Per Mp L3M + Current (fast)],
        DATEADD ( 'Calendar'[Date], -1, MONTH ),
        ALL ( Supplier )
    )

VAR TwoMonthsBack =
    CALCULATE (
        [Cm Per Mp L3M + Current (fast)],
        DATEADD ( 'Calendar'[Date], -2, MONTH ),
        ALL ( Supplier )
    )
    
RETURN
ABS(SIGN([Invoiced Qty Invoice Date]))*COALESCE([Cm Per Mp L3M + Current (fast)], PrevMonthValue, TwoMonthsBack)
```

```
! Projected CM = 
VAR step1 =
    SUMMARIZE(
        PurchaseOrderLinesInvoiced,
        'Calendar'[CalendarYearMonthKey],
        'Product'[Pk  Product],
        Supplier[PkSupplier ]
        )
VAR _Result =
    SUMX(
        step1,
        [Invoiced Qty Invoice Date] * [! Projected CM per MP (fast)]
    )
RETURN
    _Result
```
