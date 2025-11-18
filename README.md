### Big Data Assignment – Spark Data Analysis with PySpark
---

### Overview

In this assignment, I used **Apache Spark (PySpark)** to perform scalable data analysis on multiple real-world datasets.  
You will practice:

- **Data loading and cleaning** from CSV and JSON sources  
- **Spark DataFrame transformations** (select, filter, withColumn, groupBy, pivot, joins)  
- **Spark SQL functions** for dates, times, aggregations, and user-defined logic  
- **Exploratory analysis** of sales, restaurant risks, foreclosures, and population growth  

All work was  completed in a **Jupyter notebook** using **PySpark**.

---

### Datasets

You are provided with the following datasets (paths may vary based on your environment):

- **Bakery Transactions**: `bakery.csv`  
  - Transaction-level sales data, including date, time, transaction ID, and item.  
- **Restaurant Inspection Data (Durham County, NC)**: `Restaurants_in_Durham_County_NC.json`  
  - Includes establishment type, status, seating capacity, inspection risk, and geolocation.  
- **Durham County Foreclosures**: `durham-nc-foreclosure-2006-2016.json`  
  - Contains parcel numbers, geocodes, and foreclosure years.  
- **Global Population by Country (1980–2010)**: `populationbycountry19802010millions.csv`  
  - Country-level population counts by year.

---

### Learning Objectives

By completing this assignment, I was able to:

- Build and configure a **SparkSession** in PySpark.  
- Read **CSV** and **JSON** data into Spark DataFrames.  
- Apply **UDFs**, date/time functions, and window functions.  
- Construct **aggregated summaries**, **pivots**, and **ranked outputs**.  
- Derive business and policy insights from large datasets.

---

### Tasks

#### Problem 1 – Weekday Revenue Estimation (Bakery)

Using the **bakery transactions** dataset:

- Parse the `Date` field and identify **weekdays** (Monday–Friday).  
- Define **price categories** (e.g., premium, standard, budget) and assign prices to items via a **UDF**.  
- For weekdays only, compute for each date:
  - Total number of **distinct transactions**  
  - **Estimated daily revenue** (sum of assigned item prices)  
- Produce a table of the **top 10 weekday dates by estimated revenue**, including:
  - `Date`, `Weekday`, `Total_Transactions`, `Estimated_Revenue`

---

#### Problem 2 – High-Capacity, Moderate-Risk Restaurant Zones

Using the **Durham restaurant inspection** dataset:

- Filter to **ACTIVE** establishments in the **Food Service** area.  
- Keep restaurants with **seating capacity ≥ 20** and **moderate risk levels** (e.g., risk 3 or 4 as defined in the data).  
- Use **geolocation (latitude)** to assign each restaurant to a **zone**:
  - **North**, **Central**, or **South** based on latitude ranges.  
- Group by `zone` and `establishment_type` to compute:
  - Number of restaurants  
  - Total seating capacity  
- Output the **top 15 zone–establishment combinations**, ordered by restaurant count and total seats.

---

#### Problem 3 – Top-Selling Item per Hour (Bakery)

Using the **bakery** dataset:

- Convert the `Time` column to a proper timestamp and extract the **hour of day**.  
- Restrict to business hours **(e.g., 6–21)** and exclude invalid rows.  
- Define a **time-of-day category** (Morning, Afternoon, Evening) based on hour.  
- For each hour:
  - Count how many times each item appears.  
  - Use a **window function** to find the **top-selling item per hour** (hours 7–21).  
- Create a **pivot table** where:
  - Rows = `Item`  
  - Columns = hours 7–21  
  - Cells = count of occurrences  
  - Add a `Total` column and sort by total count.

---

#### Problem 4 – Foreclosure Trends by Area and Crisis Period

Using the **foreclosure** dataset:

- Extract the **year** from the foreclosure date field.  
- Bucket years into ranges:
  - `2006–2009`, `2010–2013`, `2014–2016`  
- Derive an **area code** from the parcel number (e.g., first 3 characters).  
- For each area code, compute:
  - Foreclosure counts per year bucket  
  - Total number of foreclosures  
- Build a **pivoted summary** with columns for each year bucket and filter to area codes with **≥ 50 total foreclosures**.  
- Display the **top 10 area codes** ordered by total foreclosures.

---

#### Problem 5 – Weekday Co-Purchase Patterns (Bakery)

Using the **bakery** dataset:

- Focus on **weekday transactions** (Monday–Friday).  
- Define **day parts** based on hour (`Breakfast`, `Lunch`, `Dinner`).  
- Group by `date`, `Transaction`, and `day_part` to collect all items into a list.  
- Restrict to **multi-item transactions** (more than one item).  
- For each transaction, generate all **unique unordered item pairs** using combinations.  
- For each `day_part`, count how many times each item pair appears.  
- Using window functions, find the **top 5 most frequent item pairs per day part** and present them, including their pair counts.

---

#### Problem 6 – Restaurant Risk Patterns by ZIP Code

Using the **restaurant** dataset:

- Filter to **ACTIVE** Food Service establishments with valid **geolocation**.  
- Create a **risk category** UDF to map numeric risk levels to labels:
  - e.g., `No Risk`, `Low Risk`, `Medium Risk`, `High Risk`  
- For each ZIP code, count restaurants by risk category.  
- Identify ZIP codes that have **both Low Risk and High Risk** establishments.  
- Build and display a **pivot table**:
  - Rows = ZIP codes  
  - Columns = risk categories  
  - Cells = restaurant counts, plus a `total` column.

---

#### Problem 7 – Population Interpolation & Growth by Region

Using the **global population** dataset:

- Unpivot the wide year format (1980–2010) into a **long format** with columns: `Country`, `year`, `population`.  
- Use **window functions** to perform **linear interpolation** of missing population values per country.  
- Pivot selected decades (1980, 1990, 2000, 2010) back into columns.  
- Exclude aggregate regions (e.g., World, Africa, Europe, etc.) and keep only valid countries.  
- Assign each country to a **region** (e.g., Asia, Americas, Africa, Europe) based on predefined mappings.  
- Compute **population growth rates** and **regional average growth** (population-weighted).  
- Output a table showing for each country:
  - `Region`, `Country`, population in each decade, and regional average growth rate.

---

### Environment & Requirements

- **Language**: Python 3  
- **Framework**: PySpark  
- **Environment**: Jupyter Notebook  
- **Libraries**:
  - `pyspark.sql` (DataFrame, functions, Window)  
  - Standard Python libraries (`os`, `itertools`, etc.)  
---

### How to Run

1. Start **Jupyter Notebook** in your environment with Spark configured.  
2. Open the assignment notebook.  
3. Ensure the dataset paths in the notebook match your local setup.  
4. Run all cells in order:
   - Spark session setup  
   - Data loading  
   - Problems 1–7 (in sequence)  
5. Verify that each problem produces the required outputs (tables, pivot summaries, and rankings).

---

### Delivered

- **Completed Jupyter notebook** with:
  - All code cells executed successfully  
  - Clear labeling of each problem (Problem 1–7)  
- **Generated outputs** (tables and summaries) inline in the notebook.  


