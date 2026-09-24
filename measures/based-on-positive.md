```
//CM Per Mp #3 weighted average based on positive values
var _filter =
    FILTER(
        PurchaseOrderLinesInvoiced,
        PurchaseOrderLinesInvoiced[WeighedAvgCmPerMp] >0 && PurchaseOrderLinesInvoiced[QtyInvoiced] >0
    )
return
DIVIDE(
    SUMX(
        _filter,
        PurchaseOrderLinesInvoiced[WeighedAvgCmPerMp] * PurchaseOrderLinesInvoiced[QtyInvoiced]
    ), 
    SUMX(_filter, PurchaseOrderLinesInvoiced[QtyInvoiced])
)
```
