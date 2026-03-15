# Set of measures to adjust height or weight of charts

## Gives maximum value of category (maximum child value)
```
  ln max category = 
  MAXX(
      ALL(land_notes_i[issue]),
      [ln count]
  ) *1.19
```

## v2
Similar result - more sophisticated approach

```
Y axis = 
var _tab =
    SUMMARIZECOLUMNS(
        details[Question],
        DimAnswers[Answer],
        TREATAS({"Actual workplace time", "Individual preferences", "Team preferences"}, details[Question]),
        "respondents", DISTINCTCOUNT(details[Respondent ID]) / CALCULATE(DISTINCTCOUNT(details[Respondent ID]), ALLSELECTED(details))
    )
RETURN
MAXX(_tab, [respondents])*1.2
```


## max based on column with filtered out item

```
Selected KPI (max) = 
var _max =
    MAXX(
        FILTER(
            VALUES(facts[NEXT LEASE EVENT YEAR]),
            facts[NEXT LEASE EVENT YEAR]<> BLANK()
        ),
        [Selected KPI]
    )
RETURN
_max * 1.2
```
