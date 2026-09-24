# Removes empty lines

```
CALCULATE(
    DISTINCTCOUNT('Product'[Product Number]),
    FILTER(
        'Product',
        NOT(ISBLANK([Demand Qty])) &&
        [Demand Qty] > 0
    )
)
```
