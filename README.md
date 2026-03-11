# Power BI Desktop Training — 2-Day Course

A hands-on, project-based 2-day course for learning Microsoft Power BI Desktop. Students build real dashboards from raw data using local CSV files (Day 1) and a cloud Snowflake database (Day 2).

> **Note:** This course uses **Power BI Desktop** throughout. A Microsoft 365 or Power BI Pro account is not required.

---

## Course Philosophy

This course teaches Power BI as a tool — but the real goal is to develop **analytical thinking**. Knowing how to click through a BI tool is a commodity skill. Knowing *what* to build, *why* to build it, and *which visualization* best communicates your insight — that's what makes an analyst valuable.

**What we believe:**
- The question always comes before the chart. A dashboard built without a business question is just decoration.
- Chart choice is not a matter of preference — different relationships in data require different visual encodings. Using the wrong chart obscures insight.
- Every number needs a comparison. Raw totals mean nothing without a benchmark, a trend, or a target.
- Data storytelling is a skill. The best analysts don't just find insights — they communicate them so clearly that action follows.

**Where Power BI fits in the analytics landscape:** Power BI is primarily a tool for *descriptive* and *diagnostic* analytics — answering "what happened?" and "why did it happen?" These two types cover the vast majority of real-world business analytics needs. Understanding where the tool fits helps you know both what it's excellent at and where its limits are.

> 📖 **Before starting Day 1**, read [`Analytics Foundations.md`](Analytics%20Foundations.md) — it covers the 4 types of analytics, the Visual Vocabulary framework for choosing charts, and the principles of data storytelling. These concepts will inform every design decision you make during the course.

---

## Course Map

### Day 1 — Foundations with Local Data

| Activity | File | Description |
|---|---|---|
| Main guide | `Day 1/Power BI Desktop Day 1.md` | Full step-by-step instructions |
| Datasets | `Day 1/datasets/` | 6 CSV files (NY property, e-commerce 2017–2019, products, stores) |
| Challenge mockup | `Day 1/challenge-mockup.html` | Target dashboard for the end-of-day challenge |
| Solution | `Day 1/solution/` | Reference `.pbix` file |

**What students build:**
- Part 1: NY Property Sales — first visuals, basic interactions
- Part 2: E-Commerce Sales Dashboard — Power Query cleaning, Append Queries (combining 3 years of data), star schema relationships, slicers, drill hierarchy
- End-of-day Challenge: Build a Sales Summary page independently from a mockup

**Key skills covered:** CSV import, Power Query transformations, Append Queries, data types, table relationships, column/bar/line charts, slicers, cross-filtering

---

### Day 2 — Cloud Data, DAX, and Advanced Features

Work through the files **in this order:**

| # | Activity | File | Est. Time |
|---|---|---|---|
| 1 | Connect to Snowflake | `Day 2/Connecting to Snowflake.md` | 60–75 min |
| 2 | DAX Fundamentals | `Day 2/DAX Fundamentals Lab.md` | 25–30 min |
| 3 | Time Intelligence | `Day 2/Time Intelligence Lab.md` | 25–30 min |
| 4 | Challenge | `Day 2/challenge/Challenge Brief.md` | 45–60 min |
| 5 | Advanced Features | `Day 2/Advanced Features Lab.md` | 20–25 min |

**Challenge resources (in `Day 2/challenge/`):**

| File | Purpose |
|---|---|
| `Challenge Brief.md` | Requirements — what to build and success criteria |
| `mockup.html` | Visual target — open in browser |
| `hints.md` | Step-by-step guidance with checkpoints (use only if stuck) |

**Key skills covered:** Snowflake connection, Import vs DirectQuery, star schema (7 tables), DAX measures vs. calculated columns, CALCULATE, filter context, DIVIDE, Date table, TOTALYTD, SAMEPERIODLASTYEAR, drill-through pages, bookmarks, Q&A natural language visual

---

## Prerequisites

- **Power BI Desktop** installed (free — [download here](https://powerbi.microsoft.com/desktop))
- **Snowflake account** with access to `CHINOOK_DB` (Day 2 only — provided by instructor)
- No prior Power BI experience required
- Basic comfort with spreadsheets is helpful

---

## Datasets

| Dataset | Location | Used in |
|---|---|---|
| `ny_property_sales.csv` | `Day 1/datasets/` | Day 1 Part 1 |
| `sales2017_raw.csv` | `Day 1/datasets/` | Day 1 Part 2 (messy data exercise) |
| `sales2018.csv` | `Day 1/datasets/` | Day 1 Part 2 (Append Queries) |
| `sales2019.csv` | `Day 1/datasets/` | Day 1 Part 2 (Append Queries) |
| `producthierarchy.csv` | `Day 1/datasets/` | Day 1 Part 2 (product dimension) |
| `store_cities.csv` | `Day 1/datasets/` | Day 1 Part 2 (store/geography dimension) |
| Chinook DB (Snowflake) | Cloud | Day 2 (all activities) |

---

## Key Learning Outcomes

By the end of this 2-day course, students will be able to:

- Import and clean data from CSV files and cloud databases
- Build star schema data models with proper relationships
- Use Power Query to transform, append, and reshape data
- Write DAX measures and calculated columns
- Apply time intelligence (YTD, year-over-year comparisons)
- Create professional reports: cards, charts, maps, tables, treemaps
- Add interactivity: slicers, cross-filtering, drill-through, bookmarks
- Use the Q&A natural language feature for exploratory analysis

---

## Reference Materials

- `Analytics Foundations.md` — **Start here.** The 4 types of analytics, visual vocabulary, chart selection, and data storytelling principles
- `DAX-Guide.md` — Comprehensive DAX reference with Chinook examples
- `Working With Power BI-Outline.pdf` — Original course outline
- `Working with PowerBI Slides.pdf` — Slide deck (storytelling, analytics types, BI importance)
- [Tableau Visual Vocabulary](https://www.tableau.com/solutions/gallery/visual-vocabulary) — The definitive reference for choosing the right chart type
- [Power BI Desktop Documentation](https://learn.microsoft.com/en-us/power-bi/fundamentals/desktop-what-is-desktop)
- [DAX Reference](https://learn.microsoft.com/en-us/dax/)
- [From Data to Viz](https://www.data-to-viz.com/) — Interactive decision tree for chart selection
- [Power BI Community](https://community.powerbi.com/)

---

*For questions or feedback, contact the instructor.*
