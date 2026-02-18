---

## 📓 Notebook — Data Transformation Pipeline

**File:** `notebooks/ultra_marathon_pipeline.ipynb`  
**Runtime:** PySpark 4.1.1 + Python 3.8+  
**Input:** `TWO_CENTURIES_OF_UM_RACES.csv` (raw Kaggle download, ~7.4M rows)  
**Output:** 10 processed files (5 dim tables + 5 agg tables) in `/data/processed/`

### What the notebook does

| Stage | Cell(s) | Description |
|-------|---------|-------------|
| **0 · Config** | 1 | Single `BASE_DIR` variable drives all file paths — change once, runs anywhere |
| **1 · Spark Session** | 1 | Initialises PySpark with 8 GB driver memory, 100 shuffle partitions |
| **2 · Ingest & Clean** | 1 | Loads CSV, normalises column names, strips BOM characters, parses dates / distances / performance times, validates age (1900–2005) and speed (0–25 km/h), filters invalid records |
| **3 · Deduplication** | 1 | Drops duplicate `(athlete_id, event_name)` pairs — removes ~2.5M rows |
| **4 · Filter** | 1 | Drops `athlete_gender = 'X'` (< 0.1 % of records), then drops rows missing `distance_km`, `avg_speed_kmh`, or `performance_seconds` |
| **5 · EDA** | 5 | Shared setup cell creates `dist_category`, `month`, and `athlete_tier` once; five charts (overview dashboard, elite vs average, growth boom, gender trend, peak age curve) |
| **6 · Star Schema** | 1 | Exports `dim_athletes`, `dim_events`, `dim_date`, `dim_distance`, `dim_gender`, `fact_races` |
| **7 · Pre-aggregations** | 1 | Exports `agg_yearly`, `agg_country`, `agg_seasonality`, `agg_age_performance`, `agg_elite_vs_avg` for Power BI |

### Key design decisions

- **`BASE_DIR` config block** — all 15 output paths are derived from one variable at the top of the notebook, eliminating hardcoded paths scattered across cells.
- **`bin_distance()` defined once** — in the original notebook this helper was copy-pasted into 4 different cells. It now lives in the EDA setup cell and is available everywhere.
- **`athlete_tier` computed centrally** — Elite classification (90th percentile per gender × distance, PRD §5) was previously recalculated in multiple cells with slightly different logic. Now computed once in the setup cell and reused by all EDA charts and the `agg_elite_vs_avg` export.
- **`.toPandas().to_parquet()` instead of `.write.parquet()`** — avoids Hadoop temp-file permission errors on Windows (documented in cell comments).
- **`sample_size > 10` filter on `agg_age_performance`** — removes statistically unreliable age buckets from the Peak Age Curve.

