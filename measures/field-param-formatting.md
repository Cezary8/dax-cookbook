# Formatting items of Field parameter table
Goal is to assign appropiate colours to particular items.
The point is items changes depanding on picking different field parameter

## Definition
```
Selected colour = 
var _field = SELECTEDVALUE(switch_map[switch_map Fields])
var _item = 
    SWITCH(_field,
    "'facts'[Status]", SELECTEDVALUE(facts[Status]),
    "'facts'[Size Bracket]",SELECTEDVALUE(facts[Size Bracket]),
    "'facts'[Category (Max)]", SELECTEDVALUE(facts[Category (Max)]),
    "'facts'[Time at previous location band]", SELECTEDVALUE(facts[Time at previous location band]),
    "'facts'[Year (lease event)]", SELECTEDVALUE('facts'[Year (lease event)])
    )
RETURN
CALCULATE(
    SELECTEDVALUE(t_map_colour[Colour]),
    FILTER(
        t_map_colour,
        _item = t_map_colour[Item]
    )
)
```

## Remarks
* There is Field parameter table created to let end user choose between different dimension to display on a slicer.
  ```
  switch_map = {
    ("Status", NAMEOF('facts'[Status]), 0),
    ("Area bracket", NAMEOF('facts'[Size Bracket]), 1),
    ("Area change", NAMEOF('facts'[Category (Max)]), 2),
    ("Time at location", NAMEOF('facts'[Time at previous location band]), 3),
    ("Year", NAMEOF('facts'[Year (lease event)]), 5)
    }
  ```
* Output is presented on Button Slicer, which fortunetaly behaves like simple table in terms of displaying items of chosen field.
* Thanks to accent bar on Button Slicer it should behave as a map's legend as well
* Root source of colour in accent bar can be found in table t_map_colour, that consists of columns: KPI, Item, Colour (HEX code inside)
