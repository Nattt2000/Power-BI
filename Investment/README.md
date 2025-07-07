# Document Description

My Power BI project is based on data provided during a course. The report works with investment data from various companies. We can see the visualizations in line charts, bar charts and clear comparisons in matrices. In this project, I created several new tables, some of which are using Power BI parameters. I also created some new measures using DAX functions for calculations, analysis and dynamic headlines.

## Example of used tables 

### Restricted Calendar

Created using:  
```DAX
dimDate = CALENDAR("1/1/2005","31/12/2015")
```

### Test of parameter
#### parameter table with if parameter

Created using:  
```DAX
Test1 = GENERATESERIES(2000, 2020, 1)
```


## Example of used measures

### Restricted sum of investments in current year
#### Only investments with date lower than MAX date, Only if not blank

Created using:  
```DAX
Investice (RT) 2 = IF
(
    NOT ISBLANK([investice (CY)]),
    CALCULATE
    (
        [investice (CY)],
        'dimDate'[Date] <= MAX(dimDate[Date])
    ),
    BLANK()
)
```
