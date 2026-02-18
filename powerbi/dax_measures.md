# DAX Measures Documentation

All DAX measures used in the Ultra-Marathon Analytics Dashboard.

---

## 📊 Core KPI Measures

### Total Finishers
```dax
Total Finishers = SUM(agg_yearly[total_finishers])
```
Sums all finishers across all selected filters (year, gender, distance).

---

### Total Races
```dax
Total Races = SUM(agg_yearly[total_races])
```
Sums all distinct races across the filtered context.

---

### Avg Speed
```dax
Avg Speed = 
DIVIDE(
    SUMX(agg_yearly, agg_yearly[avg_speed_kmh] * agg_yearly[total_finishers]),
    SUM(agg_yearly[total_finishers])
)
```
Weighted average speed (weighted by finisher count to avoid bias toward small races).

---

### Avg Age
```dax
Avg Age = 
DIVIDE(
    SUMX(agg_yearly, agg_yearly[avg_age] * agg_yearly[total_finishers]),
    SUM(agg_yearly[total_finishers])
)
```
Weighted average age across all finishers in the current filter context.

---

## 👥 Gender Measures

### Female Count
```dax
Female Count = 
CALCULATE(
    SUM(agg_yearly[total_finishers]),
    agg_yearly[athlete_gender] = "F"
)
```

---

### Male Count
```dax
Male Count = 
CALCULATE(
    SUM(agg_yearly[total_finishers]),
    agg_yearly[athlete_gender] = "M"
)
```

---

### Female %
```dax
Female % = 
DIVIDE(
    [Female Count],
    [Total Finishers],
    0
)
```
Returns the proportion of female finishers. Formatted as percentage in visuals.

---

## ⚡ Performance Measures

### Elite Speed
```dax
Elite Speed = 
CALCULATE(
    AVERAGE(agg_elite_vs_avg[avg_speed_kmh]),
    agg_elite_vs_avg[athlete_tier] = "Elite"
)
```
Average speed for Elite-tier runners (top 10% by gender-specific speed threshold).

---

### Average Speed Tier
```dax
Average Speed Tier = 
CALCULATE(
    AVERAGE(agg_elite_vs_avg[avg_speed_kmh]),
    agg_elite_vs_avg[athlete_tier] = "Average"
)
```
Average speed for Average-tier runners.

---

### Peak Age Speed
```dax
Peak Age Speed = 
CALCULATE(
    AVERAGE(agg_age_performance[avg_speed_kmh]),
    agg_age_performance[athlete_age] >= 35,
    agg_age_performance[athlete_age] <= 45
)
```
Average speed for athletes in the 35–45 age "endurance peak" band.

---

### Pace Decay %
```dax
Pace Decay % = 
VAR Speed50km =
    CALCULATE(
        AVERAGE(agg_elite_vs_avg[avg_speed_kmh]),
        agg_elite_vs_avg[distance_km] = 50
    )
VAR Speed100mi =
    CALCULATE(
        AVERAGE(agg_elite_vs_avg[avg_speed_kmh]),
        agg_elite_vs_avg[distance_km] = 160.934
    )
RETURN
    DIVIDE(Speed100mi - Speed50km, Speed50km, 0)
```
Expresses the speed drop from the shortest to longest distance as a percentage (negative value = decay).

---

## 📊 Display & Formatting Measures

### Speed Display Label
```dax
Speed Display Label = FORMAT([Avg Speed], "0.00") & " km/h"
```

---

### Age Display Label  
```dax
Age Display Label = FORMAT([Avg Age], "0.0") & " yrs"
```

---

> **Note:** All measures live in the `_measures` table inside the Power BI data model to keep them organized and separate from dimension/fact tables.
