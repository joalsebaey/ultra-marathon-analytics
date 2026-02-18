# Data Dictionary

This document describes every column in every processed table used in the Power BI dashboard.

---

## 📁 Dimension Tables

### `dim_gender.csv`

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `athlete_gender` | STRING | Single-character gender code | `M` |
| `gender_full_name` | STRING | Full gender label | `Male` |

---

### `dim_distance.csv`

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `distance_km` | FLOAT | Standardized distance in kilometers | `50.0` |
| `distance_name` | STRING | Human-readable distance label | `50km` |
| `distance_category` | STRING | Category grouping (Short/Long Ultra) | `Short Ultra` |
| `distance_type` | STRING | Whether this is a standard race distance | `Standard` |

---

### `dim_athletes.csv`

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `athlete_id` | INTEGER | Unique athlete identifier | `1023` |
| `athlete_gender` | STRING | Gender code (M/F) | `M` |
| `athlete_country` | STRING | ISO country code (uppercase) | `FRA` |
| `birth_year` | FLOAT | Athlete's year of birth | `1985.0` |
| `athlete_club` | STRING | Club affiliation (nullable) | `Trail Runners Paris` |

---

### `dim_events.csv`

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `event_name` | STRING | Official race name | `UTMB (FRA)` |
| `year_of_event` | INTEGER | Year the event took place | `2019` |
| `event_start_date` | DATE | Exact start date of the event | `2019-08-30` |
| `distance_km` | FLOAT | Distance of this event in km | `100.0` |
| `distance_original` | STRING | Original distance string from source | `100km` |
| `distance_unit` | STRING | Unit of measurement | `km` |
| `event_number_of_finishers` | INTEGER | Total number of finishers in this event | `2500` |

---

### `dim_date.csv`

Date dimension table for time-intelligence DAX calculations.

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `Date` | DATE | Full date | `2019-08-30` |
| `Year` | INTEGER | Calendar year | `2019` |
| `Month` | INTEGER | Month number (1–12) | `8` |
| `MonthName` | STRING | Month name | `August` |
| `Quarter` | INTEGER | Calendar quarter (1–4) | `3` |
| `DayOfWeek` | STRING | Name of weekday | `Friday` |

---

## 📁 Aggregation Tables

### `agg_yearly.csv`

Annual aggregated performance metrics by year, gender, and distance.

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `year_of_event` | INTEGER | Race year | `2015` |
| `athlete_gender` | STRING | Gender group | `F` |
| `distance_km` | FLOAT | Race distance | `50.0` |
| `total_finishers` | INTEGER | Count of finishers | `45230` |
| `total_races` | INTEGER | Number of distinct races | `312` |
| `avg_speed_kmh` | FLOAT | Average speed in km/h | `6.72` |
| `avg_age` | FLOAT | Average age of finishers | `41.3` |

---

### `agg_country.csv`

Participation totals by country and year.

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `athlete_country` | STRING | ISO country code | `FRA` |
| `year_of_event` | INTEGER | Race year | `2018` |
| `total_finishers` | INTEGER | Count of finishers from this country | `98340` |

---

### `agg_seasonality.csv`

Race volume by month and distance for seasonality analysis.

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `month` | INTEGER | Month number (1–12) | `6` |
| `distance_km` | FLOAT | Race distance | `100.0` |
| `race_count` | INTEGER | Number of races in that month | `1240` |
| `total_finishers` | INTEGER | Total finishers in that month | `407000` |

---

### `agg_age_performance.csv`

Speed by age group and gender to identify the endurance performance peak.

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `athlete_age` | FLOAT | Athlete age (integer value) | `38.0` |
| `athlete_gender` | STRING | Gender group | `M` |
| `distance_km` | FLOAT | Race distance | `50.0` |
| `avg_speed_kmh` | FLOAT | Average speed for this age group | `7.21` |
| `sample_size` | INTEGER | Number of records in this group | `12400` |

---

### `agg_elite_vs_avg.csv`

Speed comparison between Elite (top 10%) and Average runners across distances and genders.

| Column | Type | Description | Example |
|--------|------|-------------|--------|
| `distance_km` | FLOAT | Race distance | `160.934` |
| `athlete_gender` | STRING | Gender group | `M` |
| `athlete_tier` | STRING | Performance tier (`Elite` or `Average`) | `Elite` |
| `avg_speed_kmh` | FLOAT | Average speed for this tier | `11.45` |
| `athlete_count` | INTEGER | Count of athletes in this group | `8900` |
