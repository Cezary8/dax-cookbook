# Cumulative sum till selected quarter in slicer
Idea is to pick YEARQUARTER that willlimit cumulative sum.

## Calculation
```
facts rolling2 = 
var _maxq = // looking for most current quarter (had to do that on values)
    CALCULATE(
        MAX('calendar'[yyyyq val]),
        ALL('calendar')
    )
var _mfilter = //using previous var as a filter to get quarter as a text
CALCULATE(
    VALUES('calendar'[YYYYQ]),
    FILTER(
        all('calendar'),
        'calendar'[yyyyq val] = _maxq
    )
)
var _curr = SELECTEDVALUE('calendar'[YYYYQ],_mfilter) //getting quarter for each data context; iff missing taking maximum quarter calulated in previous variables
var _slicer = SELECTEDVALUE('calendar copy'[calendar_YYYYQ]) //getting selected quarter
var _rolling =
    CALCULATE(
        facts[facts],
        FILTER(
            ALL('calendar'[YYYYQ]),
            'calendar'[YYYYQ] <= _curr
            && 'calendar'[YYYYQ] <= _slicer
        )
    )
return
_rolling
```

## Remarks
* need to create first copy of calendar table ('calendar copy') __without any relationship__
```
calendar copy = SELECTCOLUMNS('calendar','calendar'[YYYYQ],'calendar'[yyyyq val])//cannot have an relation
```
* calendar_YYYYQ (e.g. '2021Q1'), whereas calendar_yyyq val contain values like 20211, which are integer
* can work in visuals broken by calendar_YYYYQ, but doesn't need to
