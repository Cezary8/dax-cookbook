# Cumulative sum with replaced 0 and other conditions

```
VAR CalendarDateFilter =
    IF(
        IF(
            MIN( Calendar[Date] ) > TODAY( ),
            BLANK( ),
            CALCULATE(
                SUM( Inventory[InventoryQty] ),
                // (ALL() inserted to assures, that all transaction until that specific choosen date is considered.
                FILTER( ALL( Calendar ), Calendar[Date] <= MAX( Calendar[Date] ) )
            )
        )
            = 0
            && MAX( Calendar[Date] ) <> ( EOMONTH( MAX( Calendar[Date] ), 0 ) ),
        BLANK( ),
        IF(
            MIN( Calendar[Date] ) > TODAY( ),
            BLANK( ),
            CALCULATE(
                SUM( Inventory[InventoryQty] ),
                // (ALL() inserted to assures, that all transaction until that specific choosen date is considered.
                FILTER( ALL( Calendar ), Calendar[Date] <= MAX( Calendar[Date] ) )
            )
        )
    )
VAR NoCalendarDateFilter =
    IF(
        CALCULATE( SUM( Inventory[InventoryQty] ), FILTER( ALL( Calendar ), Calendar[Date] <= TODAY( ) ) ) = 0,
        BLANK( ),
        CALCULATE( SUM( Inventory[InventoryQty] ), FILTER( ALL( Calendar ), Calendar[Date] <= TODAY( ) ) )
    )
RETURN
    IF(
        CALCULATE( IF( ISFILTERED( Calendar ), CalendarDateFilter, NoCalendarDateFilter ) ) = 0,
        BLANK( ),
        CALCULATE( IF( ISFILTERED( Calendar ), CalendarDateFilter, NoCalendarDateFilter ) )
    )
```

## Description
**zmienna CalendarDateFilter**
Co do zasady wyświetlamy narastającą sumę Inventory[InventoryQty] od samego początku, ale stosujemy kilka modyfikacji:
* Pomijamy przypadki gdzie początek okresu jest w przyszłości (wtedy blank())
* Jeśli ta narastająca suma o której pisałem na początku = 0 i jednocześnie koniec przeglądanego okresu nie jest jednocześnie ostatnim dniem miesiąca to jeśli oba te warunki są spełnione wyświetlamy blank() zamiast 0

*ACHTUNG:  Inventory[InventoryQty] może być ujemne albo dodatnie, dlatego wraz z postepem czasu te wartosci niekoniecznie muszą rosnąć
 
 
**zmienna NoCalendarDateFilter** 
Co do zasady wyświetlamy narastającą sumę Inventory[InventoryQty] od samego początku, ale stosujemy kilka modyfikacji:
* Jeśli narastająca suma o której pisałem na początku = 0 to wyświetlamy blank() zamiast 0
 
 
**Ostateczne działanie:**
Jeśli ktróakolwiek kolumna z tabeli Calendar jest użyta (a w Power Query do któego zaciągamy dane jest użyta) to użyj CalendarDateFilter, w przeciwnym razie użyj NoCalendarDateFilter
W powyższym wyniku 0 zastąp blankiem()
