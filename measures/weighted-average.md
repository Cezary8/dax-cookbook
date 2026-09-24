# Weighted Average Purchase Lead Time
Depending on O-number selected


```
Weighted Average Medical Product Purchase Leadtime Lowest Level =
CALCULATE(
    SUM(
        SubComponentMedicalProductPurchaseLeadTime[MedicalProductPurchaseLeadTime]
    )
        * ( CALCULATE(
            DIVIDE(
                [Arrival Qty],
                CALCULATE(
                    [Arrival Qty],
                    ALLSELECTED( SubComponentMedicalProductPurchaseLeadTime )
                )
            )
        ) )
)
```

```
weight =
VAR ArrivalLine = SUM(SubComponentMedicalProductPurchaseLeadTime[ArrivalQty])
VAR TotalArrival = CALCULATE( SUM( SubComponentMedicalProductPurchaseLeadTime[ArrivalQty] ), ALLSELECTED( SubComponentMedicalProductPurchaseLeadTime ) )

RETURN
DIVIDE(ArrivalLine,TotalArrival)
```

```
Weighted Average Medical Product Purchase Leadtime Higest Level = 
SUMX(
    SubComponentMedicalProductPurchaseLeadTime,
    SubComponentMedicalProductPurchaseLeadTime[MedicalProductPurchaseLeadTime]
        * [Weight]
)
```


```
Weighted Average Purchase Lead Time = 
IF(
    CALCULATE(
        SUM(
            SubComponentMedicalProductPurchaseLeadTime[MedicalProductPurchaseLeadTime]
        )
    )
        <> 0,
    IF(
      HASONEVALUE( 'O-Number'[O-Number]),
        [Weighted Average Medical Product Purchase Leadtime Lowest Level],
        [Weighted Average Medical Product Purchase Leadtime Higest Level]
    ),
    BLANK( )
)
```
