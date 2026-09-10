# Big Data Assignment — PySpark Analysis

Seven PySpark exercises over four public datasets: bakery transactions, Durham County restaurant inspections, Durham foreclosures, and world population 1980–2010.

`PySpark` `Spark SQL` `Window functions` `Jupyter` `Python 3`

Coursework — big data course, Assignment 2, run October 2025.

---

## What it does

One notebook builds a `SparkSession`, loads two CSVs and two JSON files, and answers seven questions with DataFrame transformations, UDFs, window functions, and pivots. Every problem prints its result table inline; the notebook is committed with outputs, so the answers are readable without a Spark cluster.

| Dataset | Format | Rows |
|---|---|---|
| `bakery.csv` | CSV | 21,293 |
| `Restaurants_in_Durham_County_NC.json` | nested JSON | 2,463 |
| `durham-nc-foreclosure-2006-2016.json` | nested JSON | 1,948 |
| `populationbycountry19802010millions.csv` | wide CSV, one column per year | 232 |

## The seven problems

| # | Question | Technique |
|---|---|---|
| 1 | Top 10 weekday dates by estimated revenue | UDF price lookup, `date_format`, distinct-transaction count |
| 2 | Highest-capacity moderate-risk restaurant zones | latitude bucketing into North/Central/South, groupBy |
| 3 | Top-selling item per hour, plus an item × hour pivot | `Window.partitionBy(hour)`, `pivot` |
| 4 | Foreclosures by parcel area code and crisis period | year bucketing, pivot, `total >= 50` filter |
| 5 | Most frequent weekday item pairs by daypart | `collect_list`, `itertools.combinations`, ranked window |
| 6 | ZIP codes containing both low- and high-risk restaurants | risk-category UDF, pivot, self-intersection |
| 7 (extra credit) | Population interpolation and regional growth | `stack` unpivot, window interpolation, re-pivot by decade |

## Selected results

- Best weekday: 2016-10-31, 96 transactions, $684.00 estimated revenue.
- Central zone full-service restaurants dominate capacity — 92 establishments, 13,179 seats, against 84 and 10,491 in the South.
- Coffee is the top-selling item in every business hour from 07:00 to 21:00, peaking at 946 sales in the 11:00 hour.
- Foreclosure area code 112 has 147 total, but 97 of them fall in 2006–2009. Area 159 has 142 total with 72 in 2014–2016 — two neighbourhoods with opposite trajectories through the crisis.
- Breakfast's most common pair is Coffee + Pastry, 158 co-purchases.
- ZIP 27707 has the most inspected restaurants at 202, split 85 high-risk / 35 medium / 82 low.

## The interesting part: pivots as the answer shape

Four of the seven problems end in a pivot, and that is the design decision worth defending. Problem 4 could have returned one row per area-code-and-year-bucket, but bucketing into `2006-2009`, `2010-2013`, `2014-2016` as *columns* is what makes the crisis visible — area 112 and area 159 have nearly identical totals and read as identical until the years become columns. Problem 3 does the same for time of day, and Problem 6 for risk category, where the interesting set is ZIP codes that contain *both* extremes, which is a condition you can only filter on once the categories sit side by side.

Problem 5 is the one that resists Spark's DataFrame API: pairwise co-purchase counts need `itertools.combinations` over a `collect_list` per transaction, then an exploded regroup, because there is no built-in for self-cross-joining an array against itself.

## Running it

```bash
jupyter notebook "Big Data Assignment.ipynb"
```

Requires PySpark. The datasets are not in the repo; the notebook reads them from `/home/jovyan/shared/data/`, the shared path of the course's Jupyter/Spark image. Change `SHARED` in cell 2 to point at your own copies.

## Repository map

| Path | Purpose |
|---|---|
| `Big Data Assignment.ipynb` | All seven problems, with stored outputs |

## Limitations

- Item prices in Problem 1 are invented by a UDF (premium $5, standard $3, budget $2, everything else $2.50) — the bakery dataset has no price column, so "revenue" is an estimate against a made-up price list.
- Zone assignment in Problem 2 uses raw latitude cut points, not real administrative boundaries.
- The population CSV's first column is unnamed, which Spark flags as a header mismatch; the notebook renames it positionally rather than fixing the source.
- Problem 7's growth rate is a per-region average, so every country in a region shows the same value — country-level growth is computed but not surfaced in the final table.
