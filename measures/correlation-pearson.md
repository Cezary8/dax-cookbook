# Correlation coefficient (Pearson)

Correlation between two measures: 

## Definition
```
Pearson corelation rent vs area =
var __avgRent =
    CALCULATE(
        AVERAGE(leases[achieved rent]),
        ALLSELECTED(leases) )
var __avgTerm =
    CALCULATE(
        AVERAGE(leases[remaining term (key lease event)]),
        ALLSELECTED(leases) )
var __nominator =
    SUMX(leases,
        (leases[achieved rent]-__avgRent)*(leases[remaining term (key lease event)]-__avgTerm))
var __sqRent =
    SUMX(leases,
        POWER(leases[achieved rent]-__avgRent,2) )
var __sqArea =
    SUMX(leases,
        POWER(leases[remaining term (key lease event)]-__avgTerm,2))
var __Pearson =
    DIVIDE(
        __nominator,
        SQRT(__sqRent*__avgTerm),
        BLANK() )
return
__Pearson
```
