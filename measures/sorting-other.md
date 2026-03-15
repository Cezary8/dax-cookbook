# Custom sorting with "Other" item forced to be the last one
"Other" category needs to be the last item no metter of value, whereas other items should be sorted by KPI

## Definition
```
rank q8 = 
IF(
    [Resp q8] >0,
    IF(
        SELECTEDVALUE(q8_table[Question copy])= "Other",
        99,
        RANKX(
            ALLSELECTED(q8_table[Question copy]),
            [Resp q8]
        )
    )
)
```
