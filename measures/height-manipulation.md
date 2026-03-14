# Set of measures to adjust height or weight of charts

## Gives maximum value of category (maximum child value)
```
  ln max category = 
  MAXX(
      ALL(land_notes_i[issue]),
      [ln count]
  ) *1.19
```
