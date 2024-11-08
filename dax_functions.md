## DAX Functions
To enhance data analysis and derive meaningful insights, I wrote the following DAX functions to create measures and calculated columns tailored to the project’s needs.
# Base Metrics: This includes the following;
Total Revenue =
   SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] * RELATED('Product Lookup'[ProductPrice])
   )

 Total Cost = 
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] * RELATED('Product Lookup'[ProductCost])
)

  Total Profit =
[Total Revenue] - [Total Cost]


 Total Orders = 
DISTINCTCOUNT(
    'Sales Data'[OrderNumber]
)


 Total Returns = 
COUNT(
    'Returns Data'[ReturnQuantity]
)

 Total Returned Quantity = 
SUM(
    'Returns Data'[ReturnQuantity]
)

 Total Customers = 
DISTINCTCOUNT('Sales Data'[CustomerKey]
)

 Average Revenue Per Customer = 
DIVIDE(
    [Total Revenue],
    [Total Customers]
)

 Return Rate = 
DIVIDE( 
    [Total Returned Quantity], [Total Quantity Sold]
)

 Average Retail price = 
AVERAGE(
    'Product Lookup'[ProductPrice]
)

 # Time Intelligence Measures
 Previous Month Revenue = 
CALCULATE(
    [Total Revenue],
    PARALLELPERIOD('Calendar Lookup'[Date],
    -1,
    MONTH)
)

 Previous Month Profit = 
CALCULATE(
    [Total Profit],
    PARALLELPERIOD( 'Calendar Lookup'[Date],
    -1,
    MONTH)
)

 Previous Month Orders = 
CALCULATE(
    [Total Orders],
    DATEADD('Calendar Lookup'[Date],
    -1,
    MONTH)
)

 Previous Month Returns = 
CALCULATE(
    [Total Returns],
    PARALLELPERIOD('Calendar Lookup'[Date],
    -1,
    MONTH)
)

 Revenue Target = 
[Previous Month Revenue] * 1.1


 Profit Target = 
[Previous Month Profit] * 1.1


 Order target = 
[Previous Month Orders] * 1.1


# Calculated Columns
 Day Of Week = 
WEEKDAY(
    'Calendar Lookup'[Date],
    2
)

 Day type = 
IF(
    'Calendar Lookup'[Day Of Week] IN {6,7} ,
    "Weekend",
    "Weekday"
)

 Price Point = 
SWITCH(
    TRUE(),
    'Product Lookup'[ProductPrice] > 500, "High",
    'Product Lookup'[ProductPrice] > 100, "Mid-Range",
    "Low"
)

 Quantity Type = 
IF(
    'Sales Data'[OrderQuantity] > 1,
     "Multiple Item",
     "Single Item"
  )

