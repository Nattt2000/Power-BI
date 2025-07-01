# Document Description

## Used Tables

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

### Test of parameter
#### parameter table created using Power BI What-if parameter

Created using:  
```DAX
Test parametru = GENERATESERIES(2005, 2015, 1)
```

## Used Main Measures
### Average value of investments

Created using:  
```DAX
a_inv = AVERAGE(Data[Investice])
```

### Count of rows

Created using:  
```DAX
c_rows = COUNTROWS(data)
```

### Count of clients

Created using:  
```DAX
dc_klient = DISTINCTCOUNT('Data'[Klient])
```

### Sum of investments in current year

Created using:  
```DAX
Investice (CY) = SUM(Data[Investice])
```

### Sum of investments in previous year

Created using:  
```DAX
Investice (PY) = 
CALCULATE
(
    [Investice (CY)],
    SAMEPERIODLASTYEAR('dimDate'[Date])
)
```

### Restricted sum of investments in current year
#### Only investments with date lower than MAX date

Created using:  
```DAX
Investice (RT) = 
CALCULATE
(
   [Investice (CY)],
    'dimDate'[Date] <= MAX('dimDate'[Date])
)
```
