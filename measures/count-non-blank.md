# Count distinct ids only for these rows that measure is not blank or greater than 0


```
CALCULATE(
    DISTINCTCOUNT('Product'[Product Number]),
    FILTER(
        'Product',
        NOT(ISBLANK([Demand Qty])) &&
        [Demand Qty] > 0,
    )
```
