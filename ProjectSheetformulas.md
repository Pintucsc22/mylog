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
Column: Time Schedule (I)
Formula:
```

=IF(A8="","", IF(H8="Break",
   TEXT($E$6 + SUMIF($A$7:$A$8,A8,$F$7:$F$8)/1440 + IF($E$6 + SUMIF($A$7:$A$8,A8,$F$7:$F$8)/1440 >= TIME(13,0,0), TIME(0,30,0), 0),"hh:mm")
   & " to " &
   TEXT($E$6 + (SUMIF($A$7:$A$8,A8,$F$7:$F$8)+F8)/1440 + IF($E$6 + (SUMIF($A$7:$A$8,A8,$F$7:$F$8)+F8)/1440 >= TIME(13,0,0), TIME(0,30,0), 0),"hh:mm"),
   
   TEXT($E$6 + (SUMPRODUCT(
       ($A$8:$A=A8) *
       (ISNUMBER($F$8:$F)) *
       (
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) < VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) +
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) = VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) * (ROW($H$8:$H) <= ROW())
       ) * $F$8:$F
   ) - F8)/1440 + IF($E$6 + (SUMPRODUCT(
       ($A$8:$A=A8) *
       (ISNUMBER($F$8:$F)) *
       (
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) < VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) +
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) = VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) * (ROW($H$8:$H) <= ROW())
       ) * $F$8:$F
   ) - F8)/1440 >= TIME(13,0,0), TIME(0,30,0), 0),"hh:mm")
   & " to " &
   TEXT($E$6 + SUMPRODUCT(
       ($A$8:$A=A8) *
       (ISNUMBER($F$8:$F)) *
       (
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) < VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) +
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) = VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) * (ROW($H$8:$H) <= ROW())
       ) * $F$8:$F
   )/1440 + IF($E$6 + SUMPRODUCT(
       ($A$8:$A=A8) *
       (ISNUMBER($F$8:$F)) *
       (
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) < VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) +
         (VALUE(IFERROR(REGEXEXTRACT($H$8:$H,"(\d+)"),0)) = VALUE(IFERROR(REGEXEXTRACT(H8,"(\d+)"),0))) * (ROW($H$8:$H) <= ROW())
       ) * $F$8:$F
   )/1440 >= TIME(13,0,0), TIME(0,30,0), 0),"hh:mm")
))


```

---

