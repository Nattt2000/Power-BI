# Document Description

My first PBI document based on test data from the application.  
The first chart shows the amount of earnings by calendar month, while the second image displays earnings by country on a map.  
The bottom chart shows sales amounts for individual products. Each product is color-coded in bar charts representing the companies that manufacture it.  
On the left, you can select a specific month of interest, and the charts will adjust to show data only for the selected period.

## Used Helper Tables

### Calendar

Created using:  
```DAX
Calendar = CALENDAR(DATE(2013,01,01), DATE(2014,12,31))
```
