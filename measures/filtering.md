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

## ALLSELECTED combined with ALLEXCEPT
There is not excact ALLEXCEPT function that would work for ALLSELECTED. Nevertheless it is possible to get the same result by combining ALLSELECTED with VALUES.
The idea is to keep in VALUES columns that you want to remove from ALLSELECTED influence

```
    CALCULATE(
        DISTINCTCOUNT(q6_satisfaction[Respondent ID]),
        VALUES(q6_satisfaction[Subcategory_]),
        VALUES(q6_satisfaction[Question]),
        ALLSELECTED(q6_satisfaction)
    )
```
