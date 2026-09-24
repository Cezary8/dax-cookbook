# Dynamic count of incidents that are between 7 and 14 days old

```
INC 07-14 PY = 
//var _maxfact = 
//    CALCULATE(
//        MAX('Fact Service'[Created Date]),
//        ALL('Dim Calendar')
//    )
var _currentDate =
     CALCULATE(
         MAX('Dim Calendar'[Date]),
         FILTER(
             'Dim Calendar',
             'Dim Calendar'[Year] = [param Year Value] -1 &&
             'Dim Calendar'[Date] <= DATE([max year] - 1, [max month], [max day]) &&
             'Dim Calendar'[Fact service dates] = 1
         )
     )
var _result =
// IF(
//     _currentDate <= _maxfact,
    CALCULATE(
       COUNTROWS('Fact Service'),
       DATESINPERIOD(
            'Dim Calendar'[Date],
            _currentDate - 8,
            -7,
            DAY
        ),
        FILTER(
            ALL('Fact Service'[Resolved Date]),
            'Fact Service'[Resolved Date] > _currentDate || ISBLANK('Fact Service'[Resolved Date])
        ),
        'Fact Service'[Source] = "Incidents"
    )
//)
RETURN
_result

```
