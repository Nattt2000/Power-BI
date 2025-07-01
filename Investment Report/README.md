# Document Description

## Used Helper Tables

### Restricted Calendar

Created using:  
```DAX
dimCalendar = CALENDAR("1/1/2005","31/12/2015")
```

### Auto Calendar

Created using:  
```DAX
dimCALENDARAUTO = CALENDARAUTO()
```

### Full Calender with new columns

Created using:  
```DAX
dimDate = 
ADDCOLUMNS
(
    CALENDAR("1/1/2005","31/12/2015"),
    "Den v měsíci", DAY([Date]),
    "Den v týdnu", WEEKDAY([Date],2),
    "Den", FORMAT([Date],"DDDD","cs-CZ"),
    "Den číslo", INT([Date]),
    "Měsíc", FORMAT([Date], "MMMM", "cs-CZ"),
    "Měsíc číslo", MONTH([Date]),
    "Měsíc rok", FORMAT([Date], "mmmm","cs-CZ") & " " & YEAR([Date]),
    "Měsíc rok číslo", INT(YEAR([Date]) * 12 + MONTH([Date]) -1),
    "Čtvrtletí číslo", INT( FORMAT([Date],"q")),
    "Čtvrtletí", "Q" & INT(FORMAT([Date],"q")),
    "Čtvrtletí rok", "Q" & INT(FORMAT([Date],"q")) & "-" & YEAR([Date]),
    "Čtvrtletí rok číslo", INT(YEAR([Date])*4 + INT(FORMAT([Date],"q")) -1),
    "Rok",YEAR([Date]),   
    "DateKey", VALUE(FORMAT([Date],"YYYYMMDD"))  
```

### MIN MAX Calendar

Created using:  
```DAX
dimDateDyn = 
CALENDAR
(
    DATE(YEAR(MIN(Data[Datum])),1,1),
    DATE(YEAR(MAX(Data[Datum])),12,31)
)
```

### ISO Calendar

Created using:  
```DAX
ISO týdenní kalendář = 
VAR ISO_Rok_OD = 2015
VAR ISO_Rok_DO = 2020
VAR PrvniKrok = 
    ADDCOLUMNS
    (
        CALENDAR
        (
            DATE(ISO_Rok_OD - 1, 1, 1),
            DATE(ISO_Rok_DO + 1, 12, 31)
        ),
        "Den v týdnu číslo", WEEKDAY([Date], 2),
        "Den v týdnu", FORMAT([Date], "dddd", "cs-cz"),
        "Kalendářní rok",YEAR([Date]),
        "Kalendářní týden", WEEKNUM([Date], 2),
        "ISO týden číslo", WEEKNUM([Date], 21),
        "ISO týden pořadí", INT(([Date] + 5) / 7)    
    )
VAR DruhyKrok =
    ADDCOLUMNS
    (
        ADDCOLUMNS
        (
            PrvniKrok,
            "ISO čtvrtletí číslo",
            SWITCH
            (
                TRUE(),
                [ISO týden číslo] <= 13, 1,
                [ISO týden číslo] <= 26, 2,
                [ISO týden číslo] <= 39, 3,
                4
            )
        ),
        "ISO pořadí týdne ve čtvrtletí",
        SWITCH
        (
            TRUE(),
            [ISO čtvrtletí číslo] = 1, [ISO týden číslo],
            [ISO čtvrtletí číslo] = 2, [ISO týden číslo] - 13,
            [ISO čtvrtletí číslo] = 3, [ISO týden číslo] - 26,
            [ISO týden číslo] - 39
        )
    )
VAR TretiKrok = 
    ADDCOLUMNS
    (
        DruhyKrok,
        "ISO rok číslo",
        IF
        (
            [ISO týden číslo] < 5 && [Kalendářní týden] > 50,
            [Kalendářní rok] + 1,
            IF
            (
                [ISO týden číslo] > 50 && [Kalendářní týden] < 5,
                [Kalendářní rok] - 1,
                [Kalendářní rok]
            )
        )
    )
VAR CtvrtyKrok = 
    ADDCOLUMNS
    (
        TretiKrok,
        "ISO den v roce",
        VAR IsoRok = [ISO rok číslo]
        VAR Den = [Date]
        RETURN
        COUNTROWS
        (
            FILTER
            (
                TretiKrok,
                [ISO rok číslo] = IsoRok && [Date] <= Den
            )
        ),
        "ISO týden rok", "ISO T" & [ISO týden číslo]  & "-R" & [ISO rok číslo],
        "ISO čtvrtletí rok", "ISO Č" & [ISO čtvrtletí číslo] & "-R" & [ISO rok číslo],
        "ISO rok", "ISO R" & [ISO rok číslo],
        "ISO čtvrtletí pořadí", [ISO rok číslo] * 4 - 1 + [ISO čtvrtletí číslo]
        
    )
VAR PatyKrok = 
    ADDCOLUMNS
    (
        ADDCOLUMNS
        (
            CtvrtyKrok,
            "ISO den ve čtvrtletí",
            VAR IsoCtvrtletiPoradi = [ISO čtvrtletí pořadí]
            VAR Den = [Date]
            RETURN
            COUNTROWS
            (
                FILTER
                (
                    CtvrtyKrok,
                    [ISO čtvrtletí pořadí] = IsoCtvrtletiPoradi && [Date] <= Den
                )
            ),
            "ISO den v roce (do 364)",
            IF
            (
                [ISO den v roce] > 364,
                364,
                [ISO den v roce]
            )
        ),
        "ISO den ve čtvrtletí (do 91)",
        IF
        (
            [ISO den ve čtvrtletí] > 91,
            91,
            [ISO den ve čtvrtletí]
        )
    )
VAR Vysledek =
    FILTER
    (
        PatyKrok,
        [ISO rok číslo] >= ISO_Rok_OD && 
        [ISO rok číslo] <= ISO_Rok_DO
    )
RETURN
    Vysledek
```

### Test of parameter
#### parameter table created using Power BI What-if parameter

Created using:  
```DAX
Test parametru = GENERATESERIES(2005, 2015, 1)
```

### Test1
#### parameter table created using Power BI What-if parameter

Created using:  
```DAX
Test1 = GENERATESERIES(2005, 2010, 1)
```

### Test2
#### parameter table created using Power BI What-if parameter

Created using:  
```DAX
test2 = GENERATESERIES(2000, 2020, 1)
```
