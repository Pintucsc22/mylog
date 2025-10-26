# Google Sheets Formulas

## Formula 1 - Weighted Value
Column: Task %(G)
Formula:
```

=IF(OR(A8="", E8="", F8=""), "",
(IF(E8="Link-High", 25,
IF(E8="High", 20,
IF(E8="Link-Medium", 20,
IF(E8="Medium", 15,
IF(E8="Link-Low", 12,
IF(E8="Low", 8, 0)))))) * F8) /
SUMPRODUCT(
(A$8:A=$A8) *
(
(E$8:E="Link-High") * 25 +
(E$8:E="High") * 20 +
(E$8:E="Link-Medium") * 20 +
(E$8:E="Medium") * 15 +
(E$8:E="Link-Low") * 12 +
(E$8:E="Low") * 8
) *
F$8:F
)

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

