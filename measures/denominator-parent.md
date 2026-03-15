# Denominator for share of current group

There is matrix table with 4 or 5 levels and we want to calculate share of item within particular group: if you expand 3rd level we want to see share of each item within total for 3rd level.
So each item should be devided not for grand total but for total of this of 3rd level group.
Below measure shows how to calculate denominator for such calculation - total of parent item

## Definition
```
all buildings denominator =
var __town = //measure for whole town when street is selected
CALCULATE(
        [#all buildings],
        ALL(buildings), FILTERS( buildings[town]),FILTERS( buildings[local authority]), FILTERS(buildings[region])
)
var __la = //measure for whole local authority or submarket when town is selected
CALCULATE(
        [#all buildings],  ALL(buildings), FILTERS( buildings[local authority]), FILTERS(buildings[region]),ALL(leases),
        ALL(gis_submarkets), FILTERS(gis_submarkets[submarket]), FILTERS(gis_submarkets[market])
)
var __reg = //measure for whole region or market when local authority or submarket is selected
CALCULATE(
        [#all buildings],
        ALL(buildings), FILTERS(buildings[region]), ALL(leases),
        ALL(gis_submarkets), FILTERS(gis_submarkets[market])
)
var __uk = //measure for whole UK when region or market is selected
CALCULATE(
        [#all buildings],
        ALL(buildings),ALL(leases), ALL(gis_submarkets)
    )
var __layer = //checks which dimension is filtered(selected) and implement appropiate measure
SWITCH(TRUE(),
        ISFILTERED(buildings[no and street]), __town,
        ISFILTERED(buildings[town]), __la,
        ISFILTERED(buildings[local authority]), __reg,
        ISFILTERED(gis_submarkets[submarket]), __reg,
        ISFILTERED(buildings[region]), __uk,
        ISFILTERED(gis_submarkets[market]), __uk,
        __uk
)
return
__layer
```
