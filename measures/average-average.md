# Average based on Average

```
IF(
    ISFILTERED( 'Product - Finished Product' ) || ISFILTERED('Production Order'),
    AVERAGEX( 'Product - Finished Product', [AverageTotalFinishedProductArea]),
    AVERAGEX( 'Product - Sub Component', [AverageTotalFinishedProductArea])
)
```
