# Time Intelligence Lab


**When to do this:** After completing Dashboard 1 (Connecting to Snowflake) — before the Challenge

---

## What Is Time Intelligence?

Time intelligence is a family of DAX functions that let you calculate values relative to time — "revenue this year vs. last year", "running total through today", "growth since the same month last year". These are among the most requested features in any business dashboard.

Power BI has built-in time intelligence functions, but they require one thing first: **a proper Date table** (also called a Calendar table). Without it, Power BI can't know how dates relate to each other — it can't identify "the previous month" or "the start of the fiscal year" without a structured date reference.

---

## Part 1: Create a Date Table

You'll create a Date table using a single DAX formula. It will generate one row per day covering the full range of your data.

### Step 1A: Create the table

1. Go to **Modeling** tab → **New Table**
2. In the formula bar, type:

```dax
DATE = CALENDARAUTO()
```

3. Press **Enter**

> **What `CALENDARAUTO()` does:** It scans every date column in your entire data model and automatically generates a continuous list of dates covering the full date range — from the earliest date found to the latest. For the Chinook data, this covers **2021 through 2025**.

> **📋 Checkpoint:** Switch to **Table View** and select the `Date` table. You should see a single column named `Date` with one row per day from January 1, 2021 through December 31, 2025 — roughly 1,826 rows. If it shows a very short date range or only a few rows, check that INVOICE[INVOICE_DATE] is formatted as a Date column (not text).

---

### Step 1B: Add useful columns to the Date table

A single date column isn't enough — you need Year, Month, Quarter, etc. for slicers and chart axes. Add these calculated columns to the `Date` table:

**Year:**
```dax
Year = YEAR('Date'[Date])
```

**Month Number** (for sorting):
```dax
Month Number = MONTH('Date'[Date])
```

**Month Name:**
```dax
Month Name = FORMAT('Date'[Date], "MMMM")
```

**Quarter:**
```dax
Quarter = "Q" & QUARTER('Date'[Date])
```

**Year-Month** (useful for line chart axes):
```dax
Year-Month = FORMAT('Date'[Date], "YYYY-MM")
```

> **📋 Checkpoint:** In Table View, the `Date` table should now show 6 columns: Date, Year, Month Number, Month Name, Quarter, Year-Month.
>
> **Fix Month Name sorting:** Month names will sort alphabetically (April before January) by default. To fix this:
> 1. Click `Month Name` column header in Table View
> 2. **Column Tools** tab → **Sort by Column** → select `Month Number`
> Now months will sort January → December correctly on charts.

---

### Step 1C: Connect the Date table to your model

1. Go to **Model View**
2. Drag `Date[Date]` → `INVOICE[INVOICE_DATE]`
3. Verify the relationship: **1** (Date) → **\*** (INVOICE) with single-direction cross-filter

> **📋 Checkpoint — Model View:** You should now see a 4th relationship line connecting `Date` to `INVOICE`. The `Date` table is your hub for time-based filtering — when a user picks a year in a slicer, the filter travels through this relationship to INVOICE, then to INVOICE_LINE via the existing relationship.

---

## Part 2: Time Intelligence Measures

Now that the Date table is connected, DAX's time intelligence functions will work. Create these measures in the `INVOICE_LINE` table (or in a dedicated `_Measures` table if you prefer to keep them organized).

### Measure 1: Revenue YTD (Year-to-Date)

Year-to-date revenue resets to zero at the start of each year and accumulates through the current date.

```dax
Revenue YTD = TOTALYTD(
    SUM(INVOICE_LINE[Revenue]),
    'Date'[Date]
)
```

> **What this does:** For any date context (a month, a quarter, a day), it sums revenue from January 1 of that year through that point in time.

---

### Measure 2: Revenue Last Year

```dax
Revenue Last Year = CALCULATE(
    SUM(INVOICE_LINE[Revenue]),
    SAMEPERIODLASTYEAR('Date'[Date])
)
```

> **What this does:** Takes whatever date period is in the current filter context (e.g., "2025") and shifts it back exactly 12 months (to "2024"), then computes revenue for that shifted period.

---

### Measure 3: Year-over-Year Growth %

```dax
Revenue YoY Growth % = DIVIDE(
    SUM(INVOICE_LINE[Revenue]) - [Revenue Last Year],
    [Revenue Last Year],
    BLANK()
)
```

> **What this does:** Computes the percentage change between this period and the same period last year. Returns BLANK() (shows nothing) when there's no prior year data to compare against — cleaner than showing 0% or an error.

---

### Measure 4: Revenue MTD (Month-to-Date)

```dax
Revenue MTD = TOTALMTD(
    SUM(INVOICE_LINE[Revenue]),
    'Date'[Date]
)
```

---

## Part 3: Build a Time Intelligence Page

Add a new report page to your Dashboard 1 file. Name it **"Revenue Trends"**.

### Visual 1: Revenue trend line chart with YTD overlay

1. Add a **Line Chart** visual
2. Fields:
   - `Date[Year-Month]` → **X-axis**
   - `Total Revenue` → **Y-axis** (Line 1)
   - `Revenue YTD` → **Y-axis** (Line 2 — drag a second measure)
3. Format:
   - Title: `Monthly Revenue vs. YTD`
   - Line 1 (Total Revenue): solid, primary color
   - Line 2 (Revenue YTD): dashed, secondary color

> **📋 Checkpoint:** You should see two lines. The monthly revenue line goes up and down each month. The YTD line always goes up (it accumulates) and resets at the start of each new year (you'll see it dip back down at Jan 2022, Jan 2023, etc.).

---

### Visual 2: Year-over-Year comparison bar chart

1. Add a **Clustered Bar Chart**
2. Fields:
   - `Date[Year]` → **Y-axis**
   - `Total Revenue` → **X-axis** (bar 1)
   - `Revenue Last Year` → **X-axis** (bar 2)
3. Format:
   - Title: `Revenue: This Year vs. Last Year`
   - Note: 2021 will show no "Last Year" bar (no 2020 data)

> **📋 Checkpoint:** Each year from 2021–2025 should have two bars (except 2021 which has only one). The bars should be similar in length since Chinook's revenue was fairly stable year-to-year.

---

### Visual 3: YoY Growth % by year

1. Add a **Clustered Column Chart**
2. Fields:
   - `Date[Year]` → **X-axis**
   - `Revenue YoY Growth %` → **Y-axis**
3. Format:
   - Title: `Year-over-Year Revenue Growth`
   - Format Y-axis as **Percentage**
   - Add a constant line at Y = 0 (Format → Analytics → Constant line → Value: 0)

> **📋 Checkpoint:** 2021 should show blank/no bar (no prior year). 2022–2025 should show small positive or negative growth percentages. Chinook's data is relatively flat so growth rates will be small (single-digit %).

---

### Visual 4: Year slicer

1. Add a **Slicer** visual
2. Drag `Date[Year]` → Field
3. Format: **Dropdown** or **List** style

> **Test interaction:** Select just **2023** in the slicer. All three charts should update to show only 2023 data. The YTD line should run from Jan 2023 to Dec 2023 and accumulate through that year only.

---

## Part 4: Key Concepts Summary

| Function | What it does | When to use it |
|---|---|---|
| `CALENDARAUTO()` | Generates a complete date table from your data | Always — create this first in any model with dates |
| `TOTALYTD()` | Running total from Jan 1 to current date in context | YTD KPIs, cumulative trend lines |
| `TOTALMTD()` | Running total from the 1st of the current month | MTD dashboards, daily tracking |
| `SAMEPERIODLASTYEAR()` | Shifts the date filter back 12 months | Year-over-year comparisons |
| `DATEADD()` | Shifts a date by any number of periods | Custom comparisons (e.g., 3 months ago) |

---

## Why the Date Table Is Required

You might wonder: why not just use `INVOICE[INVOICE_DATE]` directly?

The answer is **contiguity**. Time intelligence functions need a continuous, gap-free list of dates to work correctly. If your transaction data has a day with no sales (which Chinook does — weekends, for example), there's a gap in the date sequence. DAX can't correctly compute "the start of the year" or "the same period last year" over a gappy sequence.

The Date table is always continuous (every day is present), so DAX always has a clean calendar to navigate. The `INVOICE_DATE` column just connects transactions to their place in that calendar.

**Best practice:** Every Power BI model with date-based analysis should have exactly one Date table, connected to all fact tables that have dates.

---

## What You Learned

| Concept | Skill gained |
|---|---|
| `CALENDARAUTO()` | Generate a Date table automatically from your data range |
| Date table columns | Year, Month, Quarter helpers for axes and slicers |
| `TOTALYTD` | Year-to-date cumulative revenue |
| `SAMEPERIODLASTYEAR` | Prior-year comparison in any context |
| YoY Growth % | Percentage change measure using DIVIDE |
| Date relationship | Connecting the Date table to fact tables in the model |
