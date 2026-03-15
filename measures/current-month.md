# Current month filtering persist
Idea is to force report keep always current month selected

It requires adding calculated column to calendar table.
Adjusting variable refresh_dt may be needed

```
CurrentMonth = 
var refresh_dt = FORMAT(DATE(2021,7,1), "mmm-yy")
var check = FORMAT('d calendar'[date], "mmm-yy") = refresh_dt
return
IF(
    check,
    "Current Month ✔",
    FORMAT('d calendar'[date], "mmm" )
)
```
