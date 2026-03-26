# Filtering within measure
Idea is filter data between two tables without any relationship between them.
Filtering is being processed within measure

## Calculation

```
!Baseline Project SBA = --zwięzłe filtrowanie wewnątrz miary (bez relacji)
CALCULATE(
    MAX(Project[Project Strategic Business Area]),
    Project[Project Current or History] = "Current",
    Project[Project Name Current] = MAX(Baseline[Baseline Project Name])
)
```
