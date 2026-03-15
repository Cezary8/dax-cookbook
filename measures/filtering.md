# Filtering

## KEEPFILTERS
  Modifies filtering in CALCULATE or CALCULATETABLE
  Instead of overwrite context filters with new filters, KEEPFILTERS forces to intersect both condition. That means that this function incorporates AND operator between both conditions

```
CALCULATE(
    SUM(unpvt_Prelet[Value]),
    KEEPFILTERS(unpvt_Prelet[Attribute] = "Prelet")
)
```

## ALLSELECTED
Removes filters from current visual, but keep those applied from different visuals
Modified ALL, that removes internal filters, but keeps external ones

```
CALCULATE(
    [#bdg],
    ALLSELECTED(Schemes[Type])
)
```
