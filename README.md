# Newark-Yard-Check

## Goal

As the main customer for the Newburgh, NY Terminal yard checks are done by our drivers several times a day in order to ensure the Newark, NJ Brewery is running on schedule and there is no risk of shut downs. As these checks are done multiple times a day, often by non-technical employees, it can be rather time consuming to sort and compute the information. This sheet will aim to streamline that process.

## Finished Sheet

<a ref="https://docs.google.com/spreadsheets/d/1uK5SkBqNS1JtcgBdN_emZWEh6T3XF4gg034D8EL2WAM/">Newark Yard Check</a>

## Background Information

The Newark, NJ Brewery has two can production lines. These are labelled Line 85 and Line 65. Each trailer of empty cans is enough to keep the lines running for two hours at a normal production rate. This sheet needs to correctly compute the number or trailers for each line, use this information to compute the hours of product for each line, sort the yard check and format the yard check visually.

## Explanation of Mechanics

Main Query
```
=IFERROR(QUERY({IFERROR(ARRAYFORMULA(QUERY(TO_TEXT(C5:E19),"SELECT Col1,Col2, UPPER(Col3),"&E22&" WHERE (Col1 IS NOT NULL) AND (Col3 LIKE '%65%')",0)),{"","","",""});
IFERROR(ARRAYFORMULA(QUERY(TO_TEXT(C5:E19),"SELECT Col1,Col2,Col3,"&E23&" WHERE (Col1 IS NOT NULL) AND (Col3 LIKE '%85%')",0)),{"","","",""});
IFERROR(ARRAYFORMULA(QUERY(TO_TEXT(C5:E19),"SELECT Col1,Col2,Col3,' ' WHERE (Col1 IS NOT NULL) AND (NOT Col3 LIKE '%65%') AND (NOT Col3 LIKE '%85%') ORDER BY Col3",0)),{"","","",""})}
, "SELECT * WHERE Col1 IS NOT NULL",0),{"Failed","to","pull","results"})
```

This is the main formula which serves to sort the pull the data and sorts the data.

This is a nested SQL query so the outer query runs a query on the three inner queries. This simple combines all three queries.

The first query pulls all trailers with 65 in the product cell. It also adds the product per hour as a column.
The second query pulls all trailers with 85 in the product cell. It also adds the product per hour as a column.
The third query pulls all remaining cells and sorts them by product alphabetically. These are normally Dunnages or Empty trailers.

The error handling is to ensure that the queries always return an array of 1x4 cells.

# Number of trailers for each line
```
=IFERROR(QUERY(K5:K18,"Select COUNT(K) WHERE K LIKE '%65%' LABEL COUNT(K) ''",0),0)
```

This is simply a query to count all cells (trailers) with 65 in the product line. If there are no results then it should return 0 instead of an error. Determining the hours of product per lines is as simple as multiplying the number or trailers by the hours of product per trailer.

----

This is a very simple sheet but it saves about 15 minutes a day due to removing the requirement to manually sort the sheet and count the number of trailers.
