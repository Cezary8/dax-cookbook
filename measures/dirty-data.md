# Dirty data dashboard concept
Idea of report that tests error based on defined in SQL script criteria and displays in report data ponts requiring attention
Context: development_pipeline_b (one side) table with data that are being tested and development_pipeline_i table with (many side) with error flag and description what type of issue

## Measure used in conditional formatting
 ```
fdp completion (practical) = 
var __field =  "practical_dt"
var __scale = 
        CALCULATE(
            SELECTEDVALUE(development_pipeline_i[scale]),
            development_pipeline_i[field] = __field,
            all(fields[caption])
        )
var __hex = 
    SWITCH(TRUE(),
    __scale= "missing", [hex missing],
    __scale= "suspicious", [hex suspicious],
    __scale= "highly suspicious", [hex highly suspicious]
)
return
__hex
 ```

