# Google Sheets Formulas

## Formula 1 - Task %
### Column: Task Priorty Value Helper Column(AL)
Formula:
```

=IF(A8="","",VLOOKUP(E8,$AB$1:$AC$6,2,0)*F8)

```
### Column: Task %(G)
Formula:
```

=IF(AL8="","", AL8 / SUMIFS($AL$8:$AL, $A$8:$A, A8))

```

## Formula 2 - Time Schedule

Helper Columns Added

- AM: TotalMinutes (SUMIF per group)
- AN: CumulativeMinutes (sum up to current step)
- AO: StepNumber (numeric step from H)
- I: Time Schedule (final "hh:mm to hh:mm")
### Column: TotalMinutes (AM)
Formula:
```
=IF(A8="", "", SUMIF($A$8:$A, A8, $F$8:$F))
```
### Column: CumulativeMinutes (AN)
Formula:
```
=IF(A8="","",
 SUM(
   FILTER(
     $F$8:$F$500,
     ($A$8:$A$500=A8) * 
     (
       (AO$8:AO$500>0) * (AO$8:AO$500<=AO8) + 
       (AO$8:AO$500=0) * (ROW($A$8:$A$500)<=ROW())
     )
   )
 )
)

```
### Column: StepNumber (AO)
Formula:
```
=IF(H8="", 0, VALUE(REGEXEXTRACT(H8, "\d+")))
```
### Column: Time Schedule (I)
Formula:
```

=IF(A8="","",
 IF(H8="Break",
   TEXT($E$6 + AN8/1440 + IF($E$6 + AN8/1440 >= TIME(13,0,0), TIME(0,30,0), 0),"hh:mm")
   & " to " &
   TEXT($E$6 + (AN8+F8)/1440 + IF($E$6 + (AN8+F8)/1440 >= TIME(13,0,0), TIME(0,30,0), 0),"hh:mm"),

   LET(
     startTime, $E$6 + (AN8-F8)/1440,
     endTime,   $E$6 + AN8/1440,
     adjStart, IF(startTime >= TIME(13,0,0), startTime + TIME(0,30,0), startTime),
     adjEnd,   IF(endTime   >= TIME(13,0,0), endTime   + TIME(0,30,0), endTime),
     TEXT(adjStart,"hh:mm") & " to " & TEXT(adjEnd,"hh:mm")
   )
 )
)


```
## Formula 3 - Task %
### Column: B4
Formula:
```

=IF(
  COUNTIFS(A8:A, TODAY(), J8:J, {"Completed","Completed_Waiting"})=0,
  "0%",
  TEXT(
    SUMIFS(G8:G, A8:A, TODAY(), J8:J, "Completed") +
    SUMIFS(G8:G, A8:A, TODAY(), J8:J, "Completed_Waiting") * 0.8,
    "0%"
  )
)


```
---

