# Data bar based on unicode

```
Data bar area prelet = 
var _nr_icons_total = 12
var _perc = [Area Pre-Let]/[Area]
var _nr_icons_fill = _nr_icons_total * _perc
var _nr_icons_empty = _nr_icons_total - _nr_icons_fill
var _icon_fill = "█"
var _icon_empty = "░"

var _data_bar = 
    REPT(_icon_fill, _nr_icons_fill) &
    REPT(_icon_empty, _nr_icons_empty)
RETURN
_data_bar
```
